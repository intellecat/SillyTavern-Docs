---
order: 80
route: /ko/usage/core-concepts/instructmode/
---

# Instruct Mode

Instruct Mode를 사용하면 Alpaca, ChatML, Llama2 등과 같은 다양한 프롬프트 형식으로 훈련된 지시 따르기 모델에 대한 프롬프팅을 조정할 수 있습니다.

!!! 적용 대상: Text Completion APIs
Chat Completion API의 동등한 설정은 [Prompt Manager](prompt-manager.md)를 사용하세요.
!!!

## API 지원

### Text Completion API

완전히 지원됩니다. 여기에는 다음이 포함됩니다:

* Text Completion 아래의 모든 소스
* KoboldAI Classic
* AI Horde

#### 형식 선택

선택한 instruct 템플릿은 백엔드에서 실행 중인 실제 모델의 기대와 일치해야 합니다.

이것은 일반적으로 HuggingFace의 모델 카드에 반영되며, 일부는 SillyTavern 호환 JSON 파일도 제공합니다.

예: [NeverSleep/Noromaid-13b-v0.1.1](https://huggingface.co/NeverSleep/Noromaid-13b-v0.1.1#prompt-template-custom-format-or-alpaca)

### Chat Completion API (OpenAI, Claude 등)

Chat Completion API에는 지원되지 않으며 **(필요하지 않습니다)**. 완전히 다른 프롬프트 빌더를 사용합니다.

### NovelAI

NovelAI에 *기술적으로* 지원되지만, 그들의 모델 중 어느 것도 instruct 형식을 이해하도록 훈련되지 않았습니다. NovelAI 모델은 채팅 메시지에서 중괄호로 감싸인 지시사항이 발견될 때 *자동으로* 활성화되는 특수 instruct 모듈을 사용할 수 있으므로 전체 프롬프트에 Instruct Mode를 사용하면 출력의 **품질이 저하됩니다**.

다음은 NovelAI의 instruct 모듈을 자동 활성화하는 예입니다:

```txt
User: { Write a happy song about Nintendo Switch. }
```

## Instruct Mode 설정

### System Prompt

!!!warning 최근 변경 사항
시스템 프롬프트는 이제 별도의 엔티티입니다. 자세한 내용은 [Advanced Formatting](advancedformatting.md#system-prompt) 페이지를 참조하세요.
!!!

### Templates

잘 알려진 instruct 모델에 대한 시퀀스가 있는 기성 템플릿을 제공합니다.

*템플릿을 변경하면 저장되지 않은 설정이 마지막으로 저장된 상태로 재설정됩니다! 잃고 싶지 않은 변경 사항이 있으면 템플릿을 저장하는 것을 잊지 마세요.*

### Activation Regex

유효한 정규 표현식으로 정의된 경우, 모델에 연결되고 이름이 이 regex와 일치하면 이 템플릿을 자동으로 선택합니다.

Instruct mode는 미리 활성화되어야 합니다. 템플릿 전체에서 첫 번째 regex 일치만 선택됩니다(알파벳 순서로 평가됨).

### Wrap Sequences with Newline

각 시퀀스 텍스트는 프롬프트에 삽입될 때 줄바꿈 문자로 감싸집니다. Alpaca 및 그 파생물에 필요합니다.

줄 종결자를 완전히 제어하려면 비활성화하세요.

### Replace Macro in Sequences

활성화하면 메시지 감싸기 시퀀스에서 정의된 경우 알려진 \{\{macro\}\} 대체가 대체됩니다.

또한, 특수 \{\{name\}\} 매크로를 메시지 접두사에서 사용하여 메시지에 첨부된 실제 이름을 참조할 수 있습니다(현재 활성화된 \{\{char\}\} 또는 \{\{user\}\}가 아닌). 이것은 그룹 채팅이나 /sendas 명령을 사용할 때 유용할 수 있습니다. 이름을 확인할 수 없는 경우 "System"이 대체 플레이스홀더로 사용됩니다.

### Include Names

활성화하면 접두사 시퀀스 이후에 채팅 기록 로그에 캐릭터 및 사용자 이름을 앞에 추가합니다.

다음 옵션을 사용할 수 있습니다:

* **Never**: 메시지 콘텐츠 앞에 이름 접두사를 추가하지 않습니다.
* **Groups and Past Personas**: 그룹 캐릭터 및 과거 페르소나의 메시지에만 이름 접두사를 추가합니다.
* **Always**: 메시지 콘텐츠 앞에 항상 이름 접두사를 추가합니다.

### Sequences: Story String Wrapping

!!!warning 최근 변경 사항
시스템 프롬프트 감싸기가 제거되고 Story String 감싸기로 대체되었습니다.
!!!

위치가 "Default (top of context)"로 설정된 경우 Story String이 감싸이는 방법을 정의합니다

#### Story String Prefix

Story String 앞에 삽입됩니다.

#### Story String Suffix

Story String 뒤에 삽입됩니다.

### Sequences: Chat Messages Wrapping

이러한 설정은 프롬프트를 구성할 때 다른 역할에 속하는 메시지가 감싸이는 방법을 정의합니다.

모든 접두사 시퀀스는 중지 문자열로도 자동으로 사용됩니다.

#### User Message Prefix

User 메시지 앞과 가장하기 시 마지막 프롬프트 줄로 삽입됩니다.

#### User Message Suffix

User 메시지 뒤에 삽입됩니다.

#### Assistant Message Prefix

Assistant 메시지 앞과 AI 응답을 생성할 때 마지막 프롬프트 줄로 삽입됩니다.

#### Assistant Message Suffix

Assistant 메시지 뒤에 삽입됩니다

#### System Message Prefix

System(슬래시 명령 또는 확장 프로그램에 의해 추가됨) 메시지 앞에 삽입됩니다.

#### System Message Suffix

System 메시지 뒤에 삽입됩니다.

#### System same as User

true로 확인하면 System 메시지는 User 역할 메시지 시퀀스를 사용합니다.

그렇지 않으면 System 메시지는 자체 시퀀스를 사용합니다(비어 있지 않은 경우) 또는 전혀 감싸기를 하지 않습니다(비어 있는 경우).

### Misc. Sequences

프롬프트 구축의 미세 조정을 위한 다양한 고급 구성

#### First Assistant Prefix

첫 번째 Assistant 메시지 앞에 삽입됩니다.

!!!info
**채팅 기록**의 첫 번째 메시지만 계산되며, 실제로 프롬프트에 먼저 들어가는 메시지가 아닙니다!
!!!

#### Last Assistant Prefix

마지막 Assistant 메시지 앞 또는 AI 응답을 생성할 때 마지막 프롬프트 줄로 삽입됩니다.

!!!info
백그라운드에서 텍스트를 생성할 때(예: Stable Diffusion 프롬프트 또는 요약) 사용되지 않습니다. 대신 System Instruction Prefix 또는 Regular Assistant Prefix가 사용됩니다.
!!!

#### System Instruction Prefix

백그라운드에서 중립/시스템 텍스트를 생성할 때(예: Stable Diffusion 프롬프트 또는 요약) 마지막 프롬프트 줄로 삽입됩니다.

#### User Filler Message

채팅 기록이 User 메시지로 시작하지 않으면 채팅 기록의 시작 부분에 삽입됩니다.

**사용 사례:** instruct 형식이 *엄격하게* 프롬프트가 사용자 우선이어야 하고 교대로 역할이 있는 메시지만 있어야 하는 경우, 예: Llama 2 Chat, Mistral Instruct.

#### Stop Sequence

응답의 끝을 나타내는 텍스트입니다. 백엔드 API에 중지 문자열로도 전송됩니다.

중지 시퀀스가 생성되면 그 이후의 모든 것이 출력에서 제거됩니다(시퀀스 자체 포함).
