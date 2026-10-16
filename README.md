Guia Oficial do GAAP: Dominando o Claude Code

Bem-vindo(a) ao guia de configuração e boas práticas do Claude Code do GAAP!

O Claude Code é a ferramenta oficial de linha de comando (CLI) agêntica da Anthropic. Diferente de um chat comum no navegador, ele vive diretamente no seu terminal, entende toda a estrutura do nosso repositório, edita múltiplos arquivos, executa testes, analisa erros no terminal e interage diretamente com o Git.

Este documento foi criado para padronizar a instalação no Windows, configurar nosso ambiente da melhor forma possível e garantir que todos do time extraiam o máximo de produtividade no dia a dia.

📋 Índice
Pré-requisitos e Instalação no Windows
Resolvendo Problemas Comuns de Instalação (PATH)
Como Fazer Login e Autenticar
Configuração Ideal (O Segredo da Produtividade)
Fluxo de Trabalho no Dia a Dia
Tabela de Comandos Essenciais (Cheat Sheet)
Regras de Ouro do Nosso Time
💻 1. Pré-requisitos e Instalação no Windows

Atualmente, o Claude Code possui um instalador nativo para Windows que não exige instalar o Node.js manualmente e já conta com atualização automática em segundo plano.

Passo 1: Garanta que o Git for Windows está instalado

Embora o Claude Code rode no PowerShell, ele utiliza o Git Bash por baixo dos panos para executar diversas ferramentas de busca e manipulação de arquivos com mais eficiência.

Se ainda não tiver o Git instalado, baixe em: git-scm.com/download/win
Passo 2: Instale o Claude Code via PowerShell (Recomendado)
Abra o Windows Terminal ou o PowerShell (não precisa abrir como Administrador).
Cole o comando abaixo e pressione Enter:
irm https://claude.ai/install.ps1 | iex

Alternativas de Instalação:
Via WinGet: winget install Anthropic.ClaudeCode (Nota: instalações via WinGet não atualizam sozinhas; exigem rodar winget upgrade Anthropic.ClaudeCode periodicamente).
Via CMD (Prompt de Comando): curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd
Via WSL2 (Ubuntu/Debian): Caso o nosso projeto rode dentro do subsistema Linux no Windows, instale diretamente dentro do terminal do WSL com:

curl -fsSL https://claude.ai/install.sh | bash

Passo 3: Verifique a instalação

Feche o terminal atual e abra uma nova janela (isso é essencial para o Windows atualizar as variáveis de ambiente). Em seguida, execute:

claude --version


Para fazer um check-up completo e garantir que tudo está funcionando perfeitamente no seu sistema, rode:

claude doctor

🛠️ 2. Resolvendo Problemas Comuns de Instalação (PATH)

Se ao digitar claude --version aparecer o erro "O termo 'claude' não é reconhecido como nome de cmdlet..." ou um aviso dizendo que C:\Users\SeuUsuario\.local\bin não está no seu PATH, siga este passo a passo rápido:

Pressione Win + R no teclado, digite sysdm.cpl e aperte Enter.
Vá na aba Avançado e clique no botão Variáveis de Ambiente... (no canto inferior).
Na seção superior (Variáveis de usuário para...), procure a variável chamada Path, selecione-a e clique em Editar....
Clique em Novo e cole exatamente o caminho abaixo:
%USERPROFILE%\.local\bin

Clique em OK em todas as janelas para salvar.
Reinicie o seu terminal (e o VS Code, se estiver aberto) e teste claude --version novamente!
🔐 3. Como Fazer Login e Autenticar

Nunca inicie o Claude Code solto na pasta raiz do seu usuário (C:\Users\Nome). Ele foi feito para trabalhar dentro da pasta do projeto.

Navegue pelo terminal até a pasta do repositório em que você vai trabalhar:
cd C:\caminho\para\nosso-projeto

Inicie o Claude Code digitando:
claude

Na primeira execução, ele solicitará a autenticação:
Selecione o método de login utilizado pelo nosso time (Conta Claude.ai Pro/Max/Team ou Anthropic Console API).
Uma aba se abrirá no seu navegador padrão. Faça o login com a conta autorizada da equipe e confirme o código exibido no terminal.
Pronto! Caso precise trocar de conta no futuro, basta digitar /login ou /logout dentro da sessão do Claude.
⚙️ 4. Configuração Ideal (O Segredo da Produtividade)

Para que o Claude Code não gaste tempo (e tokens!) tentando adivinhar como nosso projeto funciona toda vez que alguém abre o terminal, precisamos configurá-lo corretamente.

4.1. O arquivo CLAUDE.md (A Memória do Projeto)

