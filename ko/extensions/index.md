---
label: 확장 기능
icon: plug
expanded: true
order: 35
route: /ko/extensions/
---

# 확장 기능

SillyTavern은 확장 기능 패널에서 활성화하거나 비활성화할 수 있는 많은 확장 기능을 제공합니다. 확장 기능은 새로운 기능을 추가하거나, 기존 기능의 동작을 변경하거나, AI가 사용할 추가 콘텐츠를 제공할 수 있습니다. 확장 기능 패널의 "Download Extensions & Assets" 메뉴에서 더 많은 확장 기능을 설치할 수 있습니다.

## 확장 기능 패널

확장 기능 패널을 열거나 닫으려면 상단 바에서 **<i class="fa-solid fa-cubes fa-fw"></i> Extensions**를 선택하세요.

- **<i class="fa-solid fa-cubes"></i> Manage extensions**: 확장 기능 활성화, 비활성화 및 업데이트
- **Download Extensions & Assets**: SillyTavern 리포지토리에서 [추가 확장 기능](#installable-extensions), 캐릭터, 사운드 및 배경 설치
- **Notify on extension updates**: 설치된 확장 기능에 사용 가능한 업데이트가 있을 때 알림을 받으려면 체크
- **<i class="fa-solid fa-cloud-arrow-down"></i> Install extension**: Git 리포지토리 URL에서 [타사 확장 기능](#third-party-extensions) 가져오기

## 내장 확장 기능

이러한 확장 기능은 SillyTavern에 내장되어 있으며 별도로 설치할 필요가 없습니다. 확장 기능 패널에서 활성화하거나 비활성화할 수 있습니다.

:::callout
**[Chat Translation](Translation.md)**

채팅 메시지를 다른 언어로 번역
:::

:::callout
**[Image Captioning](captioning.md)**

이미지에서 텍스트를 생성하여 AI가 대화에서 시각적 콘텐츠를 "보고" 응답할 수 있도록 함
:::

:::callout
**[Image Generation](Stable-Diffusion.md)**

로컬 또는 클라우드 기반 Stable Diffusion, FLUX 또는 DALL-E API를 사용하여 이미지 생성
:::

:::callout
**[Expression Images](Expression-Images.md)**

채팅 창 옆이나 뒤에 표시되는 AI 캐릭터의 이미지 (일명 '스프라이트')
:::

:::callout
**[Summarize](Summarize.md)**

채팅 기록 자동 요약
:::

:::callout
**[Chat Vectorization](Chat-vectorization.md)**

채팅 기록에서 관련 메시지를 찾아 컨텍스트에 추가
:::

:::callout
**[Text To Speech](TTS.md)**

ElevenLabs, Silero, 시스템 TTS, **[AllTalk](AllTalk.md)**, **[XTTS](XTTS.md)** 등을 통한 채팅 메시지 음성 낭독
:::

:::callout
**[Quick Reply](/For_Contributors/st-script.md#quick-replies-script-library-and-auto-execution)**

한 번의 클릭으로 채팅 메시지에 응답하고, 명령어 및 STscript 실행 등
:::

:::callout
**Token Counter**

텍스트를 토큰으로 변환하고 토큰 수 계산
:::

---

## 설치 가능한 확장 기능

!!!tip
확장 기능을 다운로드하려면 git이 **반드시** 설치되어 있어야 합니다. git이 설치되어 있지 않다면 [Git 설치 페이지](https://git-scm.com/downloads)의 지침을 따르세요.
!!!

**<i class="fa-solid fa-cubes"></i> Extensions** => **Download Extensions & Assets** 메뉴로 이동하여 **<i class="fa-solid fa-plug-circle-exclamation"></i> Load Asset List** 버튼을 클릭하면 앱에서 직접 사용 가능한 모든 확장 기능 목록을 탐색할 수 있습니다. 확장 기능을 설치하려면 **<i class="fa-solid fa-download"></i> Download** 버튼을 클릭하세요. 확장 기능에 대해 자세히 알아보려면 이름 옆의 **<i class="fa-solid fa-arrow-up-right-from-square"></i> Link** 버튼을 클릭하여 GitHub 페이지를 여세요.

!!!info Extensions는 Extras가 아닙니다
Extras 프로젝트는 2024년 4월에 중단되었습니다. 확장 기능을 사용하기 위해 Extras를 설치할 필요가 없습니다.
!!!

:::callout
**[Blip](Blip.md)**

가변 속도로 캐릭터 메시지 텍스트에 애니메이션을 적용하고 애니메이션과 함께 사운드를 재생합니다.
:::

:::callout
**[Dynamic Audio](Dynamic-Audio.md)**

채팅에 몰입감 있는 배경 음악과 주변 소리를 추가합니다.
:::

:::callout
**[EmulatorJS](EmulatorJS.md)**

SillyTavern 채팅에서 직접 레트로 콘솔 게임을 플레이합니다.
:::

:::callout
**[Live2d](Live2d.md)**

live2d 모델 지원을 추가합니다. 커스터마이징 가능한 표정, 애니메이션 및 상호작용.
:::

:::callout
**[Objective](Objective.md)**

채팅 중에 AI가 목표를 향해 나아갈 수 있도록 목표를 설정합니다.
:::

:::callout
**[RVC](RVC.md)**

Text-to-Speech 모듈에 실시간 음성 복제 기능을 추가합니다.
:::

:::callout
**[Speech Recognition](Speech-Recognition.md)**

브라우저 또는 extras를 사용하여 음성을 텍스트로 변환합니다.
:::

:::callout
**[VRM](VRM.md)**

VRM 모델 지원을 추가합니다. 커스터마이징 가능한 표정, 애니메이션 및 상호작용.
:::

:::callout
**[Web Search](WebSearch.md)**

LLM 프롬프트에 웹 검색 결과를 추가합니다.
:::

:::callout
**[Weather](https://github.com/SillyTavern/Extension-Weather)**

슬래시 명령어 또는 함수 도구로 사용 가능한 날씨 API 중 하나를 사용하여 날씨 정보를 제공합니다.
:::

:::callout
**[Chat Top Bar](https://github.com/SillyTavern/Extension-TopInfoBar)**

빠른 작업 바로가기가 있는 상단 바를 채팅 창에 추가합니다.
:::

:::callout
**[Chess](https://github.com/SillyTavern/SillyTavern-Chess)**

LLM과 체스 게임을 플레이합니다.
:::

:::callout
**[Code Runner](https://github.com/SillyTavern/Extension-CodeRunner)**

채팅의 코드 블록에서 JavaScript 및 STscript 코드를 실행할 수 있습니다.
:::

:::callout
**[D&D Dice](https://github.com/SillyTavern/Extension-Dice)**

모든 주사위 굴림 요구사항을 위한 7개의 클래식 D&D 주사위 세트.
:::

:::callout
**[Duplicate Finder](https://github.com/SillyTavern/Extension-DupeFinder)**

유사성 그룹별로 캐릭터를 클러스터링하여 중복을 쉽게 찾을 수 있는 기능을 추가합니다.
:::

:::callout
**[Emoji Picker](https://github.com/SillyTavern/Extension-EmojiPicker)**

채팅 메시지에 이모지를 빠르게 삽입할 수 있는 버튼을 추가합니다.
:::

:::callout
**[Group Greetings](https://github.com/SillyTavern/Extension-GroupGreetings)**

그룹 채팅 전용 대체 인사말을 설정할 수 있습니다.
:::

:::callout
**[Group SendAs](https://github.com/SillyTavern/SillyTavern-GroupSendAs)**

선택한 그룹 멤버에 대한 /sendas 명령어 템플릿을 빠르게 삽입할 수 있는 버튼을 추가합니다.
:::

:::callout
**[HypeBot](https://github.com/SillyTavern/Extension-HypeBot)**

NovelAI의 HypeBot 엔진을 사용하여 최근 채팅을 기반으로 개인화된 제안을 표시합니다. 활성화된 NovelAI 구독이 필요합니다.
:::

:::callout
**[Idle](https://github.com/SillyTavern/Extension-Idle)**

사용자가 일정 시간 동안 유휴 상태일 때 "유휴 프롬프트"를 추가하여 대화를 자연스럽게 계속합니다.
:::

:::callout
**[Image Metadata Viewer](https://github.com/SillyTavern/Extension-ImageMetadataViewer)**

채팅에 첨부된 확대된 이미지의 메타데이터를 봅니다.
:::

:::callout
**[LaTeX](https://github.com/SillyTavern/Extension-LaTeX)**

채팅 메시지에서 LaTeX 및 AsciiMath 수식을 렌더링합니다.
:::

:::callout
**[Mermaid](https://github.com/SillyTavern/Extension-Mermaid)**

SillyTavern 채팅에 Mermaid 다이어그램 및 순서도 렌더링을 추가합니다.
:::

:::callout
**[Notebook](https://github.com/SillyTavern/Extension-Notebook)**

메모를 저장할 장소를 추가합니다. 서식 있는 텍스트 형식을 지원합니다.
:::

:::callout
**[Parameter Randomizer](https://github.com/SillyTavern/Extension-Randomizer)**

생성할 때마다 API 설정 슬라이더를 무작위로 지정할 수 있는 기능을 추가합니다.
:::

:::callout
**[Prome Visual Novel Extension](https://github.com/Bronya-Rand/Prome-VN-Extension)**

더 많은 기능(포커스 모드, 레터박스 모드 등)으로 현재 비주얼 노벨 경험을 향상시킵니다!
:::

:::callout
**[Prompt Inspector](https://github.com/SillyTavern/Extension-PromptInspector)**

서버로 보내기 전에 출력 프롬프트를 검사하고 편집할 수 있는 옵션을 추가합니다.
:::

:::callout
**[Push Notifications](https://github.com/SillyTavern/SillyTavern-PushNotifications)**

수신 채팅 메시지에 대한 푸시 알림을 받을 수 있습니다.
:::

:::callout
**[Quick Persona](https://github.com/SillyTavern/Extension-QuickPersona)**

채팅 바에서 사용자 페르소나를 선택할 수 있는 드롭다운 메뉴를 추가합니다.
:::

:::callout
**[RSS](https://github.com/SillyTavern/Extension-RSS)**

슬래시 명령어 또는 함수 도구로 RSS 피드에서 최신 뉴스를 가져옵니다.
:::

:::callout
**[Screen Share](https://github.com/SillyTavern/Extension-ScreenShare)**

메시지를 보낼 때 멀티모달 모델에 화면 이미지를 제공합니다.
:::

:::callout
**[Silence Player](https://github.com/SillyTavern/Extension-Silence)**

확장 기능 메뉴에 무음 오디오 플레이어를 추가합니다. 브라우저 탭이 백그라운드에서 종료되는 것을 방지하는 데 도움이 될 수 있습니다.
:::

:::callout
**[Timelines](https://github.com/SillyTavern/SillyTavern-Timelines)**

채팅 기록에 타임라인 내비게이션을 추가합니다.
:::

:::callout
**[Variable Viewer](https://github.com/LenAnderson/SillyTavern-Variable-Viewer)**

변수를 보고 수정하는 쉬운 방법.
:::

:::callout
**[WebLLM](https://github.com/SillyTavern/Extension-WebLLM)**

확장 기능이 브라우저에서 직접 언어 모델을 사용할 수 있는 인터페이스를 제공합니다.
:::

## 타사 확장 기능

!!!danger
타사 확장 기능을 사용하면 의도하지 않은 부작용이 발생하고 보안 위험이 있을 수 있습니다.
**<i class="fa-solid fa-cloud-arrow-down"></i> Install extension**을 통해 확장 기능을 가져오기 전에 항상 소스를 신뢰하는지 확인하세요.
타사 확장 기능으로 인한 손상에 대해서는 책임지지 않습니다.
!!!

타사 확장 기능을 설치하려면 **<i class="fa-solid fa-cubes"></i> Extensions** => **<i class="fa-solid fa-cloud-arrow-down"></i> Install Extension** 메뉴로 이동하여 확장 기능 리포지토리의 URL을 붙여넣으세요. 선택적으로 브랜치와 ([다중 사용자](../Administration/multi-user.md) 시나리오에서) 설치 대상을 지정할 수 있습니다: 모든 사용자 또는 현재 사용자만. 확장 기능이 자동으로 다운로드되고 로드됩니다.
