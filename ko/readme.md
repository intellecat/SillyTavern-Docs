---
route: /ko/
---

# SillyTavern이란 무엇인가요?

![SillyTavern - LLM Frontend for Power Users](/static/banner.png)

SillyTavern(줄여서 ST)은 텍스트 생성 LLM, 이미지 생성 엔진 및 TTS 음성 모델과 상호작용할 수 있는 로컬 설치형 사용자 인터페이스입니다. 우리의 목표는 사용자에게 LLM 프롬프트에 대한 최대한의 유틸리티와 제어권을 부여하는 것이며, 가파른 학습 곡선도 즐거움의 일부로 받아들입니다.

SillyTavern은 LLM 애호가들로 구성된 헌신적인 커뮤니티가 제공하는 열정 프로젝트이며 항상 무료이자 오픈소스로 제공될 것입니다. 2023년 2월 TavernAI 1.2.8의 포크로 시작된 SillyTavern은 현재 200명 이상의 기여자와 2년간의 독립적인 개발을 거쳐, 숙련된 AI 애호가들을 위한 선도적인 소프트웨어로 계속 기능하고 있습니다.

## 스크린샷

|   [![API Connection](/static/screenshot1.jpg)](/static/screenshot1.jpg)    |  [![Chat UI](/static/screenshot2.jpg)](/static/screenshot2.jpg)   |
|:--------------------------------------------------------------------------:|:-----------------------------------------------------------------:|
| [![Advanced Formatting](/static/screenshot3.jpg)](/static/screenshot3.jpg) | [![World Info](/static/screenshot4.jpg)](/static/screenshot4.jpg) |

## 설치 요구사항

하드웨어 요구사항은 최소한입니다: NodeJS 18 이상을 실행할 수 있는 모든 환경에서 실행됩니다. 로컬 머신에서 LLM 추론을 수행하려는 경우, 최소 6GB VRAM을 갖춘 3000 시리즈 NVIDIA 그래픽 카드를 권장합니다.

플랫폼별 설치 가이드를 따라주세요:

* [Windows](/Installation/Windows.md)
* [Linux and Mac](/Installation/LinuxMacOS.md)
* [Android](/Installation/Android.md)
* [Docker](/Installation/Docker.md)

## 브랜치

SillyTavern은 모든 사용자에게 원활한 경험을 보장하기 위해 두 가지 브랜치 시스템을 사용하여 개발되고 있습니다.

* `release` -🌟 **대부분의 사용자에게 권장됩니다.** 가장 안정적이고 권장되는 브랜치로, 주요 릴리스가 푸시될 때만 업데이트됩니다. 대다수 사용자에게 적합합니다. 일반적으로 월 1회 업데이트됩니다.
* `staging` - ⚠️ **일반 사용에는 권장되지 않습니다.** 이 브랜치는 최신 기능을 포함하지만, 언제든지 작동하지 않을 수 있으므로 주의해야 합니다. 파워 유저와 열성팬 전용입니다. 하루에 여러 번 업데이트됩니다.

## SillyTavern 외에 무엇이 필요한가요?

SillyTavern은 인터페이스일 뿐이므로, 추론을 제공할 LLM 백엔드에 대한 액세스가 필요합니다. 즉시 사용 가능한 채팅을 위해 AI Horde를 사용할 수 있습니다. 그 외에도 많은 로컬 및 클라우드 기반 LLM 백엔드를 지원합니다: OpenAI 호환 API, KoboldAI, Tabby 등 다양합니다. 지원되는 API에 대한 자세한 내용은 [API 연결](/Usage/API_Connections/index.md) 섹션에서 확인할 수 있습니다.

## 캐릭터 카드

SillyTavern은 "캐릭터 카드" 개념을 중심으로 구축되었습니다. 캐릭터 카드는 LLM의 동작을 설정하는 프롬프트 모음이며, SillyTavern에서 지속적인 대화를 나누기 위해 필요합니다. ChatGPT의 GPT나 Poe의 봇과 유사하게 작동합니다. 캐릭터 카드의 내용은 무엇이든 될 수 있습니다: 추상적인 시나리오, 특정 작업에 맞춤화된 어시스턴트, 유명 인물 또는 가상의 캐릭터.