O arquivo CLAUDE.md na raiz do projeto funciona como um "manual de instruções" que o Claude lê automaticamente no início de toda sessão.

Se o repositório ainda não tiver um CLAUDE.md:

Dentro do Claude Code, digite:

/init


Ele vai analisar nossos arquivos (package.json, requirements.txt, estrutura de pastas, etc.) e criar um CLAUDE.md inicial automaticamente!

Boas Práticas para o nosso CLAUDE.md:

Mantenha o arquivo enxuto (menos de 100 linhas). Coloque apenas informações que mudam as decisões do agente:

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

Preferências Pessoais (CLAUDE.local.md):

Se você quiser dar instruções que só valem para você (ex: "Me explique cada alteração de forma didática pois sou novo em React"), crie um arquivo chamado CLAUDE.local.md na raiz (certifique-se de que ele está no .gitignore).

4.2. Integração com o VS Code / Cursor

Você pode usar o Claude Code integrado diretamente ao terminal do VS Code para visualizar diffs (diferenças de código) lado a lado no editor!

Abra as extensões do VS Code (Ctrl+Shift+X) e instale a extensão oficial Claude Code (da Anthropic).
Dentro do terminal do VS Code com o claude aberto, digite:
/ide


para conectar a sessão do terminal ao seu editor.

Digite também:
/terminal-setup


Isso configura o atalho Shift + Enter para pular de linha no prompt sem enviar a mensagem acidentalmente!

4.3. Configurando Permissões (/permissions)

Por padrão, o Claude pede permissão antes de editar arquivos ou rodar comandos no terminal. Para ganhar agilidade sem abrir mão da segurança:

Quando ele pedir para rodar um comando seguro e recorrente (como npm test ou eslint), escolha a opção "Always allow..." (Sempre permitir para este projeto).
Isso salva a regra no arquivo .claude/settings.local.json, evitando que você precise confirmar comandos de teste dezenas de vezes por dia.
🚀 5. Fluxo de Trabalho no Dia a Dia
💡 Adendo: O Claude Code também é um assistente para pesquisas e trabalhos acadêmicos!

Apesar de ser conhecido principalmente por ajudar na programação, o Claude Code não serve apenas para escrever e corrigir código. Ele também pode funcionar como um assistente pessoal para pesquisas, estudos, relatórios e trabalhos que estejam armazenados em arquivos no seu computador.

Como ele consegue analisar arquivos e pastas acessíveis no ambiente em que está sendo executado, você pode utilizá-lo para ler documentos, cruzar informações, resumir materiais e organizar ideias sem precisar copiar e colar manualmente todo o conteúdo em um chat no navegador.

Importante: para utilizar esse recurso, você não precisa estar dentro de um repositório de código. Pode abrir o terminal diretamente na pasta que contém seus documentos e iniciar o Claude Code ali. Ele trabalhará com os arquivos aos quais tiver acesso, respeitando as permissões disponíveis e os formatos que conseguir ler.

📚 Exemplos de uso fora da programação
Resumir documentos: analisar PDFs, arquivos Markdown, textos e outros formatos compatíveis para produzir resumos dos principais pontos.
Pesquisar em vários arquivos: procurar informações específicas em diferentes documentos e reunir os resultados em uma única resposta.
Apoiar trabalhos acadêmicos: organizar materiais de estudo, estruturar tópicos de um trabalho, sugerir perguntas de pesquisa e ajudar a construir uma linha de raciocínio.
Analisar artigos científicos: comparar objetivos, metodologias, resultados e conclusões de diferentes artigos disponibilizados localmente.
Organizar anotações: transformar anotações desorganizadas em tópicos, roteiros de apresentação ou resumos estruturados.
Produzir relatórios: utilizar informações presentes em documentos para elaborar uma primeira versão de um relatório, mantendo a possibilidade de conferir os dados nas fontes originais.
Extrair informações de documentos: identificar datas, conceitos, requisitos, nomes de seções ou outros dados relevantes em um conjunto de arquivos.
Comparar versões: verificar diferenças entre documentos, identificar alterações e destacar possíveis inconsistências.
🖥️ Como utilizar o Claude Code com seus documentos

Imagine que você tem uma pasta chamada Trabalho_Faculdade, contendo artigos científicos em PDF, anotações e um documento com as instruções do professor.

Você pode seguir estes passos:

Abra o PowerShell ou o terminal do VS Code.
Navegue até a pasta que contém os documentos:
cd "C:\Users\SeuUsuario\Documents\Trabalho_Faculdade"

Inicie o Claude Code:
claude

