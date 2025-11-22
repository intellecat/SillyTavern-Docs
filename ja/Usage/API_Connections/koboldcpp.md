---
route: /usage/api-connections/koboldcpp/
---

# KoboldCpp

KoboldCppはGGMLおよびGGUFモデル用の自己完結型APIです。

Nyxによるこの[VRAM Calculator](https://huggingface.co/spaces/NyxKrage/LLM-Model-VRAM-Calculator)は、モデルが必要とするおおよそのRAM/VRAMを教えてくれます。

## Nvidia GPU Quickstart

このガイドはWindowsを使用していることを前提としています。

* 最新リリースをダウンロード: <https://github.com/LostRuins/koboldcpp/releases>
* KoboldCppを起動します。Microsoft Defenderからのポップアップが表示される場合がありますが、`Run Anyway`をクリックしてください。
* バージョン1.58以降、KoboldCppは次のようになっているはずです:

![KoboldCpp 1.58](/static/koboldcpp.png)

* `Quick Launch`タブで、モデルと希望する`Context Size`を選択します。
* `Use CuBLAS`を選択し、`GPU ID`の横の黄色のテキストがGPUと一致していることを確認します。
* 低VRAMでも`Low VRAM`にチェックを入れないでください。
* Nvidia 10シリーズ以前のGPUを持っていない限り、`Use QuantMatMul (mmq)`のチェックを外してください。
* `GPU Layers`はモデルをロードしたときに自動入力されているはずです。今はそのままにしておいてください。
* `Hardware`タブで、`High Priority`にチェックを入れます。
* `Save`をクリックして、毎回の起動時にKoboldCppを設定する必要がないようにします。
* `Launch`をクリックしてモデルがロードされるのを待ちます。

次のようなものが表示されるはずです:

```txt
Load Model OK: True
Embedded Kobold Lite loaded.
Starting Kobold API on port 5001 at http://localhost:5001/api/
Starting OpenAI Compatible API on port 5001 at http://localhost:5001/v1/
======
Please connect to custom endpoint at http://localhost:5001
```

これでSillyTavern内でKoboldCppに`http://localhost:5001`をAPI URLとして接続してチャットを開始できます。

**おめでとうございます！完了です！**

ある意味では。

### GPU Layers

KoboldCppは動作していますが、できるだけ多くのレイヤーをGPUにオフロードすることでパフォーマンスを向上させることができます。ターミナルで次のようなものが表示されるはずです:

```txt
llm_load_tensors: offloading 9 repeating layers to GPU
llm_load_tensors: offloaded 9/33 layers to GPU
llm_load_tensors:        CPU buffer size = 25215.88 MiB
llm_load_tensors:      CUDA0 buffer size =  7043.34 MiB
....................................................................................................
llama_kv_cache_init:  CUDA_Host KV buffer size =  1479.19 MiB
llama_kv_cache_init:      CUDA0 KV buffer size =   578.81 MiB
```

数字を恐れないでください。この部分は見た目ほど難しくありません。`CPU buffer size`は使用されているシステムRAMの量を指します。これは無視してください。`CUDA0 buffer size`は使用されているGPU VRAMの量を指します。`CUDA_Host KV buffer size`と`CUDA0 KV buffer size`は、モデルのコンテキストに専用されているGPU VRAMの量を指します。この場合、KoboldCppは約9 GBのVRAMを使用しています。

私は12 GBのVRAMを持っており、コンテキストに2 GBのVRAMしか使用されていないため、モデルをロードするために約10 GBのVRAMが残っています。9レイヤーが約7 GBのVRAMを使用し、`7000 / 9 = 777.77`なので、各レイヤーが約`777.77 MIB`のVRAMを使用すると仮定できます。`10,000 MIB / 777.77 = 12.8`なので、切り捨てて今後このモデルでは12レイヤーをロードします。

モデル、コンテキストサイズ、システムのVRAMを使用して独自の計算を行い、KoboldCppを再起動します:

* 以前に`Save`をクリックした場合は、`Load`を使用して以前の設定をロードできます。それ以外の場合は、以前に選択したのと同じ設定を選択します。
* `GPU Layers`を新しいVRAM最適化された数値(私の場合は12レイヤー)に変更します。
* `Save`をクリックして更新された設定を保存します。

次のようなものが表示されるはずです:

```txt
llm_load_tensors: offloading 12 repeating layers to GPU
llm_load_tensors: offloaded 12/33 layers to GPU
llm_load_tensors:        CPU buffer size = 25215.88 MiB
llm_load_tensors:      CUDA0 buffer size =  9391.12 MiB
....................................................................................................
llama_kv_cache_init:  CUDA_Host KV buffer size =  1286.25 MiB
llama_kv_cache_init:      CUDA0 KV buffer size =   771.75 MiB
```

KoboldCppは12 GBのVRAMのうち約11.5 GBを使用しています。これは、KoboldCppが自動的に生成した設定よりもはるかに優れたパフォーマンスを発揮するはずです。

**おめでとうございます！(実際に)完了です！**

KoboldCpp設定のより詳細な説明については、Kalomazeの[Simple Llama + SillyTavern Setup Guide](https://rentry.org/llama_v2_sillytavern)をご覧ください。
