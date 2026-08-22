---
route: /ko/usage/api-connections/google/
label: Google Gemini
title: Google Gemini
---

# Google Gemini

Gemini는 Google AI Studio(이전 MakerSuite) 및 Google Vertex AI를 포함한 여러 API를 통해 사용할 수 있는 Google의 최첨단 다중 모드 LLM입니다. 이 가이드는 SillyTavern에서 Gemini API 연결을 설정하는 데 도움이 됩니다.

## Google AI Studio

AI Studio는 Google Cloud Platform(GCP) 프로젝트를 설정할 필요 없이 최신 Google AI 모델을 시험해 볼 수 있는 가장 빠르고 사용자 친화적인 방법입니다. Gemini 모델에 액세스하는 데 사용할 수 있는 간단한 API 키를 제공합니다.

### 1단계: Google AI Studio 키 만들기

1. [Google AI Studio](https://aistudio.google.com/apikey) 페이지로 이동하여 Google 계정으로 로그인합니다.
2. "Get API Key"를 클릭하고 이용 약관에 동의합니다.
3. "Create API Key"를 클릭하여 API 키를 생성합니다.
4. API 키를 클립보드에 복사합니다.

### 2단계: SillyTavern에 API 키 입력

1. SillyTavern에서 "API Connections" 페이지로 이동합니다.
2. API 유형으로 "Chat Completion"을 선택합니다.
3. 드롭다운 메뉴에서 "Google AI Studio"를 선택합니다.
4. 이전에 복사한 API 키를 "API Key" 텍스트 상자에 입력합니다.
5. "Connect" 버튼을 클릭하여 키를 저장합니다.

이제 SillyTavern에서 Google AI Studio API를 사용할 수 있습니다.

## Google Vertex AI

Vertex AI는 Google Cloud Platform(GCP)에서 제공하는 서비스입니다. Gemini 시리즈를 포함한 다양한 AI 모델에 대한 액세스를 제공합니다.

Vertex AI API를 설정하는 방법에는 여러 가지가 있으며 사용 가능한 모델은 사용된 방법에 따라 다를 수 있습니다.

### 서비스 계정

Google Cloud Platform(GCP)은 Vertex AI에 액세스하기 위해 서비스 계정이 필요하며 간단한 API 키는 작동하지 않습니다. 서비스 계정 JSON 파일에서 토큰이 생성되고 이를 사용하여 Vertex AI API에 대한 요청을 인증합니다.

다음 단계에 따라 서비스 계정을 만들 수 있습니다:

**전제 조건:**

1. Google Cloud Platform(GCP) 계정이 있어야 합니다.
2. GCP 계정 내에 프로젝트가 생성되어 있어야 합니다.
3. 해당 프로젝트에 대해 청구가 활성화되어 있어야 합니다.

#### 1단계: Vertex AI API 활성화

키가 작동하려면 프로젝트에 대해 API가 활성화되어 있어야 합니다.

1. Google Cloud Console로 이동합니다: <https://console.cloud.google.com/>
2. 상단 바에서 올바른 프로젝트가 선택되어 있는지 확인하세요.
3. Vertex AI API 페이지로 이동합니다: <https://console.cloud.google.com/apis/library/aiplatform.googleapis.com>
4. 아직 활성화되지 않은 경우 "Enable" 버튼을 클릭합니다.

#### 2단계: 서비스 계정 만들기

Vertex AI API에 액세스하는 데 사용될 ID입니다.

1. Google Cloud Console에서 "Service Accounts" 페이지로 이동합니다. 상단 검색 바에서 검색하거나 이 직접 링크를 사용할 수 있습니다: <https://console.cloud.google.com/iam-admin/serviceaccounts>
2. GCP 프로젝트를 선택하고 "+ CREATE SERVICE ACCOUNT"를 클릭합니다.
3. 서비스 계정 이름: `my-vertex-ai-client`와 같이 설명적인 이름을 지정합니다.
4. "CREATE AND CONTINUE"를 클릭합니다.
5. 이 서비스 계정에 프로젝트 액세스 권한 부여: "Role" 드롭다운에서 Vertex AI User를 검색하고 선택합니다. 이 역할은 너무 많은 액세스 권한을 부여하지 않고 모델을 실행하는 데 필요한 권한을 부여합니다.
6. "CONTINUE"를 클릭한 다음 "DONE"을 클릭합니다.

#### 3단계: JSON 키 생성

이것은 필요한 "비밀번호" 파일입니다. 중요한 정보가 포함되어 있으므로 공유하거나 공개적으로 업로드하지 마세요.

1. 이제 서비스 계정 목록으로 돌아가야 합니다. 방금 만든 계정을 찾습니다(예: sillytavern-vertex-ai).
2. 해당 행의 맨 오른쪽에 있는 점 3개 메뉴(⋮)를 클릭하고 "Manage keys"를 선택합니다.
3. "ADD KEY" -> "Create new key"를 클릭합니다.
4. Key type이 JSON으로 설정되어 있는지 확인합니다.
5. "CREATE"를 클릭합니다.

.json 파일이 즉시 컴퓨터에 다운로드됩니다. 분실하면 복구할 수 없으므로 안전하게 보관하세요.

#### 4단계: SillyTavern에 JSON 내용 입력

다운로드한 JSON 파일에는 Vertex AI API로 인증하는 데 필요한 모든 정보가 포함되어 있습니다. 다음과 같이 보일 것입니다:

```json
{
    "type": "service_account",
    "project_id": "your-gcp-project-name",
    "private_key_id": "...",
    "private_key": "-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n",
    "client_email": "sillytavern-vertex-ai@your-gcp-project-name.iam.gserviceaccount.com",
    "client_id": "...",
    "auth_uri": "https://accounts.google.com/o/oauth2/auth",
    "token_uri": "https://oauth2.googleapis.com/token",
    "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
    "client_x509_cert_url": "..."
}
```

1. 방금 다운로드한 .json 파일을 간단한 텍스트 편집기(Windows의 메모장, Mac의 TextEdit 또는 VS Code)로 엽니다.
2. 파일의 모든 텍스트를 선택합니다(Ctrl+A 또는 Cmd+A).
3. 텍스트를 클립보드에 복사합니다(Ctrl+C 또는 Cmd+C).
4. SillyTavern에서 "API Connections" 페이지로 이동하고 API 유형으로 "Chat Completion"을 선택한 다음 드롭다운 메뉴에서 "Google Vertex AI"를 선택합니다. 인증 방법을 "Service Account"로 전환합니다.
5. 복사한 전체 내용을 "Service Account JSON Content" 텍스트 상자에 붙여넣습니다.
6. "Validate JSON" 버튼을 클릭하여 올바르게 복사했는지 확인합니다.
7. 마지막으로 아래로 스크롤하여 API 설정 페이지 하단의 "Connect"를 클릭합니다.

이제 SillyTavern에서 Google Vertex AI API를 사용할 수 있습니다.

### Express Mode

Express 모드는 Google Cloud에서 Generative AI를 시작하는 가장 빠른 방법입니다. 서비스 계정을 설정할 필요 없이 Gemini API를 사용할 수 있습니다. 대신 API 키를 직접 사용할 수 있습니다.

자세한 내용은 공식 문서를 참조하세요: [Vertex AI in express mode overview](https://cloud.google.com/vertex-ai/generative-ai/docs/start/express-mode/overview).

#### 1단계: 계정이 Express Mode에 적격한지 확인

이전에 Google Cloud 프로젝트를 만드는 데 사용하지 않은 Google 계정이 있어야 합니다.
기존 Google Cloud 프로젝트(무료 평가판 포함)가 있는 경우 이 목적으로 새 프로젝트를 만들 수 있습니다.

#### 2단계: Vertex AI Express Mode 활성화

1. 다음 웹 페이지로 이동합니다: [Vertex AI Studio](https://cloud.google.com/generative-ai-studio).
2. "Try it free"를 클릭합니다.
3. 이용 약관에 동의하고 Google 계정으로 로그인합니다.
4. 국가를 선택하고 "Agree & start free"를 클릭합니다. 설정이 완료될 때까지 기다립니다.

#### 3단계: API 키 만들기

1. Google Cloud 콘솔이 Express Mode에서 실행 중인지 확인합니다. 페이지 왼쪽 상단에 배너가 표시되어야 합니다.
2. 왼쪽 사이드바의 "API Keys" 링크를 클릭합니다.
3. "Create API Key" 버튼을 클릭합니다.
4. 새 API 키가 생성됩니다. 이 키를 클립보드에 복사합니다.

#### 4단계: SillyTavern에 API 키 입력

1. SillyTavern에서 "API Connections" 페이지로 이동합니다.
2. API 유형으로 "Chat Completion"을 선택합니다.
3. 드롭다운 메뉴에서 "Google Vertex AI"를 선택합니다.
4. 인증 방법을 "Express Mode (API Key)"로 전환합니다.
5. 이전에 복사한 API 키를 "API Key" 텍스트 상자에 붙여넣습니다.
6. "Connect" 버튼을 클릭하여 키를 저장합니다.

이제 Express Mode에서 SillyTavern에서 Google Vertex AI API를 사용할 수 있습니다.