Explique o que você precisa fazer, indicando quais arquivos devem ser analisados.

Por exemplo:

"Analise os documentos desta pasta e identifique os principais conceitos relacionados ao tema do meu trabalho. Organize os resultados por assunto e indique de qual arquivo veio cada informação. Não altere os documentos originais."

A partir daí, o Claude poderá examinar os arquivos que conseguir acessar e ajudar a organizar as informações.

✍️ Prompts prontos para pesquisas e trabalhos

Você pode adaptar os exemplos abaixo para suas próprias necessidades:

1. Resumir vários artigos

"Leia os artigos disponíveis nesta pasta e produza um resumo de cada um, destacando objetivo, metodologia, principais resultados e conclusão. Identifique o nome do arquivo correspondente a cada resumo e não invente informações que não estejam nos textos."

2. Comparar fontes de pesquisa

"Analise os documentos desta pasta e compare as ideias apresentadas pelos autores. Identifique os pontos em comum, as divergências e as questões que ainda precisam ser investigadas. Sempre indique de qual documento veio cada informação."

3. Organizar um trabalho acadêmico

"Leia o arquivo com as instruções do professor e os materiais de referência disponíveis nesta pasta. Sugira uma estrutura para o trabalho, com introdução, desenvolvimento e conclusão. Explique o objetivo de cada seção e indique quais fontes podem ajudar em cada parte."

4. Preparar uma apresentação

"Com base nos documentos desta pasta, monte um roteiro de apresentação de aproximadamente 10 minutos. Organize os assuntos em uma sequência lógica, sugira os tópicos de cada slide e destaque os conceitos que precisam ser explicados."

5. Encontrar informações específicas

"Procure nos documentos desta pasta todas as informações relacionadas a [tema]. Apresente os resultados em tópicos, indicando o arquivo de origem e, quando possível, a página ou seção em que cada informação aparece."

6. Revisar um texto

"Revise o documento meu_trabalho.md, verificando clareza, organização, gramática e coerência. Preserve minhas ideias e meu estilo de escrita. Primeiro, apresente as sugestões de melhoria sem modificar o arquivo original."
⚠️ Cuidados importantes ao trabalhar com documentos
Confira as fontes: sempre verifique se os resumos, citações e interpretações correspondem ao conteúdo dos documentos originais.
Não invente referências: peça que o Claude indique as fontes utilizadas e confira os dados bibliográficos antes de incluir referências em um trabalho.
Proteja informações sensíveis: não disponibilize documentos com senhas, dados pessoais, informações confidenciais ou conteúdo restrito sem autorização.
Tenha cuidado com alterações: se estiver trabalhando com documentos importantes, peça para analisar primeiro e sugerir mudanças antes de editar os arquivos.
Verifique a leitura dos arquivos: PDFs digitalizados como imagens, documentos protegidos e formatos não suportados podem exigir OCR, conversão ou ferramentas adicionais. Não presuma que todo o conteúdo foi lido integralmente.
Utilize a ferramenta como apoio: em trabalhos acadêmicos, mantenha sua autoria, confira os resultados e siga as regras da instituição sobre o uso de inteligência artificial.

Dica: assim como na programação, quanto mais contexto você fornecer, melhor será o resultado. Explique o objetivo do trabalho, quais documentos devem ser considerados, como você deseja receber a resposta e quais limitações devem ser respeitadas.

Aqui está o método mais eficiente utilizado por times profissionais para trabalhar com o Claude Code em tarefas de desenvolvimento:

Passo 1: Use o "Plan Mode" antes de tarefas grandes (Shift + Tab)

Vai criar uma feature nova ou refatorar algo complexo? Não peça para ele sair codando direto.

Pressione Shift + Tab (ou digite /plan) para ativar o Modo de Planejamento (você verá o indicador de Plan Mode na barra inferior).
Nesse modo, o Claude fica em modo somente leitura. Ele vai ler os arquivos, analisar o impacto e montar um plano passo a passo.
Revise o plano com ele (ex: "Gostei, mas em vez de criar uma tabela nova, use a tabela de usuários existente").
Quando o plano estiver perfeito, pressione Shift + Tab novamente para sair do modo de planejamento e deixe ele executar o código!
Passo 2: Referencie arquivos usando @

Em vez de dizer "Arrume o botão na tela de login", digite @ e comece a escrever o nome do arquivo (o autocomplete vai abrir):

"No arquivo @src/pages/Login.tsx, ajuste a validação do formulário seguindo o padrão que já usamos em @src/pages/Cadastro.tsx."

Isso economiza muita janela de contexto e evita que ele procure no lugar errado.

