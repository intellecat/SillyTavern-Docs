---
route: /usage/api-connections/koboldcpp/
---

# KoboldCpp

KoboldCpp هو واجهة API مستقلة لنماذج GGML و GGUF.

ستخبرك [حاسبة VRAM](https://huggingface.co/spaces/NyxKrage/LLM-Model-VRAM-Calculator) بواسطة Nyx تقريباً بمقدار RAM/VRAM الذي يتطلبه نموذجك.

## بداية سريعة لـ Nvidia GPU

يفترض هذا الدليل أنك تستخدم Windows.

* قم بتنزيل أحدث إصدار: <https://github.com/LostRuins/koboldcpp/releases>
* قم بتشغيل KoboldCpp. قد ترى نافذة منبثقة من Microsoft Defender، انقر على `Run Anyway`.
* اعتباراً من الإصدار 1.58، يجب أن يبدو KoboldCpp هكذا:

![KoboldCpp 1.58](/static/koboldcpp.png)

* ضمن علامة التبويب `Quick Launch`، حدد النموذج و`Context Size` المفضل لديك.
* حدد `Use CuBLAS` وتأكد من أن النص الأصفر بجوار `GPU ID` يطابق GPU الخاص بك.
* لا تحدد `Low VRAM`، حتى لو كان لديك VRAM منخفض.
* ما لم يكن لديك Nvidia 10-series أو GPU أقدم، قم بإلغاء تحديد `Use QuantMatMul (mmq)`.
* يجب أن يكون `GPU Layers` قد تم ملؤه عند تحميل نموذجك. اتركه هناك الآن.
* ضمن علامة التبويب `Hardware`، حدد `High Priority`.
* انقر على `Save` حتى لا تضطر إلى تكوين KoboldCpp في كل مرة تشغّله.
* انقر على `Launch` وانتظر حتى يتم تحميل النموذج.

يجب أن ترى شيئاً مثل هذا:

```txt
Load Model OK: True
Embedded Kobold Lite loaded.
Starting Kobold API on port 5001 at http://localhost:5001/api/
Starting OpenAI Compatible API on port 5001 at http://localhost:5001/v1/
======
Please connect to custom endpoint at http://localhost:5001
```

يمكنك الآن الاتصال بـ KoboldCpp داخل SillyTavern باستخدام `http://localhost:5001` كعنوان URL لـ API والبدء في الدردشة.

**تهانينا! انتهيت!**

نوعاً ما.

### طبقات GPU

KoboldCpp يعمل، لكن يمكنك تحسين الأداء عن طريق التأكد من نقل أكبر عدد ممكن من الطبقات إلى GPU. يجب أن ترى شيئاً مثل هذا في الطرفية:

```txt
llm_load_tensors: offloading 9 repeating layers to GPU
llm_load_tensors: offloaded 9/33 layers to GPU
llm_load_tensors:        CPU buffer size = 25215.88 MiB
llm_load_tensors:      CUDA0 buffer size =  7043.34 MiB
....................................................................................................
llama_kv_cache_init:  CUDA_Host KV buffer size =  1479.19 MiB
llama_kv_cache_init:      CUDA0 KV buffer size =   578.81 MiB
```

لا تخف من الأرقام؛ هذا الجزء أسهل مما يبدو. يشير `CPU buffer size` إلى مقدار RAM النظام الذي يتم استخدامه. تجاهل ذلك. يشير `CUDA0 buffer size` إلى مقدار GPU VRAM الذي يتم استخدامه. يشير `CUDA_Host KV buffer size` و`CUDA0 KV buffer size` إلى مقدار GPU VRAM المخصص لسياق نموذجك. في هذه الحالة، يستخدم KoboldCpp حوالي 9 جيجابايت من VRAM.

لدي 12 جيجابايت من VRAM، ويتم استخدام 2 جيجابايت فقط من VRAM للسياق، لذلك لدي حوالي 10 جيجابايت من VRAM متبقية لتحميل النموذج. نظراً لأن 9 طبقات استخدمت حوالي 7 جيجابايت من VRAM و`7000 / 9 = 777.77` يمكننا افتراض أن كل طبقة تستخدم تقريباً `777.77 MIB` من VRAM. `10,000 MIB / 777.77 = 12.8`، لذلك سأقوم بالتقريب إلى الأسفل وتحميل 12 طبقة مع هذا النموذج من الآن فصاعداً.

الآن قم بحسابك الخاص باستخدام النموذج وحجم السياق و VRAM لنظامك، وأعد تشغيل KoboldCpp:

* إذا كنت ذكياً، فقد نقرت على `Save` من قبل، والآن يمكنك تحميل تكوينك السابق باستخدام `Load`. وإلا، حدد نفس الإعدادات التي اخترتها من قبل.
* قم بتغيير `GPU Layers` إلى رقمك الجديد المحسّن لـ VRAM (12 طبقة في حالتي).
* انقر على `Save` لحفظ تكوينك المحدّث.

يجب أن ترى الآن شيئاً مثل هذا:

```txt
llm_load_tensors: offloading 12 repeating layers to GPU
llm_load_tensors: offloaded 12/33 layers to GPU
llm_load_tensors:        CPU buffer size = 25215.88 MiB
llm_load_tensors:      CUDA0 buffer size =  9391.12 MiB
....................................................................................................
llama_kv_cache_init:  CUDA_Host KV buffer size =  1286.25 MiB
llama_kv_cache_init:      CUDA0 KV buffer size =   771.75 MiB
```

يستخدم KoboldCpp حوالي 11.5 جيجابايت من 12 جيجابايت VRAM الخاصة بي. يجب أن يؤدي هذا أداءً أفضل بكثير من الإعدادات التي تم إنشاؤها تلقائياً بواسطة KoboldCpp.

**تهانينا! انتهيت (فعلياً)!**

للحصول على نظرة أكثر تعمقاً في إعدادات KoboldCpp، راجع دليل Kalomaze [دليل إعداد Simple Llama + SillyTavern](https://rentry.org/llama_v2_sillytavern).
