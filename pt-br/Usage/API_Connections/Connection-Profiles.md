---
icon: paperclip
route: /usage/core-concepts/connection-profiles/
order: 100
---

# Connection Profiles

Salve Connection Profiles para alternar rapidamente entre diferentes APIs, modelos e templates de formatação. Isso é útil quando você usa ativamente várias conexões de API ou precisa alternar entre diferentes configurações sem navegar pelos menus.

## Acessando Connection Profiles

Este recurso está habilitado por padrão a partir do SillyTavern 1.12.6 ou posterior como uma extensão integrada, e disponível no menu API Connections. Se você deseja *desabilitá-lo*, abra o painel Extensions, clique em "Manager extensions", localize Connection Profiles na lista, desmarque a caixa de seleção "Enabled" e clique em "Close".

## O Que é Salvo

Connection Profiles armazenam as seguintes seleções.

### Common

* [Tipo de API, modelo e URL do servidor](/Usage/API_Connections/index.md)
* [Secret Key](/Usage/faq.md#where-are-my-api-keys-stored-why-cant-i-see-them)
* [Predefinição de configurações](/Usage/Common-Settings.md)
* [Start Reply With](/Usage/Prompts/advancedformatting.md#start-reply-with) (pode estar explicitamente vazio)
* [Custom Stopping Strings](/Usage/Prompts/advancedformatting.md#custom-stopping-strings) (pode estar explicitamente vazio)
* [Reasoning Formatting](/Usage/Prompts/reasoning.md#configuration)

### Text Completion APIs

* [System Prompt e seu estado](/Usage/Prompts/advancedformatting.md#system-prompt)
* [Estado e template do Instruct Mode](/Usage/Prompts/instructmode.md)
* [Context Template](/Usage/Prompts/advancedformatting.md#context-template)
* [Tokenizer](/Usage/Prompts/advancedformatting.md#tokenizer)

### Chat Completion APIs

* [Prompt Post-Processing](/Usage/API_Connections/openai.md#prompt-post-processing)
* Predefinição de proxy

## Gerenciando Connection Profiles

!!!info Nota
Profiles salvam apenas a seleção nos campos suspensos, sem saber nada sobre as configurações subjacentes. Isso significa que você perderá alterações não salvas ao alternar para um perfil diferente. Para evitar isso, certifique-se de atualizar todas as predefinições e templates se não quiser perder alterações efêmeras.
!!!

* Para salvar um perfil, defina todas as configurações necessárias e clique no botão "Create". Em seguida, revise as configurações e forneça um nome para o perfil. **O nome deve ser único.**
* Para visualizar as informações detalhadas sobre um perfil escolhido, clique no botão "Information". Clique novamente para ocultar os detalhes.
* As configurações de Connection Profile são salvas em `settings.json` sem alterar o arquivo de salvamento do perfil associado até você pressionar o botão "Update". Isso significa que se você configurar um perfil, mas depois alternar para um diferente sem atualizar, você perderá todas as suas alterações anteriores.
* Para restaurar as seleções alteradas de um perfil salvo, clique no botão "Reload".
* Para excluir um perfil, clique no botão "Delete" e confirme a exclusão. **Esta ação é irreversível.**

## Slash Commands

Connection profiles podem ser gerenciados usando os seguintes slash commands.

1. `/profile [name]` - alterna para um perfil se o argumento for fornecido, ou obtém o nome do perfil atual se não.
2. `/profile-create [name]` - salva as configurações atuais como um novo perfil com o nome fornecido.
3. `/profile-list` - retorna um array serializado em JSON de nomes de perfis disponíveis.
4. `/profile-get [name]` - obtém os detalhes do perfil com o nome fornecido como um objeto serializado em JSON.
5. `/profile-update` - atualiza o perfil selecionado com as configurações atuais.
