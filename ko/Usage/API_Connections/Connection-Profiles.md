---
icon: paperclip
route: /ko/usage/core-concepts/connection-profiles/
order: 100
label: 연결 프로필
title: 연결 프로필
---

# 연결 프로필

연결 프로필을 저장하여 다양한 API, 모델 및 형식 템플릿 간에 빠르게 전환할 수 있습니다. 여러 API 연결을 적극적으로 사용하거나 메뉴를 탐색하지 않고 다양한 구성 간에 전환해야 할 때 유용합니다.

## 연결 프로필 액세스

이 기능은 SillyTavern 1.12.6 이상부터 내장 확장 기능으로 기본적으로 활성화되어 있으며 API 연결 메뉴에서 사용할 수 있습니다. *비활성화*하려면 확장 기능 패널을 열고 "확장 기능 관리"를 클릭한 다음 목록에서 연결 프로필을 찾아 "Enabled" 확인란을 선택 취소한 다음 "Close"를 클릭하세요.

## 저장되는 내용

연결 프로필은 다음 선택 사항을 저장합니다.

### 공통

* [API 유형, 모델 및 서버 URL](/Usage/API_Connections/index.md)
* [Secret Key](/Usage/faq.md#where-are-my-api-keys-stored-why-cant-i-see-them)
* [설정 프리셋](/Usage/Common-Settings.md)
* [Start Reply With](/Usage/Prompts/advancedformatting.md#start-reply-with) (명시적으로 비어 있을 수 있음)
* [사용자 정의 중지 문자열](/Usage/Prompts/advancedformatting.md#custom-stopping-strings) (명시적으로 비어 있을 수 있음)
* [추론 형식](/Usage/Prompts/reasoning.md#configuration)

### Text Completion API

* [시스템 프롬프트 및 그 상태](/Usage/Prompts/advancedformatting.md#system-prompt)
* [Instruct Mode 상태 및 템플릿](/Usage/Prompts/instructmode.md)
* [컨텍스트 템플릿](/Usage/Prompts/advancedformatting.md#context-template)
* [토크나이저](/Usage/Prompts/advancedformatting.md#tokenizer)

### Chat Completion API

* [프롬프트 후처리](/Usage/API_Connections/openai.md#prompt-post-processing)
* 프록시 프리셋

## 연결 프로필 관리

!!!info Note
프로필은 기본 설정에 대해 아무것도 모른 채 드롭다운 필드의 선택만 저장합니다. 즉, 다른 프로필로 전환하면 저장되지 않은 변경 사항이 손실됩니다. 이를 방지하려면 임시 변경 사항을 잃고 싶지 않다면 모든 프리셋과 템플릿을 업데이트하세요.
!!!

* 프로필을 저장하려면 필요한 모든 설정을 지정하고 "Create" 버튼을 클릭합니다. 그런 다음 설정을 검토하고 프로필 이름을 제공합니다. **이름은 고유해야 합니다.**
* 선택한 프로필에 대한 자세한 정보를 보려면 "Information" 버튼을 클릭합니다. 세부 정보를 숨기려면 다시 클릭합니다.
* 연결 프로필 설정은 "Update" 버튼을 누를 때까지 연결된 프로필 저장 파일을 변경하지 않고 `settings.json`에 저장됩니다. 즉, 프로필을 설정했지만 업데이트하지 않고 다른 프로필로 전환하면 이전 변경 사항이 모두 손실됩니다.
* 저장된 프로필에서 변경된 선택 사항을 복원하려면 "Reload" 버튼을 클릭합니다.
* 프로필을 삭제하려면 "Delete" 버튼을 클릭하고 삭제를 확인합니다. **이 작업은 되돌릴 수 없습니다.**

## 슬래시 커맨드

다음 슬래시 커맨드를 사용하여 연결 프로필을 관리할 수 있습니다.

1. `/profile [name]` - 인수가 제공되면 프로필로 전환하고 그렇지 않으면 현재 프로필의 이름을 가져옵니다.
2. `/profile-create [name]` - 제공된 이름으로 현재 설정을 새 프로필로 저장합니다.
3. `/profile-list` - JSON으로 직렬화된 사용 가능한 프로필 이름 배열을 반환합니다.
4. `/profile-get [name]` - JSON으로 직렬화된 객체로 제공된 이름을 가진 프로필의 세부 정보를 가져옵니다.
5. `/profile-update` - 현재 설정으로 선택한 프로필을 업데이트합니다.
