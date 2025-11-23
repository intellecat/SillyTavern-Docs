---
route: /extensions/objective/
---

# Objective

### O que é isso?

A extensão Objective permite que o usuário especifique um Objetivo para a IA se esforçar durante o chat. Este objetivo é dividido em tarefas passo a passo. As tarefas podem ser ramificadas, onde tarefas filhas podem ser criadas automaticamente ou manualmente. Isso dá a capacidade de criar árvores de tarefas complexas. O status de conclusão de cada tarefa na lista será verificado em certos intervalos.

Isso difere de adicionar direção estática através de prompting, pois adiciona diretivas sequenciais e ritmadas para a IA seguir sem intervenção do usuário. Dá uma experiência mais genuína da IA se esforçando autonomamente para alcançar um objetivo.

### Pré-requisitos

Antes de começar, certifique-se de ter atendido aos seguintes pré-requisitos:

- Certifique-se de estar usando a versão mais recente do SillyTavern.
- Instale a extensão "Objective" no menu "Download Extensions & Assets" no painel Extensions (ícone de blocos empilhados).

### Casos de Uso Comuns

Sua imaginação é o limite, você pode dar à IA qualquer objetivo que desejar e ela planejará como alcançá-lo. Você pode pedir que ela planeje como matar um demônio, roubar um templo, dar uma festa luxuosa ou até conquistar o mundo.

![Objective Settings Panel](/static/extensions/objective-panel.png)

### Configuração

- A extensão é encontrada no menu Extensions em Objective.

- Digite um objetivo na caixa de texto superior e depois clique em `Auto-Generate Tasks`. Isso envia uma solicitação à API conectada e pede que ela forneça uma lista de tarefas que correspondam ao objetivo que você digitou.

*Nota: Clicar em Auto-Generate Tasks excluirá todas as tarefas existentes para o Objetivo atualmente selecionado antes de adicionar novas.*

- Ao receber a resposta da IA, uma lista de tarefas será criada automaticamente no espaço abaixo da caixa de entrada Objective. As tarefas podem ser editadas após a criação.

- Na parte inferior do painel estão duas caixas: `Position in Chat` e `Task Check Frequency`
  - `Position in Chat` - Isso é o quão 'profundo' na seção de chat do prompt você quer que a tarefa atual seja inserida. Quanto menor o número, mais atenção a IA dará à tarefa. Definir como 0 fará da tarefa a coisa principal na mente da IA. Definir em valores altos colocará a tarefa em segundo plano e permitirá que a IA se concentre na conversa em questão, mas defini-la muito alta pode fazer com que a IA nunca 'chegue à' tarefa.
  - `Task Check Frequency` - Isso é com que frequência você quer que a IA verifique se a tarefa foi concluída. Se estiver definido como `3`, a IA será perguntada se a tarefa atual foi concluída a cada 3ª mensagem.

- Objetivos, tarefas e suas descrições são salvos em tempo real na sessão de chat atual. Prompts personalizados são salvos globalmente.

### Prompts Personalizados
Você pode personalizar os prompts enviados ao LLM para gerar tarefas, verificar conclusão de tarefas e para injeção de prompt. Editar prompts os salvará para a sessão atual. Prompts personalizados podem ser salvos e carregados para persistência.

- Clique em Edit Prompts para abrir a janela do editor de prompts. Você pode editar seus prompts conforme desejado.
- Para salvar prompts, insira um nome e clique em Save Prompt.
- Para carregar prompts, selecione o prompt na lista dropdown.
- Para excluir um prompt salvo, selecione-o na lista dropdown e clique em Delete Prompt

**AVISO: A Verificação de Tarefas acontece em uma solicitação de API separada. Definir Task Check Frequency como 1 dobrará suas chamadas de API para o serviço LLM. Tenha cuidado com isso se estiver usando um serviço pago.**

### Uso

Por padrão, a extensão Objective manterá o controle de todas as tarefas e seus respectivos status de conclusão automaticamente.

O Usuário também pode criar, atualizar, excluir e concluir tarefas manualmente a qualquer momento.


#### Seleção de Tarefa Atual

A tarefa atual sempre será a primeira tarefa incompleta listada. Quaisquer atualizações manuais nas tarefas acionarão uma verificação de qual deve ser a tarefa atual. Então, se você adicionar uma tarefa acima de um monte de tarefas concluídas, ela será definida como a tarefa atual. Uma vez concluída, tarefas anteriormente concluídas serão ignoradas e a próxima tarefa incompleta será selecionada como 'Current'.

Ao usar tarefas pai/filho em uma árvore de tarefas, as tarefas são selecionadas em profundidade primeiro, o que significa que todas as tarefas filhas serão selecionadas em ordem primeiro, depois continuarão pela lista de tarefas para o Objetivo/Tarefa atual.

#### Ramificar Tarefas

Clique no botão Branch Task para definir a tarefa atual como um Objetivo onde você pode gerar automaticamente ou criar manualmente tarefas como tarefas filhas. Você pode continuar a transformar qualquer tarefa filha em um Objetivo e continuar gerando à vontade.

Marcar uma tarefa pai como concluída fará com que a extensão ignore todas as subtarefas. Quando todas as tarefas filhas estiverem concluídas, a tarefa pai será marcada como concluída

#### Concluir Tarefas Manualmente

Você pode alternar manualmente o status de conclusão de uma tarefa `clicando na caixa de seleção` ao lado dela. Isso definirá a próxima tarefa incompleta a ser selecionada.

#### Verificação Manual de Tarefa

Se você quiser acionar manualmente a IA para verificar a conclusão da tarefa, clique no botão Extras Extension (a `varinha mágica` no lado direito da barra de entrada de chat) e selecione `Manual Task Check`.

![Manual Task Check](/static/extensions/task-check.png)

#### Adicionar Tarefas Manualmente

Quando nenhuma tarefa está presente, um botão `Add Task` é visível, permitindo criar manualmente a primeira tarefa.

Se outras tarefas já estiverem presentes, clique no botão `+` à direita de qualquer tarefa para inserir uma nova tarefa depois dela.

#### Excluir Tarefas

Clique no `x` vermelho para excluir uma tarefa existente. A próxima tarefa incompleta será selecionada como a tarefa atual automaticamente.

Excluir uma tarefa com tarefas filhas excluirá todas as tarefas filhas e seus descendentes.

#### Ocultar Tarefas

Se você quiser permanecer inconsciente de quais tarefas a IA está tentando concluir, marque a caixa `Hide Tasks` para ocultar a lista de tarefas e tornar as intenções da IA um mistério. Para 100% de mistério, faça isso antes de clicar em `Auto-Generate Tasks`!
