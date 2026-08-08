---
route: /ko/extensions/websearch/
---

# 웹 검색

LLM 프롬프트에 웹 검색 결과를 추가합니다.

!!! Note
일부 [Chat Completion](/Usage/API_Connections/openai.md) 소스는 내장 웹 검색 기능을 제공합니다. 이 경우 이 확장 기능은 대부분 중복됩니다. **<i class="fa-solid fa-sliders"></i> AI Response Configuration** 패널에서 "Enable web search" 토글을 확인하세요. 예를 들어 Claude, Google AI Studio / Vertex AI, OpenRouter, Chutes 및 기타 백엔드에 사용할 수 있습니다.
!!!

## 사용 가능한 소스

### Selenium Plugin

공식 서버 플러그인을 설치하고 활성화해야 합니다.

자세한 내용은 [SillyTavern-WebSearch-Selenium](https://github.com/SillyTavern/SillyTavern-WebSearch-Selenium)을 참조하세요.

Google 및 DuckDuckGo 엔진을 지원합니다.

### Extras API

`websearch` 모듈과 호스트 머신에 설치된 Chrome/Firefox 웹 브라우저가 필요합니다.

Google 및 DuckDuckGo 엔진을 지원합니다.

### SerpApi

API 키가 필요합니다.

여기에서 키를 받으세요: <https://serpapi.com/dashboard>

### SearXNG

SearXNG 인스턴스 URL(비공개 또는 공개)이 필요합니다. 검색 결과에 HTML 형식을 사용합니다.

SearXNG preferences 문자열: SearXNG에서 얻음 - preferences - COOKIES - Copy preferences hash

자세히 알아보기: <https://docs.searxng.org/>

### Tavily AI

API 키가 필요합니다.

여기에서 키를 받으세요: <https://app.tavily.com/>

### KoboldCpp

KoboldCpp URL은 Text Completion API 설정에서 제공해야 합니다. KoboldCpp 버전은 >= 1.81.1이어야 하며 시작 시 WebSearch 모듈을 활성화해야 합니다: GUI 런처에서 Network => Enable WebSearch를 활성화하거나 명령줄에 `--websearch`를 추가합니다.

참조: <https://github.com/LostRuins/koboldcpp/releases/tag/v1.81.1>

### Serper

API 키가 필요합니다.

여기에서 키를 받으세요: <https://serper.dev/>

### Z.AI

API 키가 필요하며 먼저 Chat Completion API 설정에서 설정해야 합니다. Coding API 구독과는 호환되지 않습니다!

여기에서 키를 받으세요: <https://z.ai/manage-apikey/apikey-list/>

문서: <https://docs.z.ai/api-reference/tools/web-search>

## 사용 방법

1. SillyTavern의 최신 버전을 사용하고 있는지 확인하세요.
2. SillyTavern의 "Download Extensions & Assets" 메뉴를 통해 확장 기능을 설치합니다.
3. "Web Search" 확장 기능 설정을 열고 API 키를 설정하거나 Extras에 연결한 다음 확장 기능을 활성화합니다.
4. 채팅할 때 웹 검색 결과가 프롬프트에 유기적으로 추가됩니다. **사용자 메시지만 검색을 트리거합니다.**
5. 검색 결과를 더 유기적으로 포함하려면 검색 쿼리를 단일 백틱으로 묶으세요: ```Tell me about the `latest Ryan Gosling movie`.```는 검색 쿼리 `latest Ryan Gosling movie`를 생성합니다.
6. 선택적으로 원하는 대로 설정을 구성합니다.

## 설정

### General

1. Enabled - 확장 기능을 켜고 끕니다.
2. Sources = 검색 결과 소스를 설정합니다.
3. Cache Lifetime - 프롬프트에 대해 검색 결과가 캐시되는 시간(초). 기본값 = 1주일.

### Prompt Settings

1. Prompt Budget - 삽입된 텍스트의 최대 용량을 설정합니다(토큰이 아닌 텍스트 문자). 경험 법칙: 1 토큰 ~ 3-4자, 모델의 컨텍스트 제한에 따라 조정합니다. 기본값 = 1500자.
2. Insertion Template - 결과가 프롬프트에 삽입되는 방법. 일반적인 매크로 + 특수 매크로 지원: 검색 쿼리의 경우 \{\{query\}\}, 검색 결과의 경우 \{\{text\}\}.
3. Injection Position - 프롬프트에서 결과가 가는 위치. Author's Note와 동일한 옵션: in-chat injection 또는 before/after system prompt.

### Search Activation

1. Use function tool - [function calling](/For_Contributors/Function-Calling.md)을 사용하여 검색을 활성화하거나 웹 페이지를 스크랩합니다. 지원되는 Chat Completion API를 사용하고 AI Response 설정에서 활성화해야 합니다. **참여할 때 다른 모든 활성화 방법을 비활성화합니다.**
2. Use Backticks - 단일 백틱으로 묶인 단어를 사용하여 검색 활성화를 활성화합니다.
3. Use Trigger Phrases - 트리거 구문을 사용하여 검색 활성화를 활성화합니다.
4. Regular expressions - 사용자 메시지를 일치시키기 위해 JS 스타일 정규식을 제공합니다. 정규식이 일치하면 지정된 쿼리로 검색이 트리거됩니다. 검색 쿼리는 `{{macros}}`와 일치하는 그룹을 참조하기 위한 $1 구문을 지원합니다. 예: 검색 쿼리 `news in $1`에 대한 `/what is happening in (.*)/i` 정규식은 `what is happening in New York`를 포함하는 메시지를 일치시키고 쿼리 `news in New York`로 검색을 트리거합니다.
5. Trigger Phrases - 검색을 트리거할 구문을 하나씩 추가합니다. 메시지 어디든 있을 수 있으며 쿼리는 트리거 단어에서 시작하여 "Max Words" 합계까지 걸쳐 있습니다. 처리에서 특정 메시지를 제외하려면 마침표로 시작해야 합니다. 예: `.What do you think?`. 트리거의 우선순위: 먼저 텍스트 상자의 순서대로, 그런 다음 사용자 메시지의 첫 번째.
6. Max Words - 검색 쿼리에 포함되는 단어 수(트리거 구문 포함). Google은 프롬프트당 약 32단어의 제한이 있습니다. 기본값 = 10단어.

### Page Scraping

1. Visit Links - 방문한 검색 결과 페이지에서 텍스트가 추출되어 파일 첨부 파일에 저장됩니다.
2. Visit Count - 텍스트를 위해 방문하고 구문 분석할 링크 수.
3. Visit Domain Blacklist - 방문에서 제외할 사이트 도메인. 한 줄에 하나씩.
4. File Header - 텍스트 파일의 시작 부분에 삽입되는 파일 헤더 템플릿, 추가 \{\{query\}\} 매크로 포함.
5. Block Header - 모든 링크의 구문 분석된 콘텐츠와 함께 삽입되는 링크 블록 템플릿. 페이지 URL에 \{\{link\}\} 매크로, 페이지 콘텐츠에 \{\{text\}\} 사용.
6. Save Target - 스크래핑 결과를 저장할 위치. 가능한 옵션: 트리거 메시지 첨부 파일, Data Bank의 채팅 첨부 파일 또는 이미지만(소스가 지원하는 경우).
7. Include Images - 관련 이미지를 채팅에 첨부합니다. 이미지를 지원하는 소스가 필요합니다(아래 참조).

## 추가 정보

최신 쿼리의 검색 결과는 다음 유효한 쿼리를 찾을 때까지 프롬프트에 포함된 상태로 유지됩니다.
실수로 검색을 트리거하지 않고 추가 질문을 하려면 메시지를 마침표로 시작하세요.

!!!info
Web Search function tool은 활성화되고 사용 가능한 경우 항상 다른 트리거를 재정의합니다.
!!!

트리거의 우선순위(여러 개가 활성화된 경우):

1. 백틱.
2. 정규식.
3. 트리거 구문.

처리에서 이전 쿼리를 모두 삭제하려면 사용자 메시지를 느낌표로 시작하세요. 예를 들어 사용자 메시지 `!Now let's talk about...`는 이것과 그 위의 모든 메시지를 삭제합니다.

이 확장 기능은 STscript에서 사용할 `/websearch` slash 명령도 제공합니다. 자세한 정보는 여기: [STscript Language Reference](/For_Contributors/st-script.md#extension-commands)

```stscript
/websearch (links=on|off snippets=on|off [query]) – 웹 검색 쿼리를 수행합니다. 명명된 인수를 사용하여 반환할 내용(페이지 스니펫(기본값: on), 전체 구문 분석된 페이지(기본값: off) 또는 둘 다)을 지정합니다.

예: /websearch links=off snippets=on how to make a sandwich
```

### 검색 결과에 무엇이 포함될 수 있나요?

**용어집:**

- Answer box: 질문에 대한 직접적인 답변.
- Knowledge graph: 주제에 대한 백과사전 지식.
- Page snippets: 웹 페이지의 관련 발췌.
- Relevant questions: 유사한 주제에 대한 질문과 답변.
- Images: 관련 이미지.

#### SerpApi

1. Answer box.
2. Knowledge graph.
3. Page snippets (최대 10개).
4. Relevant questions (최대 10개).
5. Images (최대 10개).

#### Selenium Plugin 및 Extras API

1. Google - answer box, knowledge graph, page snippets.
2. DuckDuckGo - page snippets.

**Selenium Plugin**은 추가로 이미지를 제공할 수 있습니다.

#### SearXNG

1. Infobox.
2. Page snippets.
3. Images.

#### Tavily AI

1. Answer.
2. Page contents.
3. Images (최대 5개).

#### KoboldCpp

1. Page titles.
2. Page snippets.

#### Serper

1. Answer box.
2. Knowledge graph.
3. Page snippets.
4. Relevant questions.
5. Images.

#### Z.AI

1. Page titles.
2. Page snippets.