Passo 3: Revisão e Controle de Erros (/diff e /rewind)
Terminou uma alteração? Digite /diff para abrir o visualizador interativo de mudanças no próprio terminal e ver tudo o que ele tocou.
O Claude seguiu um caminho ruim e quebrou o código? Não entre em pânico! Pressione Esc duas vezes seguidas (ou digite /rewind). Você pode escolher voltar no tempo apenas no código ("Rewind code only") ou voltar tanto o código quanto a conversa para um ponto anterior!
Passo 4: Higiene de Contexto (/clear e /compact)

O erro número 1 de quem começa a usar o Claude Code é fazer 5 tarefas diferentes na mesma conversa. Isso deixa a IA confusa, lenta e gasta a franquia de uso da equipe rapidamente.

Terminou uma task/issue e vai para outra? Digite /clear imediatamente para começar do zero.
Está no meio de uma tarefa grande e a sessão ficou longa? Digite /compact para o Claude resumir tudo o que foi feito até agora e liberar memória mantendo o foco na tarefa atual.
Passo 5: Automatizando o Git e Pull Requests

O Claude Code opera o Git com maestria. Exemplos de prompts para o fim da sua tarefa:

"Analise minhas alterações, rode os testes para garantir que nada quebrou e crie um commit seguindo o padrão do repositório."
"Estou com conflito de merge na branch atual. Analise os conflitos, explique o que mudou de cada lado e resolva para mim."
⚡ 6. Tabela de Comandos Essenciais (Cheat Sheet)

Digite / no prompt do Claude Code para ver todos os comandos. Abaixo estão os que você mais vai usar na rotina:

Comando / Atalho	O que faz	Quando usar
/init	Cria o arquivo CLAUDE.md analisando o repositório	Na primeira vez que configuramos um projeto novo
Shift + Tab ou /plan	Alterna para o Modo de Planejamento (somente leitura)	Antes de iniciar qualquer feature ou refatoração de múltiplos arquivos
/clear	Limpa todo o histórico da conversa atual	Sempre que você finalizar uma tarefa e for começar outra
/compact [foco]	Resume a conversa atual para liberar espaço de contexto	Quando uma tarefa longa passar de 70% de uso do contexto
/context	Mostra um gráfico visual de quanto contexto está sendo gasto	Para checar se o contexto está cheio
/diff	Abre um visualizador interativo com todas as mudanças feitas	Antes de fazer qualquer commit para revisar o código gerado
/rewind (ou Esc Esc)	Volta o código e/ou a conversa para um ponto anterior (Checkpoint)	Quando uma abordagem der errado e você quiser desfazer rápido
/resume	Abre uma lista das suas conversas anteriores para continuar	Quando você fechou o terminal ontem e quer retomar hoje
/btw [pergunta]	Faz uma pergunta rápida paralela sem poluir o histórico principal	Para tirar uma dúvida rápida de sintaxe no meio de uma tarefa
/model	Alterna o modelo ativo (ex: Sonnet para o dia a dia, Opus para problemas complexos)	Para equilibrar velocidade/custo e capacidade de raciocínio
/usage ou /cost	Mostra o consumo atual do plano ou gastos da API	Para acompanhar os limites da conta da equipe
/doctor	Verifica a saúde da instalação, variáveis e configurações	Sempre que algo parecer travado ou com erro de sistema
Ctrl + C ou Esc	Interrompe o Claude imediatamente no meio de uma ação	Se você perceber que ele entendeu errado e está indo pelo caminho errado
🤝 7. Regras de Ouro do Nosso Time

Para mantermos a qualidade do nosso código e o bom uso da ferramenta na equipe de estágio, siga sempre estes combinados:

Você ainda é o responsável pelo seu Pull Request: "O Claude que fez assim" não é justificativa em Code Review! Sempre leia o /diff, entenda cada linha gerada e remova códigos mortos ou comentários desnecessários antes de abrir um PR.
Peça para ele verificar o próprio trabalho: Sempre inclua a forma de validação no seu pedido. Em vez de "Crie a função X", peça "Crie a função X e rode os testes unitários até que todos passem".
Atenção aos Segredos (.env): Nunca cole chaves de API de produção, senhas de banco de dados ou dados sensíveis de clientes diretamente no chat.
Evolua o nosso CLAUDE.md: Se você notar que o Claude cometeu um erro repetitivo no nosso projeto (ex: importou uma biblioteca antiga em vez da nova), adicione uma linha explicando isso no CLAUDE.md e suba no seu PR para ajudar todo o resto da equipe!

Dúvidas ou descobriu um comando novo muito útil? Compartilhe no canal do nosso time e atualize este README! 🚀
