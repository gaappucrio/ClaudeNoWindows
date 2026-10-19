# Guia Oficial do GAAP: Dominando o Claude Code 🚀

Bem-vindo(a) ao guia de configuração e boas práticas do **Claude Code do GAAP**!

O **Claude Code** é a ferramenta oficial de linha de comando (CLI) agêntica da Anthropic. Diferente de um chat comum no navegador, ele vive diretamente no seu terminal, entende toda a estrutura do nosso repositório, edita múltiplos arquivos, executa testes, analisa erros no terminal e interage diretamente com o Git.

Este documento foi criado para padronizar a instalação no Windows, configurar nosso ambiente da melhor forma possível e garantir que todos do time extraiam o máximo de produtividade no dia a dia — seja para programação quanto para pesquisas e trabalhos acadêmicos.

## 📋 Índice

1. [Pré-requisitos e Instalação no Windows](#-1-pré-requisitos-e-instalação-no-windows)
2. [Resolvendo Problemas Comuns de Instalação (PATH)](#️-2-resolvendo-problemas-comuns-de-instalação-path)
3. [Como Fazer Login e Autenticar](#-3-como-fazer-login-e-autenticar)
4. [Configuração Ideal (O Segredo da Produtividade)](#%EF%B8%8F-4-configuração-ideal-o-segredo-da-produtividade)
5. [Fluxo de Trabalho no Dia a Dia](#-5-fluxo-de-trabalho-no-dia-a-dia)
   * [Fluxo para Desenvolvimento](#51-fluxo-de-trabalho-para-desenvolvimento)
   * [Pesquisas e Trabalhos Acadêmicos](#52-adendo-o-claude-code-para-pesquisas-e-trabalhos-acadêmicos)
6. [Tabela de Comandos Essenciais (Cheat Sheet)](#-6-tabela-de-comandos-essenciais-cheat-sheet)
7. [Regras de Ouro do Nosso Time](#-7-regras-de-ouro-do-nosso-time)

---

## 💻 1. Pré-requisitos e Instalação no Windows

Atualmente, o Claude Code possui um instalador nativo para Windows que não exige instalar o Node.js manualmente e já conta com atualização automática em segundo plano.

### Passo 1: Garanta que o Git for Windows está instalado

Embora o Claude Code rode no PowerShell, ele utiliza o **Git Bash** por baixo dos panos para executar diversas ferramentas de busca e manipulação de arquivos com mais eficiência.

* Se ainda não tiver o Git instalado, baixe em: [git-scm.com/download/win](https://git-scm.com/download/win)

### Passo 2: Instale o Claude Code via PowerShell (Recomendado)

1. Abra o **Windows Terminal** ou o **PowerShell** (não precisa abrir como Administrador).
2. Cole o comando abaixo e pressione `Enter`:

```powershell
irm https://claude.ai/install.ps1 | iex
```

#### Alternativas de Instalação:

* **Via WinGet:** `winget install Anthropic.ClaudeCode` *(Nota: instalações via WinGet não atualizam sozinhas; exigem rodar `winget upgrade Anthropic.ClaudeCode` periodicamente).*
* **Via CMD (Prompt de Comando):**
  ```cmd
  curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd
  ```
* **Via WSL2 (Ubuntu/Debian):** Caso o nosso projeto rode dentro do subsistema Linux no Windows, instale diretamente no terminal do WSL com:
  ```bash
  curl -fsSL https://claude.ai/install.sh | bash
  ```

### Passo 3: Verifique a instalação

1. Feche o terminal atual e abra uma **nova janela** (essencial para o Windows atualizar as variáveis de ambiente).
2. Verifique a versão executando:
   ```bash
   claude --version
   ```
3. Para fazer um check-up completo e garantir que tudo está funcionando perfeitamente no sistema, rode:
   ```bash
   claude doctor
   ```

---

## 🛠️ 2. Resolvendo Problemas Comuns de Instalação (PATH)

Se ao digitar `claude --version` aparecer o erro *"O termo 'claude' não é reconhecido como nome de cmdlet..."* ou um aviso de que `C:\Users\SeuUsuario\.local\bin` não está no seu PATH, siga este passo a passo rápido:

1. Pressione `Win + R` no teclado, digite `sysdm.cpl` e aperte `Enter`.
2. Vá na aba **Avançado** e clique no botão **Variáveis de Ambiente...** (no canto inferior).
3. Na seção superior (*Variáveis de usuário para...*), procure a variável chamada **Path**, selecione-a e clique em **Editar...**.
4. Clique em **Novo** e cole exatamente o caminho abaixo:
   ```text
   %USERPROFILE%\.local\bin
   ```
5. Clique em **OK** em todas as janelas para salvar.
6. Reinicie o seu terminal (e o VS Code, se estiver aberto) e teste `claude --version` novamente!

---

## 🔐 3. Como Fazer Login e Autenticar

> ⚠️ **Atenção:** Nunca inicie o Claude Code solto na pasta raiz do seu usuário (`C:\Users\Nome`). Ele foi feito para trabalhar dentro da pasta do projeto!

1. Navegue pelo terminal até a pasta do repositório em que você vai trabalhar:
   ```bash
   cd C:\caminho\para\nosso-projeto
   ```
2. Inicie o Claude Code digitando:
   ```bash
   claude
   ```
3. Na primeira execução, ele solicitará a autenticação:
   * Selecione o método de login utilizado pelo nosso time (Conta *Claude.ai Pro/Max/Team* ou *Anthropic Console API*).
   * Uma aba se abrirá no seu navegador padrão. Faça o login com a conta autorizada da equipe e confirme o código exibido no terminal.
4. **Pronto!** Caso precise trocar de conta no futuro, basta digitar `/login` ou `/logout` dentro da sessão do Claude.

---

## ⚙️ 4. Configuração Ideal (O Segredo da Produtividade)

Para que o Claude Code não gaste tempo (e tokens!) tentando adivinhar como nosso projeto funciona toda vez que alguém abre o terminal, precisamos configurá-lo corretamente.

### 4.1. O arquivo `CLAUDE.md` (A Memória do Projeto)

O arquivo `CLAUDE.md` na raiz do projeto funciona como um "manual de instruções" que o Claude lê automaticamente no início de toda sessão.

* **Se o repositório ainda não tiver um `CLAUDE.md`:**
  Dentro do Claude Code, digite:
  ```text
  /init
  ```
  Ele vai analisar nossos arquivos (`package.json`, `requirements.txt`, estrutura de pastas, etc.) e criar um `CLAUDE.md` inicial automaticamente!

* **Boas Práticas para o nosso `CLAUDE.md`:**
  Mantenha o arquivo enxuto (menos de 100 linhas). Coloque apenas informações que mudam as decisões do agente:

```markdown
# Guia do Projeto para o Claude Code

## Comandos Principais
- Instalar dependências: `npm install`
- Rodar servidor de desenvolvimento: `npm run dev`
- Rodar testes unitários: `npm run test`
- Verificar tipagem e lint: `npm run lint && npm run typecheck`

## Padrões de Código e Arquitetura
- Utilize TypeScript estrito (evite `any`).
- Componentes visuais ficam em `src/components/ui`.
- Sempre crie ou atualize testes ao modificar regras de negócio em `src/services`.
- Use a abordagem mais simples possível; não crie abstrações desnecessárias.

## Fluxo de Git
- Mensagens de commit devem seguir o padrão Conventional Commits em português (ex: `feat: adiciona validação de login`).
- Nunca faça push direto para a branch `main` ou `develop`.
```

* **Preferências Pessoais (`CLAUDE.local.md`):**
  Se você quiser dar instruções que só valem para você (ex: *"Me explique cada alteração de forma didática pois sou novo em React"*), crie um arquivo chamado `CLAUDE.local.md` na raiz e certifique-se de adicioná-lo ao seu `.gitignore`.

### 4.2. Integração com o VS Code / Cursor

Você pode usar o Claude Code integrado diretamente ao terminal do VS Code para visualizar diffs (diferenças de código) lado a lado no editor!

1. Abra as extensões do VS Code (`Ctrl + Shift + X`) e instale a extensão oficial **Claude Code** (da Anthropic).
2. Dentro do terminal do VS Code com o claude aberto, digite `/ide` para conectar a sessão do terminal ao seu editor.
3. Digite também `/terminal-setup` para configurar o atalho `Shift + Enter` para pular de linha no prompt sem enviar a mensagem acidentalmente!

### 4.3. Configurando Permissões (`/permissions`)

Por padrão, o Claude pede permissão antes de editar arquivos ou rodar comandos no terminal. Para ganhar agilidade sem abrir mão da segurança:

* Quando ele pedir para rodar um comando seguro e recorrente (como `npm test` ou `eslint`), escolha a opção **"Always allow..."** (*Sempre permitir para este projeto*).
* Isso salva a regra no arquivo `.claude/settings.local.json`, evitando que você precise confirmar comandos de teste dezenas de vezes por dia.

---

## 🚀 5. Fluxo de Trabalho no Dia a Dia

### 5.1. Fluxo de Trabalho para Desenvolvimento

Aqui está o método mais eficiente utilizado por times profissionais para trabalhar com o Claude Code em tarefas de desenvolvimento:

* **Passo 1: Use o "Plan Mode" antes de tarefas grandes (`Shift + Tab`)**
  Vai criar uma feature nova ou refatorar algo complexo? Não peça para ele sair codando direto.
  1. Pressione `Shift + Tab` (ou digite `/plan`) para ativar o Modo de Planejamento (indicador na barra inferior). Nesse modo, ele fica em somente leitura para analisar o impacto e montar um plano passo a passo.
  2. Revise o plano com ele (ex: *"Gostei, mas em vez de criar uma tabela nova, use a tabela de usuários existente"*).
  3. Quando perfeito, pressione `Shift + Tab` novamente para sair do modo de planejamento e deixe-o executar o código!

* **Passo 2: Referencie arquivos usando `@`**
  Em vez de dizer *"Arrume o botão na tela de login"*, digite `@` e comece a escrever o nome do arquivo para usar o autocomplete:
  > *"No arquivo `@src/pages/Login.tsx`, ajuste a validação do formulário seguindo o padrão que já usamos em `@src/pages/Cadastro.tsx`."*

* **Passo 3: Revisão e Controle de Erros (`/diff` e `/rewind`)**
  * Terminou uma alteração? Digite `/diff` para abrir o visualizador interativo de mudanças no próprio terminal.
  * O Claude seguiu um caminho ruim e quebrou o código? Pressione `Esc` duas vezes seguidas (ou digite `/rewind`). Você pode voltar no tempo apenas no código (*"Rewind code only"*) ou tanto no código quanto na conversa!

* **Passo 4: Higiene de Contexto (`/clear` e `/compact`)**
  * O erro número 1 é fazer 5 tarefas diferentes na mesma conversa. Terminou uma task e vai para outra? Digite `/clear` imediatamente.
  * Está no meio de uma tarefa longa e a sessão ficou pesada? Digite `/compact` para o Claude resumir tudo o que foi feito e liberar espaço de contexto.

* **Passo 5: Automatizando o Git e Pull Requests**
  O Claude Code opera o Git com maestria. Exemplos de prompts:
  * *"Analise minhas alterações, rode os testes para garantir que nada quebrou e crie um commit seguindo o padrão do repositório."*
  * *"Estou com conflito de merge na branch atual. Analise os conflitos, explique o que mudou de cada lado e resolva para mim."*

---

### 5.2. Adendo: O Claude Code para Pesquisas e Trabalhos Acadêmicos

💡 **Sabia que ele vai além da programação?**

O Claude Code também funciona como um assistente pessoal para pesquisas, estudos, relatórios e trabalhos armazenados em arquivos no seu computador. Como ele lê pastas locais, você pode utilizá-lo para ler documentos, cruzar informações, resumir materiais e organizar ideias sem copiar e colar manualmente em um chat no navegador.

* **Exemplos de uso fora da programação:**
  * **Resumir documentos:** Analisar PDFs, arquivos Markdown e textos para extrair principais pontos.
  * **Pesquisar em vários arquivos:** Procurar termos específicos em múltiplos documentos e reunir numa única resposta.
  * **Apoiar trabalhos acadêmicos:** Organizar materiais de estudo, estruturar tópicos e sugerir perguntas de pesquisa.
  * **Analisar artigos científicos:** Comparar objetivos, metodologias, resultados e conclusões localmente.
  * **Organizar anotações:** Transformar apontamentos soltos em tópicos ou roteiros de apresentação.
  * **Produzir relatórios e comparar versões:** Elaborar rascunhos com base em fontes originais e verificar divergências entre textos.

#### 🖥️ Como utilizar com seus documentos:

Imagine uma pasta chamada `Trabalho_Faculdade` com artigos em PDF e anotações:

1. Abra o PowerShell ou o terminal do VS Code.
2. Navegue até a pasta:
   ```bash
   cd "C:\Users\SeuUsuario\Documents\Trabalho_Faculdade"
   ```
3. Inicie o Claude Code:
   ```bash
   claude
   ```
4. Indique o que precisa:
   > *"Analise os documentos desta pasta e identifique os principais conceitos relacionados ao tema do meu trabalho. Organize os resultados por assunto e indique de qual arquivo veio cada informação. Não altere os documentos originais."*

#### ✍️ Prompts Prontos para Pesquisas e Trabalhos:

1. **Resumir vários artigos:**
   > *"Leia os artigos disponíveis nesta pasta e produza um resumo de cada um, destacando objetivo, metodologia, principais resultados e conclusão. Identifique o nome do arquivo correspondente a cada resumo."*

2. **Comparar fontes de pesquisa:**
   > *"Analise os documentos desta pasta e compare as ideias apresentadas pelos autores. Identifique pontos em comum, divergências e lacunas. Sempre indique a fonte."*

3. **Organizar um trabalho acadêmico:**
   > *"Leia o arquivo com as instruções do professor e os materiais de referência. Sugira uma estrutura de introdução, desenvolvimento e conclusão, indicando quais fontes ajudam em cada parte."*

4. **Preparar uma apresentação:**
   > *"Com base nos documentos desta pasta, monte um roteiro de apresentação de aproximadamente 10 minutos com tópicos por slide e conceitos-chave."*

5. **Encontrar informações específicas:**
   > *"Procure nos documentos desta pasta todas as informações relacionadas a [tema] em tópicos, indicando o arquivo e a seção de origem."*

6. **Revisar um texto:**
   > *"Revise o documento meu_trabalho.md verificando clareza, gramática e coerência, preservando meu estilo de escrita. Primeiro apresente sugestões sem modificar o arquivo original."*

> ⚠️ **Cuidados importantes com documentos:**
> * **Confira as fontes:** Valide se as citações e resumos batem com os originais.
> * **Proteja informações sensíveis:** Nunca exponha senhas, dados confidenciais ou documentos restritos.
> * **Verifique a leitura dos arquivos:** PDFs digitalizados como imagem ou protegidos podem exigir OCR ou conversão prévia.

---

## ⚡ 6. Tabela de Comandos Essenciais (Cheat Sheet)

Digite `/` no prompt do Claude Code para ver todos os comandos disponíveis. Os mais utilizados na rotina incluem:

| Comando / Atalho | O que faz | Quando usar |
| :--- | :--- | :--- |
| `/init` | Cria o arquivo `CLAUDE.md` analisando o repositório | Na primeira vez configurando um projeto novo |
| `Shift + Tab` ou `/plan` | Alterna para o Modo de Planejamento (somente leitura) | Antes de iniciar qualquer feature ou refatoração complexa |
| `/clear` | Limpa todo o histórico da conversa atual | Sempre que finalizar uma tarefa e for começar outra |
| `/compact [foco]` | Resume a conversa atual para liberar espaço de contexto | Quando uma tarefa longa passar de 70% de uso do contexto |
| `/context` | Mostra um gráfico visual de quanto contexto está sendo gasto | Para checar se o contexto está cheio |
| `/diff` | Abre um visualizador interativo com todas as mudanças feitas | Antes de fazer commit para revisar o código gerado |
| `/rewind` (ou `Esc Esc`) | Volta o código/conversa para um ponto anterior (Checkpoint) | Quando uma abordagem der errado e você quiser desfazer rápido |
| `/resume` | Abre uma lista de conversas anteriores para continuar | Quando você fechou o terminal ontem e quer retomar hoje |
| `/btw [pergunta]` | Faz uma pergunta rápida paralela sem poluir o histórico principal | Para tirar dúvida rápida de sintaxe no meio de uma tarefa |
| `/model` | Alterna o modelo ativo (Sonnet para dia a dia, Opus para complexos) | Para equilibrar velocidade/custo e capacidade de raciocínio |
| `/usage` ou `/cost` | Mostra consumo atual do plano ou gastos da API | Para acompanhar os limites da conta da equipe |
| `/doctor` | Verifica a saúde da instalação, variáveis e configurações | Sempre que algo parecer travado ou com erro de sistema |
| `Ctrl + C` ou `Esc` | Interrompe o Claude imediatamente no meio de uma ação | Se perceber que ele entendeu errado e está indo pelo caminho ruim |

---

## 🤝 7. Regras de Ouro do Nosso Time

Para mantermos a qualidade do nosso código e o bom uso da ferramenta na equipe, siga sempre estes combinados:

1. **Você ainda é o responsável pelo seu Pull Request:** *"O Claude que fez assim"* não é justificativa em Code Review! Sempre leia o `/diff`, entenda cada linha gerada e remova códigos mortos ou comentários desnecessários antes de abrir um PR.
2. **Peça para ele verificar o próprio trabalho:** Sempre inclua a forma de validação no pedido. Em vez de *"Crie a função X"*, peça *"Crie a função X e rode os testes unitários até que todos passem"*.
3. **Atenção aos Segredos (`.env`):** Nunca cole chaves de API de produção, senhas de banco de dados ou dados sensíveis de clientes diretamente no chat.
4. **Evolua o nosso `CLAUDE.md`:** Se notar que o Claude cometeu um erro repetitivo no projeto (ex: importou uma biblioteca antiga em vez da nova), adicione uma linha explicando isso no `CLAUDE.md` e suba no seu PR para ajudar todo o resto da equipe!

💡 *Dúvidas ou descobriu um comando novo muito útil? Compartilhe no canal do nosso time e atualize este README! 🚀*
