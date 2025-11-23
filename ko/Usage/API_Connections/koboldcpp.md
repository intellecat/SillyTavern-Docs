---
route: /usage/api-connections/koboldcpp/
label: KoboldCpp
title: KoboldCpp
---

# KoboldCpp

KoboldCpp는 GGML 및 GGUF 모델을 위한 자체 포함 API입니다.

Nyx의 이 [VRAM Calculator](https://huggingface.co/spaces/NyxKrage/LLM-Model-VRAM-Calculator)는 모델에 필요한 RAM/VRAM의 대략적인 양을 알려줍니다.

## Nvidia GPU 빠른 시작

이 가이드는 Windows를 사용한다고 가정합니다.

* 최신 릴리스 다운로드: <https://github.com/LostRuins/koboldcpp/releases>
* KoboldCpp를 시작합니다. Microsoft Defender의 팝업이 표시될 수 있습니다. `Run Anyway`를 클릭하세요.
* 버전 1.58 기준으로 KoboldCpp는 다음과 같이 보여야 합니다:

![KoboldCpp 1.58](/static/koboldcpp.png)

* `Quick Launch` 탭에서 모델과 선호하는 `Context Size`를 선택합니다.
* `Use CuBLAS`를 선택하고 `GPU ID` 옆의 노란색 텍스트가 GPU와 일치하는지 확인하세요.
* VRAM이 낮더라도 `Low VRAM`을 선택하지 마세요.
* Nvidia 10 시리즈 이하 GPU가 없으면 `Use QuantMatMul (mmq)`를 선택 취소하세요.
* 모델을 로드할 때 `GPU Layers`가 채워져 있어야 합니다. 지금은 그대로 두세요.
* `Hardware` 탭에서 `High Priority`를 선택합니다.
* `Save`를 클릭하여 KoboldCpp를 시작할 때마다 구성할 필요가 없도록 합니다.
* `Launch`를 클릭하고 모델이 로드될 때까지 기다립니다.

다음과 같이 표시되어야 합니다:

```txt
Load Model OK: True
Embedded Kobold Lite loaded.
Starting Kobold API on port 5001 at http://localhost:5001/api/
Starting OpenAI Compatible API on port 5001 at http://localhost:5001/v1/
======
Please connect to custom endpoint at http://localhost:5001
```

이제 API URL로 `http://localhost:5001`을 사용하여 SillyTavern 내에서 KoboldCpp에 연결하고 채팅을 시작할 수 있습니다.

**축하합니다! 완료되었습니다!**

어느 정도.

### GPU Layers

KoboldCpp가 작동하지만 가능한 한 많은 레이어가 GPU로 오프로드되도록 하여 성능을 향상시킬 수 있습니다. 터미널에 다음과 같이 표시되어야 합니다:

```txt
llm_load_tensors: offloading 9 repeating layers to GPU
llm_load_tensors: offloaded 9/33 layers to GPU
llm_load_tensors:        CPU buffer size = 25215.88 MiB
llm_load_tensors:      CUDA0 buffer size =  7043.34 MiB
....................................................................................................
llama_kv_cache_init:  CUDA_Host KV buffer size =  1479.19 MiB
llama_kv_cache_init:      CUDA0 KV buffer size =   578.81 MiB
```

숫자를 두려워하지 마세요. 이 부분은 보기보다 쉽습니다. `CPU buffer size`는 사용 중인 시스템 RAM의 양을 나타냅니다. 무시하세요. `CUDA0 buffer size`는 사용 중인 GPU VRAM의 양을 나타냅니다. `CUDA_Host KV buffer size`와 `CUDA0 KV buffer size`는 모델의 컨텍스트에 전용되는 GPU VRAM의 양을 나타냅니다. 이 경우 KoboldCpp는 약 9GB의 VRAM을 사용합니다.

저는 12GB의 VRAM이 있고 컨텍스트에 2GB의 VRAM만 사용하므로 모델을 로드하는 데 약 10GB의 VRAM이 남아 있습니다. 9개의 레이어가 약 7GB의 VRAM을 사용했고 `7000 / 9 = 777.77`이므로 각 레이어가 약 `777.77 MIB`의 VRAM을 사용한다고 가정할 수 있습니다. `10,000 MIB / 777.77 = 12.8`이므로 반올림하여 이 모델로 앞으로 12개의 레이어를 로드하겠습니다.

이제 모델, 컨텍스트 크기 및 시스템 VRAM을 사용하여 자체 계산을 수행하고 KoboldCpp를 다시 시작합니다:

* 똑똑하다면 이전에 `Save`를 클릭했으므로 이제 `Load`를 사용하여 이전 구성을 로드할 수 있습니다. 그렇지 않으면 이전에 선택한 것과 동일한 설정을 선택하세요.
* `GPU Layers`를 새로운 VRAM 최적화 숫자(제 경우 12개 레이어)로 변경합니다.
* `Save`를 클릭하여 업데이트된 구성을 저장합니다.

이제 다음과 같이 표시되어야 합니다:

```txt
llm_load_tensors: offloading 12 repeating layers to GPU
llm_load_tensors: offloaded 12/33 layers to GPU
llm_load_tensors:        CPU buffer size = 25215.88 MiB
llm_load_tensors:      CUDA0 buffer size =  9391.12 MiB
....................................................................................................
llama_kv_cache_init:  CUDA_Host KV buffer size =  1286.25 MiB
llama_kv_cache_init:      CUDA0 KV buffer size =   771.75 MiB
```

KoboldCpp는 12GB VRAM 중 약 11.5GB를 사용합니다. KoboldCpp에서 자동으로 생성된 설정보다 훨씬 더 나은 성능을 발휘할 것입니다.

**축하합니다! (실제로) 완료되었습니다!**

KoboldCpp 설정에 대한 자세한 내용은 Kalomaze의 [Simple Llama + SillyTavern Setup Guide](https://rentry.org/llama_v2_sillytavern)를 확인하세요.
