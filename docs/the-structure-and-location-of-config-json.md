---
title: The Structure and Location of config.json
---
# config.json 구조 및 위치 설명

# 단어 설명

현재 config.json은 크게 3가지가 존재하며 각 파일은 모두 같은 구조를 가지나 그 역할이 서로 조금씩 상이합니다. 본 문서에서는 이러한 config.json을 다음과 같이 정의합니다.

- **원격 설정 (remote config):** Nine Chronicles S3를 통해 HTTP로 전송되는 config.json 파일
- **로컬 설정 (local config):** 사용자가 런처에 설치하고 사용하는 config.json 파일.
    - `%LOCALAPPDATA%\Programs\Nine Chronicles\resources\app`
- **사용자 설정 (user config):** 각 사용자가 런처의 설정 화면을 통해 설정하고 런처의 버전에 관계 없이 서로 공유되는 config.json 파일.
    - `%APPDATA%\Nine Chronicles\config.json`
    - (타 네트워크의 경우) `%APPDATA%\Nine Chronicles\config.{Network}.json`
    - (macOS의 경우) `~/Library/Application Support/Nine Chronicles/config.json`

## 원격 설정의 설치 및 적용

원격 설정은 현재 [`https://download.nine-chronicles.com/9c-launcher-config.json`](https://download.nine-chronicles.com/9c-launcher-config.json) 에 저장되어 각 사용자에게 HTTP를 통해 임의로 배포 가능한 config.json입니다.

아래의 조건을 **모두** 만족하는 경우, 사용자의 로컬 설정은 원격 설정으로 덮어씌워집니다.

- 원격 설정과 로컬 설정의 APV가 일치하는 경우
- 원격 설정의 ConfigVersion이 로컬 설정보다 더 높은 경우

따라서 인터널 테스트와 같은 경우를 제외한 최신 버전의 사용자 배포판은 원격 설정이 적용되었을 것이라 가정할 수 있습니다.

## 사용자 설정의 덮어씌움

사용자 설정은 타 config.json과 같은 구조를 공유하나 사용자가 상황에 맞추어 직접 설정할 수 있는 값들 (주로 언어나 블럭체인 데이터 폴더 따위)를 다룹니다. 이러한 설정이 사용자 설정에 존재하는 경우 런처는 설정을 읽어올 때 사용자 설정에 있는 키의 값을 우선합니다. (항목의 값이 배열이거나 객체여도 내용물이 합쳐지는 것과 같은 동작 없이 배열이나 객체를 통째로 덮어씁니다.)

v100087부터는 이러한 구현 특성으로 인해 인터널 테스트 도중 블럭체인 데이터가 섞이는 것을 방지하기 위해, [로컬 설정의 Network 키로 어떤 사용자 설정 파일을 사용할지를 구분합니다](https://github.com/planetarium/9c-launcher/pull/1060). 메인넷의 Network 값인 `9c-main` 으로 설정된 상태에서는 config.json이 그대로 읽히지만 `9c-internal` 로 설정된 경우 config.9c-internal.json을 읽어 이러한 충돌을 방지합니다.

# 필드 구성

[9c-launcher/config.ts at fbb99228b61d6ca4140f42a6a597e34e208d9f08 · planetarium/9c-launcher](https://github.com/planetarium/9c-launcher/blob/fbb99228b61d6ca4140f42a6a597e34e208d9f08/src/config.ts#L11)

<aside>
💡 기본값이나 현재 mainnet 설정도 있으면 도움이 될 수 있을 것 같습니다.

</aside>

[config.json](https://www.notion.so/2ca6d4fdeede48cfa897377928b864ef)

<aside>
⏰ @Basix config.json에 들어가는 값 설명 채우기

</aside>
