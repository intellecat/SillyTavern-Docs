---
route: /ko/usage/api-connections/dreamgen/
label: DreamGen
title: DreamGen
---

# DreamGen

DreamGen은 AI 기반 역할극 및 스토리 작성을 위한 앱이자 API입니다. 무료 계층과 조종 가능한 AI 역할극 및 스토리 작성 목적으로 특별히 만들어진 고품질 자체 텍스트 생성 모델에 대한 무제한 월간 액세스를 허용하는 유료 구독이 있습니다. 시작하려면 계정을 만드세요: <https://dreamgen.com/>.

(무료) 크레딧은 매 달력 월 초에 재설정됩니다. 각 모델의 크레딧 비용을 보려면 [가격](https://dreamgen.com/pricing)을 참조하고 남은 크레딧을 보려면 [사용량](https://dreamgen.com/account/usage)을 참조하세요.

## DreamGen에 연결

### API 키 받기

[DreamGen API keys](https://dreamgen.com/account/api-keys) 페이지로 이동하여 "New API Key" 버튼을 클릭합니다. API 키가 클립보드에 복사되었는지 확인하세요.

![Create New DreamGen API key](/static/dreamgen/dreamgen_api_keys_new.jpg)
![Copy DreamGen API key](/static/dreamgen/dreamgen_api_keys_copy.jpg)

### 연결

1. SillyTavern 연결 설정으로 이동합니다.
2. API: Text Completion 선택
3. API Type: DreamGen 선택
4. API 키 입력
5. (선택 사항) 모델 선택

![Connecting to DreamGen](/static/dreamgen/dreamgen_st_connection.png)

## 모델

DreamGen API는 다양한 크기의 여러 모델을 제공합니다.

- Lucid Max (API에서 `lucid-v1-max` 또는 `lucid-v1-extra-large`로 호출)
- Lucid Base (API에서 `lucid-v1-base` 또는 `lucid-v1-medium`로 호출) -- 가중치 사용 가능한 [Lucid V1 Nemo](https://dreamgen.com/docs/models/lucid-v1/huggingface)에 해당합니다.

Lucid Base는 훨씬 적은 크레딧을 사용하고 더 빠르지만 Lucid Max는 더 창의적이고 더 복잡한 지침과 내러티브를 처리할 수 있습니다.

## 설정

Lucid V1 DreamGen 모델은 역할극 및 글쓰기에 최적화된 Llama 3 채팅 템플릿의 확장을 사용합니다. 특정 시스템 프롬프트와 함께 가장 잘 작동합니다.

다음 마스터 프리셋 중 하나로 시작하는 것을 강력히 권장합니다:

- instruct mode를 활성화하고 가져올 때 모든 확인란을 선택하세요.
- [DreamGen Lucid V1 Role-Play preset](https://dreamgen.com/docs/models/lucid-v1/sillytavern/master-preset/role-play)
- [DreamGen Lucid V1 Story preset](https://dreamgen.com/docs/models/lucid-v1/sillytavern/master-preset/story)

이러한 프리셋에는 모델에 지침을 보내기 위한 `/sys`에 대한 기본 제공 지원이 함께 제공됩니다. 이를 사용하여 플롯을 조종하거나 캐릭터의 행동을 제어할 수 있습니다.

![DreamGen preset selected](/static/dreamgen/dreamgen_st_preset.png)

기타 리소스:

- [**상세한 DreamGen + SillyTavern 가이드**](https://dreamgen.com/docs/models/lucid-v1/sillytavern)
- [상세한 Lucid V1 프롬프트 형식 문서](https://dreamgen.com/docs/models/lucid-v1).
- [DreamGen + SillyTavern 역할극 데모](https://imgur.com/a/dreamgen-lucid-sillytavern-roleplay-demo-bhzQpto)
- [DreamGen + SillyTavern 스토리 작성 데모](https://imgur.com/a/dreamgen-lucid-sillytavern-writing-demo-JLv5iO3)
- [자신만의 시나리오 만들기 팁](https://v2.dreamgen.com/docs/scenario-editor)

## FAQ

### 응답을 더 길게 또는 더 짧게 만들려면 어떻게 해야 하나요?

형식 프리셋에서 `Last Assistant Prefix`를 설정할 수 있습니다.

긴 메시지의 경우:

```txt
<|start_header_id|>user<|end_header_id|>

The next message is from {{char}} and is at least 100 words long<|eot_id|><|start_header_id|>writer character {{char}}<|end_header_id|>

```

짧은 메시지의 경우:

```txt
<|start_header_id|>user<|end_header_id|>

The next message is from {{char}} and is at most 50 words long<|eot_id|><|start_header_id|>writer character {{char}}<|end_header_id|>

```

끝에 있는 두 줄을 포함한 모든 줄 바꿈을 유지하세요.

![Long Message Prefix](/static/dreamgen/dreamgen_st_long_response_prefix.png)

카드나 시스템 프롬프트에 글쓰기 스타일 설명을 포함할 수도 있습니다. 예:

```txt
## Style

<your description>
```

자세히 알아보고 몇 가지 예를 보려면 ["Style" documentation](https://v2.dreamgen.com/docs/scenario-editor#style)을 참조하세요.

### 역할극 / 스토리를 조종하려면 어떻게 해야 하나요?

`/sys` 옵션을 사용하여 모델에 지침을 보냅니다. 몇 가지 예:

> The innkeeper offers Daria and the others a pint of ale.

> The next message is from Draco and should be at least 200 words, focusing on his inner conflict about the decision.

[실제로 확인하세요.](https://imgur.com/a/dreamgen-lucid-sillytavern-roleplay-demo-bhzQpto)
