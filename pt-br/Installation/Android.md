---
label: Android (Termux)
route: /installation/android-(termux)/
---

# Instalação no Android (Termux)

SillyTavern pode ser executado nativamente em dispositivos Android usando Termux.

## Instalando o Termux

!!!tip
Evite instalar o Termux da Google Play Store, essa versão não é mais mantida.
Em vez disso, use o F-Droid (recomendado) ou lançamentos do GitHub para obter a versão mais recente.
!!!

1. Baixe o Termux do [F-Droid](https://f-droid.org/en/packages/com.termux/) ou [lançamentos do GitHub](https://github.com/termux/termux-app/releases).
2. Instale o arquivo APK baixado.
3. Abra o Termux e execute seu primeiro comando:

   ```bash
   termux-change-repo
   ```

4. Selecione "Mirror group" e escolha seus servidores mais próximos. Você pode tocar na tela ou usar gestos de deslizar com o [Unexpected Keyboard](https://play.google.com/store/apps/details?id=juloo.keyboard2&hl=en).
5. Atualize o Termux:

   ```bash
   pkg update && pkg upgrade
   ```

## Instalando Dependências

Instale os pacotes necessários:

```bash
pkg install git nodejs-lts nano
```

!!!warning
Se você estiver executando Android de 32 bits, consulte a seção [Erros Comuns](#common-errors) abaixo para etapas adicionais.
!!!

## Instalando o SillyTavern

Clone o repositório do SillyTavern ([Como Escolher uma Branch](/Installation/index.md#branches)):

- **Branch Release:**

    ```bash
    git clone https://github.com/SillyTavern/SillyTavern -b release
    ```

- **Branch Staging:**

    ```bash
    git clone https://github.com/SillyTavern/SillyTavern -b staging
    ```

## Executando o SillyTavern

Para executar o SillyTavern, navegue até o diretório clonado e execute o script de inicialização:

```bash
cd ~/SillyTavern
bash start.sh
```

Para atualizar o SillyTavern, navegue até o diretório SillyTavern e execute:

```bash
cd ~/SillyTavern
git pull --rebase --autostash
```

Veja a seção [Aliases](#optional-create-aliases) abaixo para criar atalhos para simplificar este processo.

## Erros Comuns

### Unsupported platform: android arm LEtime-web

Android de 32 bits requer uma dependência externa que não pode ser instalada com npm.

Use o seguinte comando para instalá-la:

```bash
pkg install esbuild
```

Então prossiga com as etapas de instalação acima.

### Ajustes de desempenho

!!!info
Para dicas gerais sobre como melhorar o desempenho, consulte a respectiva [seção de FAQ](/Usage/faq.md#performance-tips).
!!!

Devido às limitações de hardware em dispositivos Android, você pode querer ajustar as seguintes configurações do [config.yaml](/Administration/config-yaml.md) do SillyTavern para melhor uso de memória, armazenamento e CPU:

```yaml
performance:
  # Evita carregar todos os dados de personagem até que sejam necessários
  lazyLoadCharacters: true
  # Desabilita cache em disco para reduzir uso de armazenamento
  useDiskCache: false
backups:
  chat:
    # Opcional: Desabilita backups automáticos de chat para economizar armazenamento
    enabled: false
```

!!!tip
Use o editor de texto `nano` incluído com o Termux para editar o arquivo `config.yaml`: `nano ~/SillyTavern/config.yaml`
!!!

## Opcional: Criar Aliases

Você pode criar atalhos para comandos comuns para facilitar seu fluxo de trabalho.

1. Abra um editor para modificar seu arquivo `.bashrc`:

   ```bash
   nano ~/.bashrc
   ```

2. Adicione as seguintes linhas para criar aliases:

   ```bash
   # Atualizar pacotes do Termux
   alias pkgup="pkg update && pkg upgrade"
   # Iniciar SillyTavern
   alias st='cd ~/SillyTavern && bash start.sh'
   # Atualizar SillyTavern
   alias stup='cd ~/SillyTavern && git pull --rebase --autostash'
   ```

3. Salve o arquivo e saia do editor (no nano, pressione `CTRL + X`, depois `Y`, depois `Enter`).

4. Para aplicar as mudanças, execute:

   ```bash
   source ~/.bashrc
   ```

Agora você pode usar os seguintes comandos:

- `st` para iniciar o SillyTavern
- `stup` para atualizar o SillyTavern
- `pkgup` para atualizar pacotes do Termux

## Leitura Adicional

!!!info
Os guias vinculados abaixo não são mantidos pela equipe do SillyTavern.
!!!

- Guia do SillyTavern no Termux por ArroganceComplex#2659: <https://rentry.org/STAI-Termux>
- Acessando arquivos do Termux com Material Files: <https://www.learntermux.tech/2020/10/Termux-File-Manager.html>
- Prevenir suspensão profunda do processo Termux: <https://wiki.termux.com/wiki/Termux-wake-lock>
