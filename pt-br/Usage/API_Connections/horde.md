---
route: /usage/api-connections/horde/
---

# AI Horde

## Aviso Legal

- AI Horde é um cluster de GPU distribuído e colaborativo executado inteiramente por voluntários.
- Por padrão, suas entradas são enviadas anonimamente e as respostas não podem ser vistas pela pessoa que executa o Horde Worker.
- No entanto, como é um programa de código aberto, Workers Maliciosos podem modificar o código para:
  - registrar sua atividade (prompts de entrada, respostas da IA).
  - produzir respostas ruins ou ofensivas.

!!!warning
Ao usar Horde **nunca envie** qualquer informação pessoal como nomes, endereços de e-mail, etc.
!!!

Ativar a caixa de seleção "Trusted Workers Only" limitará a seleção de workers disponíveis apenas àqueles que hospedam no Horde há algum tempo e são geralmente considerados confiáveis. Mas eles ainda podem estar vendo prompts, por exemplo, hospedando usando software não contabilizado.

Para ajudar a reduzir este problema, o SillyTavern integrou o seguinte recurso:

- Quando uma resposta de chat é gerada por um Horde Worker, o SillyTavern registra o ID do Worker e qual modelo eles estavam usando.
- Essas informações podem ser vistas passando o cursor do mouse sobre o item de chat (veja a imagem abaixo).
- Se você acredita que recebeu uma resposta maliciosa, pode passar essas informações para o administrador do Horde no [AI Horde Discord](https://discord.gg/3DxrhksKzn) para revisão e possível ação disciplinar contra esse Worker.

![Horde Worker Info Popup](/static/horde-worker.png)

## Configuração

- O SillyTavern é capaz de se conectar com o Horde pronto para uso sem configuração adicional necessária.
- Selecione 'AI Horde' no Seletor Suspenso de API no Painel de API do ST.
- Selecione um ou mais Modelos ('cérebros de IA' para os personagens) do Seletor de Modelos na parte inferior do painel.
- Selecione um personagem e comece a conversar.

![ST Kobold Horde API Connection Panel](/static/horde-config.png)

!!!warning
Por padrão, sua instância do SillyTavern se conecta à 'conta de convidado' de baixa prioridade do Horde.
Isso significa que você pode ter que esperar muito tempo por uma resposta.
Para reduzir os tempos de espera, siga as dicas abaixo.
!!!

## Dicas

- [Registre uma conta no site do Horde](https://aihorde.net/register) e adicione sua chave Horde na caixa Horde API Key do SillyTavern.
- [Configure um Horde Worker](https://github.com/Haidra-Org/AI-Horde-Worker#readme) para fornecer sua GPU para outros.
  - Permitir que outros usem sua GPU ganha ['Kudos', uma espécie de moeda exclusiva do Horde](https://github.com/Haidra-Org/AI-Horde/blob/main/FAQ.md#kudos).
  - Quanto mais kudos sua conta tiver, mais rápido você receberá respostas de chat de outros Horde Workers.
  - Kudos também podem ser usados para criar imagens de IA no [Stable Horde](https://stablehorde.net).
    - O SillyTavern suporta geração de imagens Stable Horde pronta para uso.
- Se sua GPU não é poderosa o suficiente para executar uma IA, ou você não tem um computador, você ainda pode [participar da comunidade Horde para ganhar Kudos de várias maneiras](https://github.com/Haidra-Org/AI-Horde/blob/main/FAQ.md#i-dont-have-a-powerful-gpu-how-can-i-get-kudos).
