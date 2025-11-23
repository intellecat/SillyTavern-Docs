---
route: /usage/api-connections/google/
---

# Google Gemini

Gemini é o LLM multimodal de ponta do Google, que está disponível através de várias APIs, incluindo Google Vertex AI e Google AI Studio (anteriormente MakerSuite). Este guia ajudará você a configurar as conexões da API Gemini no SillyTavern.

## Google AI Studio

AI Studio é a maneira mais rápida e amigável de experimentar os modelos de IA mais recentes do Google sem precisar configurar um projeto do Google Cloud Platform (GCP). Ele fornece uma chave de API simples que você pode usar para acessar os modelos Gemini.

### Passo 1: Criar uma Chave do Google AI Studio

1. Vá para a página [Google AI Studio](https://aistudio.google.com/apikey) e entre com sua conta Google.
2. Clique em "Get API Key", aceite os termos e condições.
3. Clique em "Create API Key" para gerar sua chave de API.
4. Copie a chave de API para sua área de transferência.

### Passo 2: Colocar a Chave de API no SillyTavern

1. No SillyTavern, vá para a página "API Connections".
2. Selecione "Chat Completion" como o tipo de API.
3. Selecione "Google AI Studio" no menu suspenso.
4. Insira a chave de API que você copiou anteriormente na caixa de texto "API Key".
5. Clique no botão "Connect" para salvar a chave.

Agora você deve ser capaz de usar a API do Google AI Studio com o SillyTavern.

## Google Vertex AI

Vertex AI é um serviço fornecido pelo Google Cloud Platform (GCP). Ele fornece acesso a vários modelos de IA, incluindo a série Gemini.

Existem várias maneiras de configurar uma API Vertex AI, e os modelos disponíveis podem variar dependendo do método usado.

### Service Account

Google Cloud Platform (GCP) requer uma conta de serviço para acessar Vertex AI, chaves de API simples não funcionarão. Um token será gerado a partir do arquivo JSON da conta de serviço, que será então usado para autenticar solicitações para a API Vertex AI.

Você pode criar uma conta de serviço seguindo estas etapas:

**Pré-requisitos:**

1. Você deve ter uma conta Google Cloud Platform (GCP).
2. Você deve ter um projeto criado dentro da sua conta GCP.
3. Você deve ter faturamento habilitado para esse projeto.

#### Passo 1: Habilitar a API Vertex AI

Antes que sua chave possa funcionar, a API deve ser habilitada para o seu projeto.

1. Vá para o Google Cloud Console: <https://console.cloud.google.com/>
2. Certifique-se de que o projeto correto está selecionado na barra superior.
3. Navegue até a página da API Vertex AI: <https://console.cloud.google.com/apis/library/aiplatform.googleapis.com>
4. Se ainda não estiver habilitada, clique no botão "Enable".

#### Passo 2: Criar a Service Account

Esta é a identidade que será usada para acessar a API Vertex AI.

1. No Google Cloud Console, navegue até a página "Service Accounts". Você pode procurá-la na barra de pesquisa superior ou usar este link direto: <https://console.cloud.google.com/iam-admin/serviceaccounts>
2. Selecione seu projeto GCP e clique em "+ CREATE SERVICE ACCOUNT".
3. Service account name: Dê a ela um nome descritivo, como `my-vertex-ai-client`.
4. Clique em "CREATE AND CONTINUE".
5. Grant this service account access to project: No menu suspenso "Role", procure e selecione Vertex AI User. Esta função concede as permissões necessárias para executar modelos sem dar muito acesso.
6. Clique em "CONTINUE" e depois clique em "DONE".

#### Passo 3: Gerar a Chave JSON

Este é o arquivo de "senha" que você precisa. Ele contém informações sensíveis, então não o compartilhe ou faça upload em nenhum lugar público.

1. Agora você deve estar de volta à lista Service Accounts. Encontre a conta que você acabou de criar (por exemplo, sillytavern-vertex-ai).
2. Clique no menu de três pontos (⋮) no lado direito dessa linha e selecione "Manage keys".
3. Clique em "ADD KEY" -> "Create new key".
4. Certifique-se de que o Key type está definido como JSON.
5. Clique em "CREATE".

Um arquivo .json será imediatamente baixado para o seu computador. Mantenha-o seguro, porque esta chave não pode ser recuperada se perdida.

#### Passo 4: Colocar o Conteúdo JSON no SillyTavern

O arquivo JSON que você baixou contém todas as informações necessárias para autenticar com a API Vertex AI. Ele se parecerá com algo assim:

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

1. Abra o arquivo .json que você acabou de baixar com um editor de texto simples (como Notepad no Windows, TextEdit no Mac, ou VS Code).
2. Selecione todo o texto no arquivo (Ctrl+A ou Cmd+A).
3. Copie o texto para sua área de transferência (Ctrl+C ou Cmd+C).
4. No SillyTavern, vá para a página "API Connections", selecione "Chat Completion" como o tipo de API e, em seguida, selecione "Google Vertex AI" no menu suspenso. Mude o método de autenticação para "Service Account".
5. Cole todo o conteúdo copiado na caixa de texto "Service Account JSON Content".
6. Clique no botão "Validate JSON" para ter certeza de que você copiou corretamente.
7. Finalmente, role para baixo e clique em "Connect" na parte inferior da página de configurações da API.

Agora você deve ser capaz de usar a API Google Vertex AI com o SillyTavern.

### Express Mode

Express mode é a maneira mais rápida de começar a usar Generative AI no Google Cloud. Ele permite que você use a API Gemini sem precisar configurar uma conta de serviço. Em vez disso, você pode usar uma chave de API diretamente.

Veja a documentação oficial para mais detalhes: [Vertex AI in express mode overview](https://cloud.google.com/vertex-ai/generative-ai/docs/start/express-mode/overview).

#### Passo 1: Garantir que sua conta é elegível para Express Mode

Você deve ter uma conta Google que não foi usada anteriormente para criar um projeto Google Cloud.
Se você tem um projeto Google Cloud existente (incluindo testes gratuitos), pode criar um novo para este propósito.

#### Passo 2: Ativar o Vertex AI Express Mode

1. Vá para a seguinte página web: [Vertex AI Studio](https://cloud.google.com/generative-ai-studio).
2. Clique em "Try it free".
3. Aceite os termos e condições e entre com sua conta Google.
4. Escolha seu país e clique em "Agree & start free". Aguarde a conclusão da configuração.

#### Passo 3: Criar uma Chave de API

1. Verifique se o seu Google Cloud console está sendo executado no Express Mode. Você deve ver um banner no canto superior esquerdo da página.
2. Clique no link "API Keys" na barra lateral esquerda.
3. Clique no botão "Create API Key".
4. Uma nova chave de API será gerada. Copie esta chave para sua área de transferência.

#### Passo 4: Colocar a Chave de API no SillyTavern

1. No SillyTavern, vá para a página "API Connections".
2. Selecione "Chat Completion" como o tipo de API.
3. Selecione "Google Vertex AI" no menu suspenso.
4. Mude o método de autenticação para "Express Mode (API Key)".
5. Cole a chave de API que você copiou anteriormente na caixa de texto "API Key".
6. Clique no botão "Connect" para salvar a chave.

Agora você deve ser capaz de usar a API Google Vertex AI no Express Mode com o SillyTavern.