캐릭터 카드를 선택하지 않고 빠른 대화를 나누거나 LLM 연결을 테스트하려면, SillyTavern을 연 후 [시작 화면](/Usage/welcome-assistants.md)의 입력 바에 프롬프트를 입력하기만 하면 됩니다. 이렇게 하면 나중에 사용자 정의할 수 있는 빈 "Assistant" 캐릭터 카드가 생성됩니다.

캐릭터 카드를 정의하는 방법에 대한 일반적인 아이디어를 얻으려면 기본 캐릭터(Seraphina)를 참조하거나 "Download Extensions & Assets" 메뉴에서 선별된 커뮤니티 제작 카드를 다운로드하세요.

처음부터 나만의 캐릭터 카드를 만들 수도 있습니다. 자세한 내용은 [캐릭터 디자인](/Usage/Characters/characterdesign.md) 가이드를 참조하세요.

## 주요 기능

* 많은 커뮤니티 제작 프리셋과 함께하는 고급 [텍스트 생성 설정](/Usage/Prompts/advancedformatting.md)
* [World Info 지원](Usage/worldinfo.md): 풍부한 세계관을 만들거나 캐릭터 카드의 토큰을 절약하세요
* [그룹 채팅](/Usage/Characters/groupchats.md): 캐릭터들이 당신과 또는 서로 대화할 수 있는 멀티봇 룸
* [풍부한 UI 커스터마이징 옵션](/Usage/User_Settings/uicustomization.md): 테마 색상, 배경 이미지, 사용자 정의 CSS 등
* [사용자 페르소나](/Usage/personas.md): AI에게 당신에 대해 알려 더 큰 몰입감을 제공하세요
* [내장 RAG 지원](/Usage/Characters/data-bank.md): AI가 참조할 문서를 채팅에 추가하세요
* 광범위한 [채팅 명령어](/Usage/Chatting/slashcommands.md) 하위 시스템과 자체 [스크립팅 엔진](/For_Contributors/st-script.md)

## 확장 기능

SillyTavern은 확장성을 지원합니다.

* [캐릭터 감정 표현 (스프라이트)](/extensions/Expression-Images.md)
* [채팅 기록 자동 요약](/extensions/Summarize.md)
* 자동 UI 및 [채팅 번역](extensions/Translation.md)
* [Stable Diffusion/FLUX/DALL-E 이미지 생성](/extensions/Stable-Diffusion.md)
* [AI 응답 메시지를 위한 텍스트 음성 변환 (ElevenLabs, Silero 또는 OS의 시스템 TTS를 통해)](/extensions/TTS.md)
* [프롬프트에 추가 실제 세계 컨텍스트를 추가하기 위한 웹 검색 기능](/extensions/WebSearch.md)
* "Download Extensions & Assets" 메뉴에서 더 많은 확장 기능을 다운로드할 수 있습니다.

## 개발자와 직접 연락하려면 어떻게 해야 하나요?

* Discord: cohee, rossascends, wolfsblvt
* Reddit: [/u/RossAscends](https://www.reddit.com/user/RossAscends/), [/u/sillylossy](https://www.reddit.com/user/sillylossy/), [u/Wolfsblvt](https://www.reddit.com/user/Wolfsblvt/)
* [GitHub 이슈 게시](https://github.com/SillyTavern/SillyTavern/issues)

## 프로젝트가 마음에 듭니다! 어떻게 기여할 수 있나요?

* 풀 리퀘스트를 환영합니다! 시작하려면 [기여 가이드라인](https://github.com/SillyTavern/SillyTavern/blob/release/CONTRIBUTING.md)을 따라주세요.
* GitHub에서 제공되는 템플릿을 사용하는 유용하고 정보가 풍부한 버그 보고서도 환영합니다.
* 프로젝트 자체에 대한 금전적 기부는 받지 않습니다.

## 개인 후원

개별 기여자에 대한 여러분의 지원은 감사하지만, 이것이 SillyTavern의 전반적인 개발 방향에 영향을 미치지는 않습니다.

* RossAscends는 개인 [Patreon](https://www.patreon.com/RossAscends) 및 [Kofi](https://ko-fi.com/rossascends)를 운영하고 있습니다

## 라이선스

SillyTavern은 [AGPL-3.0 라이선스](https://github.com/SillyTavern/SillyTavern/blob/release/LICENSE)로 배포되는 무료 오픈소스 프로젝트입니다.
