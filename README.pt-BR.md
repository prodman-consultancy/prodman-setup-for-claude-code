# ProdMan Setup for Claude Code

Um roteiro executável que transforma uma instalação nova do Claude Code numa instalação completa.

Você entrega o arquivo ao Claude Code e ele audita a máquina, instala apenas o que falta, oferece um
menu de conexões e métodos de trabalho, escreve um arquivo de instruções global e registra o que fez em
memórias permanentes. Tudo o que ele instala é global, válido em qualquer projeto e em qualquer sessão
futura.

[Read in English](README.md)

## Como usar

Do zero, são cinco passos. Já tem o Claude Code aberto e com a conta conectada? Vá direto ao passo 5.

1. **Instale o VS Code.** Baixe em [code.visualstudio.com](https://code.visualstudio.com/download) e
   instale com as opções padrão.
2. **Ponha o Claude Code no VS Code.** No VS Code, abra a área de extensões (`Ctrl+Shift+X` no
   Windows, `Cmd+Shift+X` no macOS), procure **Claude Code** e instale a que é publicada pela
   **Anthropic**.
3. **Assine um plano pago do Claude.** Crie a sua conta em [claude.ai](https://claude.ai) e assine o
   Pro ou o Max. O Claude Code não roda no plano gratuito.
4. **Entre com a sua conta.** Clique no ícone do Claude Code, uma faísca, na barra do lado esquerdo do
   VS Code, e comece uma conversa nova. Na primeira vez aparece a tela de entrada: clique em
   **Sign in** e aprove no navegador com a conta do passo 3.
5. **Rode o setup.** Copie a mensagem abaixo, cole no Claude Code e aperte Enter:

   ```text
   Instale e configure o meu Claude Code seguindo o roteiro prodman-setup-for-claude-code.md deste repositório: https://github.com/prodman-consultancy/prodman-setup-for-claude-code
   ```

Prefere o arquivo? Baixe o [`prodman-setup-for-claude-code.md`](prodman-setup-for-claude-code.md),
arraste para a conversa e diga "roda esse setup".

Responda as perguntas e nada mais. O roteiro pede números, não frases. Nada é instalado sem
confirmação, e qualquer item pode ser adicionado depois.

Ele fala o idioma em que você escreve. O roteiro em si é escrito em inglês americano, e a conversa
acompanha você.

## O que ele faz

**Audita antes de tocar em qualquer coisa.** Cada componente tem três estados possíveis, e cada um
recebe tratamento diferente: ausente instala, presente e atualizado deixa como está, presente e
defasado propõe atualização no lugar. Ele nunca instala por cima de uma versão que funciona e nunca
reinstala para "consertar" algo.

**Instala a base só se estiver faltando.** Node.js, Git, Python, uv, Docker Desktop,
VS Code. Sempre da fonte oficial atual do fornecedor, nunca de instalador embalado. Quando algo
que você pediu depende de uma peça que a máquina não tem, ele instala essa peça e termina a
instalação, em vez de pular o item.

**Oferece 19 itens opcionais em um menu só**, em três grupos, respondidos numa única resposta:

| Grupo | O que cobre |
|-------|-------------|
| Conexões, 1 a 9 | Automação de navegador, Google Workspace, Docker, GitHub, Railway, Supabase, Cloudflare, Metabase, Higgsfield |
| Métodos de trabalho, 10 a 13 | Disciplina de código mínimo, prática de engenharia, segurança por padrão, escrita em português brasileiro |
| Ferramentas separadas, 14 a 19 | Transcrição de voz, limpeza de metadados, análise de segurança de skill, notas, apresentações, scraping web |

Cada item explica, em linguagem simples, o que faz por você, o que você precisa ter antes de funcionar
e se custa algo. Nada é instalado para um serviço em que você não tem conta.

**Faz a configuração difícil no seu lugar.** Tudo que exige uma chave de API ou uma permissão vem com o
link direto para a página exata que cria aquilo. Quando isso não basta, o Claude conduz o navegador
pelas telas administrativas sozinho, e você só autentica. No Google Workspace esse é o caminho padrão,
porque o percurso manual significa criar um projeto na nuvem, habilitar uma API por ferramenta e
configurar um cliente OAuth.

O que ele cria em serviço de fora, um projeto na nuvem, um cliente OAuth, um token de acesso, nasce
na sua conta e leva o seu nome, nunca o nome de quem entregou este arquivo a você.

**Escreve um arquivo de instruções global.** No fim ele cria ou atualiza o `~/.claude/CLAUDE.md`, dentro
de um bloco marcado, mesclando em vez de sobrescrever o que já estiver lá. As regras que ele escreve
dependem do que foi instalado de fato, então nenhuma regra aponta para uma ferramenta ausente.

**Registra o que fez.** A última etapa escreve memórias permanentes descrevendo a instalação, as
conexões e suas contas, as ferramentas e seus caminhos, e o que ficou pendente. Credencial nunca vai
para memória, apenas qual serviço e qual conta.

## Requisitos

Claude Code, autenticado, no Windows ou no macOS (passos 1 a 4 acima). O resto o roteiro instala
para você, se estiver faltando.

Rode na configuração mais forte que você tiver. No rodapé da caixa onde você digita, o Claude Code
mostra o modelo e o nível de esforço. Clique ali e escolha Opus e ultracode. O roteiro confere as duas
coisas antes de tocar na máquina e conduz a troca se estiverem abaixo disso, porque aqui ele instala
software e escreve configuração, não é conversa.

## Licença

[Apache License 2.0](LICENSE).
