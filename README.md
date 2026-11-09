# Guia Oficial do GAAP: Dominando o Claude Code 🚀

Bem-vindo(a) ao guia de configuração e boas práticas do **Claude Code do GAAP**!

O **Claude Code** é a ferramenta oficial de linha de comando (CLI) agêntica da Anthropic. Diferente de um chat comum no navegador, ele vive diretamente no seu terminal, entende toda a estrutura do nosso repositório, edita múltiplos arquivos, executa testes, analisa erros no terminal e interage diretamente com el Git.

Este documento foi criado para padronizar a instalação no Windows, configurar nosso ambiente da melhor forma possível e garantir que todos do time extraiam o máximo de produtividade no dia a dia — seja para programação quanto para pesquisas e trabalhos acadêmicos.

Caso tenha interesse em definir preferências e interação entre 2 agentes diferentes, como exemplo o Gemini Antigravity e o Claude Code ao mesmo tempo, recomendo seguir o repositório: https://github.com/Esppartano/agent-personal-preferences-template.git

## 📋 Índice

1. [Pré-requisitos e Instalação no Windows](#sec-1-instalacao-windows)
2. [Resolvendo Problemas Comuns de Instalação (PATH)](#sec-2-problemas-path)
3. [Como Fazer Login e Autenticar](#sec-3-login-autenticacao)
4. [Configuração Ideal (O Segredo da Produtividade)](#sec-4-configuracao)
5. [Fluxo de Trabalho no Dia a Dia](#sec-5-fluxo-trabalho)
   * [Fluxo para Desenvolvimento](#sec-5-1-desenvolvimento)
   * [Pesquisas e Trabalhos Acadêmicos](#sec-5-2-pesquisas-academicas)
6. [Tabela de Comandos Essenciais (Cheat Sheet)](#sec-6-comandos-essenciais)
7. [Regras de Ouro do Nosso Time](#sec-7-regras-time)
8. [Otimizando o Uso de Tokens! (Multi-Modelos, Advisor e Subagentes)](#sec-8-otimizando-tokens)

---

<a id="sec-1-instalacao-windows"></a>

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

<a id="sec-2-problemas-path"></a>

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

<a id="sec-3-login-autenticacao"></a>

## 🔐 3. Como Fazer Login e Autenticar

> ⚠️ **Atenção:** Nunca inicie o Claude Code solto na pasta raiz do seu usuário (`C:\Users\Nome`). Ele foi feito para trabalhar dentro da pasta do projeto!

1. Navegue pelo terminal até a pasta do repositório em que você vai trabalhar:
   ```powershell
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

<a id="sec-4-configuracao"></a>

## ⚙️ 4. Configuração Ideal (O Segredo da Produtividade)

Para que o Claude Code não gaste tempo (e tokens!) tentando adivinhar como nosso projeto funciona toda vez que alguém abre o terminal, precisamos configurá-lo corretamente.

### 4.1. O arquivo `CLAUDE.md` (A Memória do Projeto)

O arquivo `CLAUDE.md` na raiz do projeto funciona como um "manual de instruções" que o Claude lê automaticamente no início de toda sessão.

* **Se o repositório ainda não tiver um `CLAUDE.md`:**
  Dentro do Claude Code, digite:
  ```bash
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

<a id="sec-5-fluxo-trabalho"></a>

## 🚀 5. Fluxo de Trabalho no Dia a Dia

<a id="sec-5-1-desenvolvimento"></a>

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

<a id="sec-5-2-pesquisas-academicas"></a>

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
   ```powershell
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

<a id="sec-6-comandos-essenciais"></a>

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
| `/advisor [modelo]` | Define um modelo mais forte (ex: `opus`) como conselheiro da sessão | Para usar `sonnet` ou `haiku` na execução com supervisão do `opus` |
| `/effort [nível]` | Ajusta o esforço de raciocínio (`low`, `medium`, `high`, `max`) | Para economizar tokens em tarefas fáceis (`low`) ou focar nas difíceis |
| `/usage` ou `/cost` | Mostra consumo atual do plano ou gastos da API | Para acompanhar os limites da conta da equipe |
| `/doctor` | Verifica a saúde da instalação, variáveis e configurações | Sempre que algo parecer travado ou com erro de sistema |
| `Ctrl + C` ou `Esc` | Interrompe o Claude imediatamente no meio de uma ação | Se perceber que ele entendeu errado e está indo pelo caminho ruim |

---

<a id="sec-7-regras-time"></a>

## 🤝 7. Regras de Ouro do Nosso Time

Para mantermos a qualidade do nosso código e o bom uso da ferramenta na equipe, siga sempre estes combinados:

1. **Você ainda é o responsável pelo seu Pull Request:** *"O Claude que fez assim"* não é justificativa em Code Review! Sempre leia o `/diff`, entenda cada linha gerada e remova códigos mortos ou comentários desnecessários antes de abrir um PR.
2. **Peça para ele verificar o próprio trabalho:** Sempre inclua a forma de validação no pedido. Em vez de *"Crie a função X"*, peça *"Crie a função X e rode os testes unitários até que todos passem"*.
3. **Atenção aos Segredos (`.env`):** Nunca cole chaves de API de produção, senhas de banco de dados ou dados sensíveis de clientes diretamente no chat.
4. **Evolua o nosso `CLAUDE.md`:** Se notar que o Claude cometeu um erro repetitivo no projeto (ex: importou uma biblioteca antiga em vez da nova), adicione uma linha explicando isso no `CLAUDE.md` e suba no seu PR para ajudar todo o resto da equipe!

---

<a id="sec-8-otimizando-tokens"></a>

## 🧠 8. Otimizando o Uso de Tokens! (Multi-Modelos, Advisor e Subagentes)

Rodar o modelo mais pesado (`opus`) o tempo inteiro para ler arquivos grandes, renomear pastas ou fazer edições simples consome a franquia da equipe até **5x mais rápido** sem necessidade.

O segredo de ouro no Claude Code é **combinar modelos diferentes na mesma sessão**: colocar o **Opus como o "cérebro/conselheiro"** da operação, o **Sonnet como o "executor"** principal e o **Haiku nos "subagentes"** (que fazem o trabalho braçal de ler dezenas de arquivos e vasculhar pastas em segundo plano).

### 8.1. Entendendo a Arquitetura: Principal vs. Advisor vs. Subagentes

No Claude Code, até 3 camadas de modelos podem trabalhar juntas:

1. **Modelo Principal (`--model`):** É quem conduz a conversa, edita arquivos e roda comandos no terminal (`sonnet`, `haiku`, `opus` ou `opusplan`).
2. **Modelo Conselheiro (`--advisor`):** Ativado com `--advisor opus` (ou `/advisor opus`). Enquanto o modelo principal mais econômico trabalha, o **Opus** é acionado automaticamente apenas nos **3 momentos críticos**:
   * Antes de começar a escrever código (para validar o plano de ação);
   * Quando o agente principal trava num erro repetitivo;
   * Antes de finalizar a tarefa (para auditar se tudo foi cumprido).
3. **Subagentes (`CLAUDE_CODE_SUBAGENT_MODEL` ou `/agents`):** Quando o Claude precisa vasculhar o projeto, ler vários PDFs ou rodar testes, ele cria "subagentes" que rodam em janelas de contexto isoladas e devolvem apenas um resumo para a conversa principal.

> ⚠️ **Nota técnica importante sobre o comando no terminal:**
> A flag `--advisor opus` existe nativamente na CLI, mas **não existe uma flag chamada `--subagents haiku`** diretamente no comando `claude`. Para definir o modelo dos subagentes como `haiku` (ou qualquer outro), usamos a variável de ambiente `CLAUDE_CODE_SUBAGENT_MODEL` ou arquivos de configuração! Veja abaixo como rodar.

---

### 8.2. Como Rodar o "Combo de Otimização" (Opus Cérebro + Subagentes Haiku)

#### Opção 1: Pelo Terminal (PowerShell) para uma sessão

Copie e cole no PowerShell para iniciar o **Sonnet** como principal, **Opus** como conselheiro (Advisor) e **Haiku** para todos os subagentes:

```powershell
$env:CLAUDE_CODE_SUBAGENT_MODEL="haiku"; claude --model sonnet --advisor opus
```

Se a tarefa for bem simples e você quiser economizar ainda mais usando o próprio **Haiku** como executor principal sob supervisão do **Opus**:

```powershell
$env:CLAUDE_CODE_SUBAGENT_MODEL="haiku"; claude --model haiku --advisor opus
```

#### Opção 2: Deixar Salvo nas Configurações (Recomendado!)

Para não precisar digitar o comando grande toda vez, você pode salvar essa estrutura no seu arquivo `.claude/settings.local.json` (no projeto) ou `C:\Users\SeuUsuario\.claude\settings.json` (global):

```json
{
  "model": "sonnet",
  "advisorModel": "opus",
  "env": {
    "CLAUDE_CODE_SUBAGENT_MODEL": "haiku",
    "CLAUDE_CODE_SUBAGENT_MODEL_FORCE": "1"
  }
}
```
*(A variável `CLAUDE_CODE_SUBAGENT_MODEL_FORCE=1` garante que qualquer subagente use o modelo econômico definido por você).*

---

### 8.3. Posso escolher outros modelos para Subagentes diferentes?

**Sim!** Você pode usar `haiku`, `sonnet`, `opus` ou `inherit` (herdar o modelo principal). Melhor ainda: você pode ter **subagentes especializados com modelos diferentes** para cada função!

Dentro do Claude Code, digite `/agents` (ou crie arquivos na pasta `.claude/agents/`) para definir qual modelo cada ajudante vai usar:

* **Subagente Leitor/Explorador (`model: haiku`):** Ideal para ler artigos, buscar funções no código e resumir logs. Rápido e super barato.
* **Subagente Desenvolvedor/Testador (`model: sonnet`):** Ideal para escrever testes unitários ou refatorar módulos isolados.
* **Subagente Revisor de Segurança/Arquitetura (`model: opus`):** Ideal para fazer um *code review* rigoroso antes de abrir um Pull Request.

Você também pode criar um subagente sob demanda direto na linha de comando usando a flag `--agents`:

```powershell
claude --model sonnet --advisor opus --agents '{\"resumidor\":{\"description\":\"Lê e resume arquivos extensos\",\"prompt\":\"Extraia os pontos principais de forma enxuta\",\"model\":\"haiku\"}}'
```

---

### 8.4. Guia Prático: O Melhor Comando para Cada Atividade (Sem Perder Qualidade!)

Abaixo estão as combinações mais otimizadas para os **4 cenários mais comuns do nosso grupo**, equilibrando qualidade máxima e baixo consumo de tokens (incluindo a flag `--effort`, que controla o gasto de tokens de raciocínio interno entre `low`, `medium`, `high` e `max`):

#### 📄 Caso 1: Apenas Ler e Resumir Arquivos (PDFs, Artigos, Documentação, Logs)

* **Por que otimizar:** Ler dezenas de páginas no `opus` estoura o limite de tokens rapidamente. O segredo é colocar subagentes `haiku` para ler os documentos brutos em janelas isoladas e deixar o `sonnet` (ou o próprio `haiku`) consolidar o resumo final.
* **Comando para Resumos Rápidos do Dia a Dia (Máxima Economia):**
  ```powershell
  claude --model haiku --effort low
  ```
* **Comando para Pesquisas Acadêmicas e Fichamentos Densos (Alta Qualidade de Escrita):**
  ```powershell
  $env:CLAUDE_CODE_SUBAGENT_MODEL="haiku"; claude --model sonnet --effort medium
  ```
  > 💡 **Dica de Prompt:** Peça *"Use subagentes em paralelo para ler cada artigo desta pasta e me traga uma síntese comparativa"*. Assim, o texto bruto gigante dos artigos fica isolado no `haiku` e não polui a memória principal do `sonnet`!

#### 📁 Caso 2: Organizar Pastas, Renomear Arquivos e Padronizar Diretórios

* **Por que otimizar:** Tarefas mecânicas de sistema de arquivos (`mkdir`, `mv`, organizar por data/tema) não exigem raciocínio profundo. Usar `opus` ou `effort high` aqui desperdiça tokens de pensamento à toa.
* **Comando Mais Otimizado:**
  ```powershell
  claude --model haiku --effort low
  ```

#### 🛠️ Caso 3: Programar Scripts Básicos, Automações e Correções Simples

* **Por que otimizar:** Scripts em Python/Bash, criação de componentes simples, ajustes de CSS ou correção de bugs pontuais são a especialidade nativa do `sonnet` (ou até do `haiku` com um conselheiro).
* **Comando Padrão Ágil (Melhor Custo-Benefício):**
  ```powershell
  $env:CLAUDE_CODE_SUBAGENT_MODEL="haiku"; claude --model sonnet --effort medium
  ```
* **Comando Ultra Econômico com Rede de Segurança:**
  ```powershell
  claude --model haiku --advisor sonnet --effort low
  ```
  *(O Haiku programa o script super rápido; se tiver qualquer dúvida na lógica, o Sonnet entra como conselheiro).*

#### 🏗️ Caso 4: Programar Sistemas Complexos, Arquitetura e Refatorações Grandes

* **Por que otimizar:** Aqui não podemos abrir mão da inteligência do **Opus**, mas não queremos pagar preço de Opus enquanto o Claude apenas procura arquivos ou edita imports repetitivos. Temos **duas estratégias de elite**:

* **Estratégia A — O "Cérebro Opus" (Advisor + Sonnet + Subagentes Haiku):**
  O `sonnet` escreve o código pesado, o `haiku` vasculha o repositório nos subagentes e o `opus` supervisiona o plano inicial, destrava bugs difíceis e revisa o resultado final.
  ```powershell
  $env:CLAUDE_CODE_SUBAGENT_MODEL="haiku"; claude --model sonnet --advisor opus --effort high
  ```

* **Estratégia B — O Modo `opusplan` (Opus Planeja + Sonnet Executa):**
  O Claude Code tem um modo secreto chamado `opusplan`. Nele, enquanto você está no **Modo de Planejamento (`Shift + Tab`)**, você conversa diretamente com o **Opus** para desenhar toda a arquitetura. Assim que você aprova o plano e aperta `Shift + Tab` para codar, ele troca **automaticamente para o Sonnet** para implementar o código!
  ```powershell
  $env:CLAUDE_CODE_SUBAGENT_MODEL="haiku"; claude --model opusplan
  ```

---

### 📊 Tabela Resumo: Qual Comando Usar Hoje?

| Atividade | Modelo Principal | Advisor (Cérebro) | Subagentes | Comando Rápido (PowerShell) |
| :--- | :--- | :--- | :--- | :--- |
| **Organizar pastas / Renomear** | `haiku` | *Nenhum* | `haiku` | `claude --model haiku --effort low` |
| **Ler e resumir textos simples** | `haiku` | *Nenhum* | `haiku` | `claude --model haiku --effort low` |
| **Pesquisa acadêmica / Artigos** | `sonnet` | *Nenhum* | `haiku` | `$env:CLAUDE_CODE_SUBAGENT_MODEL="haiku"; claude --model sonnet --effort medium` |
| **Scripts básicos e bugs simples** | `sonnet` | *Nenhum* (ou `sonnet`) | `haiku` | `$env:CLAUDE_CODE_SUBAGENT_MODEL="haiku"; claude --model sonnet --effort medium` |
| **Sistemas complexos (Advisor)** | `sonnet` | `opus` | `haiku` | `$env:CLAUDE_CODE_SUBAGENT_MODEL="haiku"; claude --model sonnet --advisor opus` |
| **Arquitetura pesada (`opusplan`)** | `opusplan` | *Automático no Plan* | `haiku` | `$env:CLAUDE_CODE_SUBAGENT_MODEL="haiku"; claude --model opusplan` |

### 💡 3 Dicas Extras para Economizar Tokens Todos os Dias

1. **Crie um `.claudeignore` na raiz do projeto:** Funciona igual ao `.gitignore`. Coloque lá pastas com arquivos gigantes que o Claude não precisa ler (ex: `dados_brutos/`, `*.csv`, `*.log`, `dist/`, `pdfs_antigos/`).
2. **Troque de modelo no meio da conversa se precisar:** Começou uma sessão normal e percebeu que a tarefa ficou muito fácil ou muito difícil? Basta digitar `/model haiku`, `/model sonnet` ou `/model opusplan`, e `/effort low` ou `/effort high` sem precisar fechar o terminal!
3. **Use `/btw` para dúvidas rápidas:** Se estiver no meio de um código grande e quiser perguntar *"Qual é mesmo o comando git para criar branch?"*, use `/btw qual o comando...`. Ele responde sem gravar essa pergunta no histórico principal, economizando tokens em todas as mensagens seguintes!

💡 *Dúvidas ou descobriu um comando novo muito útil? Compartilhe no canal do nosso time e atualize este README! 🚀*
