---
order: 109
route: /installation/updating/migration-guide-1-09/
---

# Guia de Migração 1.9.0

## Como migrar para uma nova branch se eu uso main/dev?

_**É recomendado fazer uma instalação limpa.**_ No entanto, se você deseja usar uma cópia existente do SillyTavern, siga as instruções abaixo.

**IMPORTANTE!** Antes de fazer qualquer coisa, faça *um backup completo* da sua instalação. Você pode *perder seus dados* no processo, então não ignore este aviso.

Não tem certeza de quais arquivos fazer backup? Veja a lista aqui: [Como Atualizar o SillyTavern](/Installation/Updating/index.md#updating-from-1120-to-1120)

### Instalações git

1. Abra um prompt de terminal (cmd, PowerShell, Termux, etc) na pasta de instalação do seu SillyTavern.
2. Digite `git fetch` e depois `git pull` para baixar as atualizações.
3. Você pode perder suas configurações. Você fez um backup? `git switch release` ou `git switch staging` mudará sua branch, respectivamente
4. Pule para o próximo item se você não tiver erros. Você pode ter algo como:
   ```
   error: Your local changes to the following files would be overwritten by checkout:
        config.conf
        public/css/bg_load.css
        public/settings.json
   ```
   Você verá uma lista de arquivos afetados. Se você não se importa que esses arquivos de configuração sejam substituídos `git switch -f release` ou `git switch -f staging` definirá sua branch.
   Se você se importa em salvar essas mudanças, restaure do backup.

5. Digite `npm install` e depois `npm run start` para testar que tudo funciona corretamente.
6. Aproveite! Restaure seus dados de um backup se necessário.

### fatal: invalid reference: release

Isso pode acontecer se você clonou apenas uma única branch de um remote antigo (antes da migração para o repositório da organização). Para corrigir isso, você precisa adicionar e buscar uma branch de um novo remote:

```
git remote add st https://github.com/SillyTavern/SillyTavern
git fetch st
git checkout -t st/release
```

Então prossiga do passo 5.

### Instalações ZIP

Nada muda para você. Apenas baixe o ZIP da branch/release como de costume.
