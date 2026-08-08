---
order: 30
route: /ko/usage/core-concepts/data-bank/
tags:
    [
        vector storage,
        RAG,
        retrieval-augmented generation,
        vectors,
        documents,
        files,
        attachments,
    ]
---

# 데이터 뱅크 (RAG)

검색 증강 생성(RAG)은 LLM에 외부 지식 소스를 제공하는 기술입니다. 모델의 훈련 데이터 외부의 정보에 액세스하여 AI 답변의 정확도를 향상시키는 데 도움이 됩니다.

SillyTavern은 다양한 소스에서 다목적 지식 베이스를 구축하고 수집된 데이터를 LLM 프롬프트에 사용하기 위한 도구 세트를 제공합니다.

## 데이터 뱅크 액세스

내장된 Chat Attachments 확장 프로그램(릴리스 버전 >= 1.12.0에 기본적으로 포함)은 "Magic Wand" 메뉴에 새 옵션인 Data Bank를 추가합니다. 이것은 SillyTavern에서 RAG에 사용 가능한 문서를 관리하기 위한 허브입니다.

## 문서 정보

데이터 뱅크는 문서라고도 하는 파일 첨부 파일을 저장합니다. 문서는 세 가지 가용성 범위로 나뉩니다.

1. 전역 첨부 파일 - 단독 또는 그룹 채팅의 모든 채팅에서 사용 가능합니다.
2. 캐릭터 첨부 파일 - 현재 선택된 캐릭터에만 사용 가능하며, 그룹에서 응답할 때도 포함됩니다. _첨부 파일은 로컬로 저장되며 캐릭터 카드와 함께 내보내지지 않습니다!_
3. 채팅 첨부 파일 - 현재 열린 채팅에서만 사용 가능합니다. 채팅의 모든 캐릭터가 이것에서 가져올 수 있습니다.

!!!info 참고
공식적으로 데이터 뱅크의 일부는 아니지만 개별 메시지에도 파일을 첨부할 수 있습니다. "Wand" 메뉴에서 Attach File 옵션을 사용하거나 메시지 작업 행의 클립 아이콘을 사용하세요.
!!!

무엇이 문서가 될 수 있나요? 실제로 일반 텍스트 형식으로 표현할 수 있는 모든 것입니다!

예시에는 다음이 포함되지만 이에 국한되지 않습니다:

- 로컬 파일(책, 과학 논문 등)
- 웹 페이지(Wikipedia, 기사, 뉴스)
- 비디오 대본

다양한 확장 프로그램 및 플러그인은 데이터를 수집하고 처리하는 새로운 방법을 제공할 수도 있습니다. 자세한 내용은 아래를 참조하세요.

## 데이터 소스

범위 중 하나에 문서를 추가하려면 "Add"를 클릭하고 사용 가능한 소스 중 하나를 선택합니다.

### Notepad

처음부터 텍스트 파일을 만들거나 기존 첨부 파일을 편집합니다.

### File

컴퓨터 하드 드라이브에서 파일을 업로드합니다. SillyTavern은 인기 있는 파일 형식에 대한 내장 변환기를 제공합니다:

- PDF (텍스트만)
- HTML
- Markdown
- ePUB
- TXT

JSON, YAML, 소스 코드 등과 같은 비표준 확장자를 가진 텍스트 파일도 첨부할 수 있습니다. 선택한 파일 유형에서 알려진 변환이 없고 파일을 일반 텍스트 문서로 구문 분석할 수 없는 경우 파일 업로드가 거부됩니다. 즉, 원시 바이너리 파일은 허용되지 않습니다.

