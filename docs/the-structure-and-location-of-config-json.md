---
title: [LAUNCHER] The Structure and Location of config.json
---

# The Structure and Location of config.json

# File Definition

Currently, there are 3 seperate "config.json"s, and each file has the same structure, but its role is slightly different. This document defines these config.json as follows:
- **remote config:** config.json file that downloaded over HTTP from Nine Chronicles S3. 
- **local config:** config.json file that user-installed on launcher and using.
    - `%LOCALAPPDATA%\Programs\Nine Chronicles\resources\app`
- **user config:** config.json file that are set by each user through the launcher's settings screen and  regardless of the launcher's version.
    - `%APPDATA%\Nine Chronicles\config.json`
    - (on different network (ex. internal, previewnet)) `%APPDATA%\Nine Chronicles\config.{Network}.json`
    - (on macOS) `~/Library/Application Support/Nine Chronicles/config.json`
    - (on Linux) `~/.config/Nine Chronicles/config.json`

## Installation and Application of remote config

Currently, remote config is served on [`https://download.nine-chronicles.com/9c-launcher-config.json`](https://download.nine-chronicles.com/9c-launcher-config.json), and can be distributed to each user over HTTP.

If **All** following conditions are met, the user's local config will be overwritten wit the remote settings.

- APV of the remote and local config matches
- ConfigVersion of remote config is higher than local config

Therefore, you can assume that the latest version of the user distribution has remote config applied, except for the case like an internal test.
## Overwrite of user config

User config share the same structure as other config.json, but deal with values that users can set themselves (mainly language or blockchain data folder path). If such a setting exists in a user config, the launcher overrides the value of the key in the user config over any config.json when reading the config.json (even if the value of the item is an array or an object, it overwrites the array or object entirely without merging)


from v100087, due to these implementation characteristics, [We distinguish which user config file to used with the "Network" key in the name of user config](https://github.com/planetarium/9c-launcher/pull/1060) To prevent the blockchain data from being mixed with different networks. When the network value is set to main network, which is '9c-main', config.json is read as it is, but if it is set to '9c-previewnet', `config.9c-previewnet.json` is read to avoid this conflict.

# Field Configuration.

[9c-launcher/config.ts at fbb99228b61d6ca4140f42a6a597e34e208d9f08 · planetarium/9c-launcher](https://github.com/planetarium/9c-launcher/blob/fbb99228b61d6ca4140f42a6a597e34e208d9f08/src/config.ts#L11)

|Field                           |Data Type    |Required|Deprecated|Usage                        |
|--------------------------------|-------------|--------|----------|-----------------------------|
|AppProtocolVersion              |string       |Yes     |No        |Headless, Launcher           |
|AwsRegion                       |string       |No      |No        |Headless                     |
|AwsSecretKey                    |string       |No      |No        |Headless                     |
|BlockChainStoreDirName          |string       |No      |No        |Headless, Launcher           |
|BlockChainStoreDirParent        |string       |No      |No        |Headless, Launcher           |
|ConfigVersion                   |number       |Yes     |No        |Launcher                     |
|Confirmations                   |number       |Yes     |No        |Headless, Libplanet          |
|DataProviderUrl                 |string       |No      |No        |                             |
|GenesisBlockPath                |string       |Yes     |No        |Headless, Launcher, Libplanet|
|HeadlessArgs                    |array\<string\>|No      |No        |Headless                     |
|IceServerSettings               |             |No      |No        |                             |
|LaunchPlayer                    |boolean      |No      |No        |Launcher                     |
|Locale                          |string       |No      |No        |Launcher                     |
|LogSizeBytes                    |number       |No      |No        |Launcher                     |
|Mixpanel                        |boolean      |No      |No        |Launcher, NineChronicles     |
|MuteTeaser                      |             |No      |Yes       |Launcher                     |
|Network                         |string       |No      |No        |                             |
|NoMiner                         |boolean      |No      |No        |Headless, Launcher           |
|NoTrustedStateValidators        |             |No      |No        |                             |
|PeerStrings                     |array\<string\>|No      |No        |Headless                     |
|RemoteNodeList                  |array\<string\>|No      |No        |Launcher                     |
|Sentry                          |boolean      |No      |No        |Launcher                     |
|SnapshotPaths                   |array\<string\>|No      |No        |Launcher                     |
|SnapshotThreshold               |             |No      |No        |                             |
|StoreType                       |             |No      |No        |                             |
|SwapAddress                     |             |No      |No        |                             |
|TrustedAppProtocolVersionSigners|array\<string\>|No      |No        |Headless                     |
|UseRemoteHeadless               |boolean      |No      |No        |Launcher                     |
|UseV2Interface                  |boolean      |No      |Yes       |Launcher                     |
|Workers                         |             |No      |No        |                             |