!!!info 참고
Microsoft Office(DOCX, PPTX, XLSX) 및 LibreOffice 문서(ODT, ODP, ODS) 가져오기는 [Server Plugin](https://github.com/SillyTavern/SillyTavern-Office-Parser)을 설치하고 로드해야 합니다. 설치 지침은 플러그인의 README 페이지를 참조하세요.
!!!

### Web

URL로 웹 페이지에서 텍스트를 스크랩합니다. HTML 문서는 [Readability](https://github.com/mozilla/readability) 라이브러리를 통해 처리되어 사용 가능한 텍스트만 추출합니다.

일부 웹 서버는 가져오기 요청을 거부하거나 Cloudflare로 보호되거나 JavaScript에 크게 의존하여 작동할 수 있습니다. 특정 사이트에 문제가 있는 경우 웹 브라우저를 통해 페이지를 수동으로 다운로드하고 파일 업로더를 사용하여 첨부하세요.

### YouTube

ID 또는 URL로 YouTube 비디오의 대본을 다운로드합니다. 제작자가 업로드했거나 Google에서 자동 생성한 것입니다. 일부 비디오는 대본이 비활성화되어 있을 수 있으며, 연령 제한 비디오의 구문 분석은 로그인이 필요하므로 사용할 수 없습니다.

스크립트는 비디오의 기본 언어로 로드됩니다. 선택적으로 두 문자 언어 코드를 지정하여 특정 언어로 대본을 가져올 수 있습니다. 이 기능은 항상 사용 가능한 것이 아니며 실패할 수 있으므로 주의해서 사용하세요.

### Web Search

!!!info 참고
이 소스는 [Web Search](/extensions/WebSearch.md) 확장 프로그램이 설치되고 올바르게 구성되어 있어야 합니다. 자세한 내용은 연결된 페이지를 참조하세요.
!!!

웹 검색을 수행하고 검색 결과 페이지에서 텍스트를 다운로드합니다. 이것은 Web 소스와 유사하지만 완전히 자동화되어 있습니다. 선택한 검색 엔진은 확장 프로그램 설정에서 상속되므로 미리 설정하세요.

시작하려면 검색 쿼리, 방문할 최대 링크 수 및 출력 유형을 지정합니다: 하나의 결합된 파일(확장 프로그램 규칙에 따라 형식화됨) 또는 각 페이지에 대한 개별 파일입니다. 페이지 스니펫도 저장하도록 선택할 수 있습니다.

### Fandom

!!!info 참고
이 소스는 [Server Plugin](https://github.com/SillyTavern/SillyTavern-Fandom-Scraper)이 설치되고 로드되어 있어야 합니다. 설치 지침은 플러그인의 README 페이지를 참조하세요.
!!!

ID 또는 URL로 [Fandom](https://www.fandom.com/) 위키에서 기사를 스크랩합니다. 일부 위키는 매우 크므로 필터 정규식을 사용하여 범위를 제한하는 것이 유용할 수 있습니다. 기사의 제목에 대해 테스트됩니다. 필터가 제공되지 않으면 모든 페이지가 내보내기 대상이 됩니다. 각 페이지에 대한 개별 파일로 저장하거나 하나의 문서로 결합할 수 있습니다.

### Bronie Parser Extension (Third-Party)

!!!warning 참고
이 소스는 타사에서 제공되며 SillyTavern 팀과 **관련이 없습니다**. 이 소스를 사용하려면 Bronya Rand의 [Bronie Parser Extension](https://github.com/Bronya-Rand/Bronie-Parser-Extension)과 파서가 작동하는 데 필요한 Server Plugins를 설치해야 합니다.
!!!

Bronya Rand의 Bronie Parser Extension은 miHoYo/HoYoverse의 [HoYoLab](https://wiki.hoyolab.com)과 같은 타사 스크레이퍼를 SillyTavern에 사용할 수 있도록 합니다. 다른 데이터 소스와 유사합니다.

현재 Bronya Rand의 Bronie Parser Extension은 다음을 지원합니다:

- miHoYo/HoYoverse's HoYoLab (Genshin Impact/Honkai: Star Rail용) [HoYoWiki-Scraper-TS](https://github.com/Bronya-Rand/HoYoWiki-Scraper-TS)를 통해

시작하려면 [설치 가이드](https://github.com/Bronya-Rand/Bronie-Parser-Extension?tab=readme-ov-file#installation)를 따라 Bronya Rand의 Bronie Parser Extension을 설치하고 지원되는 Server Plugin을 SillyTavern에 설치합니다. SillyTavern을 다시 시작하고 _Data Bank_ 메뉴로 이동합니다. `+ Add`를 클릭하면 최근에 설치한 스크레이퍼가 정보를 얻을 수 있는 소스의 가능한 목록에 추가된 것을 볼 수 있습니다.

## Vector Storage

자, 특정 주제에 대한 훌륭하고 포괄적인 정보 라이브러리를 만들었습니다. 다음은 무엇인가요?

RAG에 문서를 사용하려면 관련 데이터를 LLM 프롬프트에 삽입하는 호환 가능한 확장 프로그램을 사용해야 합니다.

SillyTavern과 함께 제공되는 Vector Storage는 이러한 확장 프로그램의 참조 구현입니다. 임베딩(벡터라고도 함)을 사용하여 진행 중인 채팅과 관련된 문서를 검색합니다.

!!!info 재미있는 사실

1. 임베딩은 텍스트 조각을 추상적으로 나타내는 숫자 배열로, 전문 언어 모델에 의해 생성됩니다. 더 유사한 텍스트는 각각의 벡터 간의 거리가 더 짧습니다.
2. Vector Storage 확장 프로그램은 [Vectra](https://github.com/Stevenic/vectra) 라이브러리를 사용하여 파일 임베딩을 추적합니다. 사용자 데이터 디렉토리의 `/vectors` 폴더에 JSON 파일로 저장됩니다. 모든 문서는 내부적으로 자체 인덱스/컬렉션 파일로 표현됩니다.
   !!!

Vectors 기능은 기본적으로 비활성화되어 있으므로 확장 프로그램 패널("Stacked Cubes" 아이콘, 상단 바)을 연 다음 "Vector Storage" 섹션으로 이동하여 "File vectorization settings" 아래의 "Enabled for files" 확인란을 선택해야 합니다.

Vector Storage 자체는 벡터를 생성하지 않으므로 호환 가능한 임베딩 제공자를 사용해야 합니다.

## Vector Providers

!!!warning 경고
임베딩은 생성한 동일한 모델을 사용하여 검색할 때만 사용할 수 있습니다. 임베딩 모델 또는 소스를 변경할 때 벡터를 다시 계산해야 합니다.
!!!

### Local

이러한 소스는 무료이며 무제한이며 CPU/GPU를 사용하여 임베딩을 계산합니다.

1. Local (Transformers) - Node 서버에서 실행됩니다. SillyTavern은 HuggingFace에서 ONNX 형식의 호환 가능한 모델을 자동으로 다운로드합니다. 기본 모델: [jina-embeddings-v2-base-en](https://huggingface.co/Cohee/jina-embeddings-v2-base-en).
2. WebLLM - 확장 프로그램을 설치하고 [WebGPU를 지원하는](https://caniuse.com/webgpu) 웹 브라우저가 필요합니다. 브라우저에서 직접 실행되며 하드웨어 가속을 사용할 수 있습니다. HuggingFace에서 지원되는 모델을 자동으로 다운로드합니다. 여기에서 확장 프로그램을 설치하세요: <https://github.com/SillyTavern/Extension-WebLLM>.
3. Ollama - <https://ollama.com/>에서 가져오세요. API 연결 메뉴(Text Completion 아래, 기본값: `http://localhost:11434`)에서 API URL을 설정합니다. 먼저 호환 가능한 모델을 다운로드한 다음 확장 프로그램 설정에서 이름을 설정해야 합니다. 예시 모델: [mxbai-embed-large](https://ollama.com/library/mxbai-embed-large). 선택적으로 모델을 메모리에 로드된 상태로 유지하는 옵션을 확인합니다.
4. llama.cpp server - [ggerganov/llama.cpp](https://github.com/ggerganov/llama.cpp)에서 가져와서 `--embedding` 플래그로 서버 실행 파일을 실행합니다. HuggingFace에서 호환 가능한 GGUF 임베딩 모델을 로드합니다. 예를 들어 [nomic-ai/nomic-embed-text-v1.5-GGUF](https://huggingface.co/nomic-ai/nomic-embed-text-v1.5-GGUF).
5. vLLM - [vllm-project/vllm](https://github.com/vllm-project/vllm)에서 가져오세요. 먼저 API 연결 메뉴에서 API URL 및 API 키를 설정합니다.
6. Extras (사용 중단) - SentenceTransformers 로더를 사용하는 [Extras API](https://github.com/SillyTavern/SillyTavern-extras)에서 실행됩니다. 기본 모델: [all-mpnet-base-v2](https://huggingface.co/sentence-transformers/all-mpnet-base-v2). 이 소스는 유지 관리되지 않으며 향후에 결국 제거될 것입니다.

### API 소스

이러한 모든 소스는 각 서비스의 API 키가 필요하며 일반적으로 사용 비용이 있지만 일반적으로 임베딩 계산은 매우 저렴합니다.

1. OpenAI
2. Cohere
3. Google AI Studio
4. Google Vertex AI
5. TogetherAI
6. MistralAI
7. NomicAI
8. OpenRouter
9. Electron Hub
10. Chutes
11. NanoGPT
12. SiliconFlow
13. Cloudflare Workers AI

## 벡터화 설정

임베딩 제공자를 선택한 후 문서를 처리하고 검색하는 규칙을 정의하는 다른 설정을 구성하는 것을 잊지 마세요.

!!!info 참고
첨부 파일에서 정보를 분할, 벡터화 및 검색하는 데는 시간이 걸립니다. 파일의 초기 수집에는 시간이 걸릴 수 있지만 RAG 검색 쿼리는 일반적으로 상당한 지연을 발생시키지 않을 만큼 충분히 빠릅니다.
!!!

### 메시지 첨부 파일

이러한 설정은 메시지에 직접 첨부된 파일을 제어합니다.

다음 규칙이 적용됩니다:

1. LLM 컨텍스트 창에 맞는 메시지만 첨부 파일을 검색할 수 있습니다.
2. vector storage 확장 프로그램이 비활성화되면 파일 첨부 파일과 함께 제공되는 메시지가 프롬프트에 완전히 삽입됩니다.
3. 파일 벡터화가 활성화되면 파일이 청크로 분할되고 가장 관련성이 높은 조각만 삽입되어 컨텍스트 공간을 절약하고 모델이 집중할 수 있도록 합니다.

- Size threshold (KB) - 청크 분할 임계값을 설정합니다. 지정된 크기보다 큰 파일만 분할됩니다.
- Chunk size (chars) - 개별 청크의 목표 크기를 설정합니다(모델 토큰이 아닌 텍스트 문자 단위!).
- Chunk overlap (%) - 인접한 청크 간에 공유될 청크 크기의 백분율을 설정합니다. 이렇게 하면 청크 간의 더 부드러운 전환이 가능하지만 일부 중복도 발생할 수 있습니다.
- Retrieve chunks - 검색할 가장 관련성이 높은 파일 청크의 최대 양을 설정합니다. 원래 순서대로 삽입됩니다.

### 데이터 뱅크 파일

이러한 설정은 데이터 뱅크 문서가 처리되는 방법을 제어합니다.

다음 규칙이 적용됩니다:

1. 파일 벡터화가 비활성화되면 데이터 뱅크는 사용되지 않습니다.
2. 그렇지 않으면 현재 범위(위 참조)의 모든 사용 가능한 문서가 쿼리에 고려됩니다. 모든 파일에서 가장 관련성이 높은 청크만 검색됩니다. 동일한 파일의 여러 청크는 원래 순서대로 삽입됩니다.
3. 삽입된 청크는 채팅 메시지를 맞추기 전에 컨텍스트의 일부를 예약합니다.

- Size threshold (KB) - 청크 분할 임계값을 설정합니다. 지정된 크기보다 큰 파일만 분할됩니다.
- Chunk size (chars) - 개별 청크의 목표 크기를 설정합니다(모델 토큰이 아닌 텍스트 문자 단위!).
- Chunk overlap (%) - 인접한 청크 간에 공유될 청크 크기의 백분율을 설정합니다. 이렇게 하면 청크 간의 더 부드러운 전환이 가능하지만 일부 중복도 발생할 수 있습니다.
- Retrieve chunks - 검색할 파일 청크의 최대 양을 설정합니다. 이 허용량은 모든 파일 간에 공유됩니다.
- Injection Template - 검색된 정보가 프롬프트에 삽입되는 방법을 정의합니다. 특수 \{\{text\}\} 매크로를 사용하여 검색된 텍스트의 위치를 지정하고 다른 매크로도 사용할 수 있습니다.
- Injection Position - 프롬프트 주입을 삽입할 위치를 설정합니다. Author's Note 및 World Info와 동일한 규칙이 적용됩니다.

### 공유 설정

- Query messages - 문서 청크를 쿼리하는 데 사용될 최신 채팅 메시지 수입니다.
- Score threshold - 관련성 점수를 기반으로 청크 검색의 컬링을 허용하도록 조정합니다(0 - 전혀 일치하지 않음, 1 - 완벽한 일치). 더 높은 값은 더 정확한 검색을 허용하고 완전히 무작위 정보가 컨텍스트에 들어가는 것을 방지합니다. 정상적인 값은 0.2(더 느슨함)와 0.5(더 집중) 사이의 범위에 있습니다.
- Chunk boundary - 파일을 청크로 분할할 때 우선순위가 지정될 사용자 정의 문자열입니다. 지정하지 않으면 기본값은 (순서대로) 이중 줄 바꿈, 단일 줄 바꿈, 단어 사이의 공백으로 분할하는 것입니다.
- Only chunk on custom boundary - 활성화되면 지정된 청크 경계에서만 청킹이 발생합니다. 그렇지 않으면 청킹은 기본 경계에서도 발생합니다.
- Translate files into English before processing - 활성화되면 [Chat Translation](/extensions/Translation.md) 확장 프로그램에서 구성된 번역 API를 사용하여 파일을 처리하기 전에 영어로 번역합니다. 영어 텍스트만 지원하는 임베딩 모델을 사용할 때 유용합니다.
- Include in World Info Scanning - 삽입된 콘텐츠가 로어북 항목을 활성화하도록 하려면 확인하세요.
- Vectorize All - 처리되지 않은 모든 파일에 대한 임베딩을 강제로 수집합니다.
- Purge Vectors - 파일 임베딩을 지우고 벡터를 다시 계산할 수 있도록 합니다.

!!!info 참고
"Chat vectorization" 설정은 [Chat Vectorization](/extensions/Chat-vectorization.md)을 참조하세요.
!!!

## 결론

축하합니다! 이제 채팅 경험이 RAG의 힘으로 향상되었습니다. 그 기능은 상상력에 의해서만 제한됩니다. 항상 그렇듯이 실험하는 것을 두려워하지 마세요!
