# Finanças Inteligentes — App de Organização Financeira com Vibe Coding

Projeto desenvolvido como parte do desafio da DIO sobre criação de soluções com IA e Vibe Coding. A proposta é tornar o acompanhamento das finanças pessoais mais simples e conversacional, preservando uma experiência acessível em português brasileiro.

> **Status do projeto:** autenticação e persistência de parte dos dados financeiros já estão conectadas ao Lovable Cloud. A migração dos dados antigos do navegador e a integração de todas as funcionalidades financeiras continuam pendentes. Este README descreve o que foi construído até aqui, sem apresentar recursos planejados como concluídos.

## Sobre o desafio

O desafio propõe usar IA como parceira criativa para transformar uma ideia em um MVP, documentando o processo no GitHub. A entrega deve apresentar o prompt/PRD final, imagens ou vídeos das interações com o app, um resumo do produto e uma reflexão sobre o processo de Vibe Coding.

## Problema e público-alvo

Pessoas que querem organizar a vida financeira podem encontrar dificuldade em registrar gastos com rapidez e entender para onde o dinheiro está indo. O projeto explora uma alternativa mais direta: registrar despesas por meio de uma conversa e acompanhar compromissos e informações financeiras em telas simples.

O público pensado inclui pessoas iniciantes, intermediárias e avançadas em organização financeira pessoal, com foco em uma experiência compreensível para quem está começando.

## O que o app faz hoje

- Apresenta uma página inicial pública com opções para criar conta ou entrar.
- Oferece cadastro e login com e-mail e senha, além de opções de entrada com Google e Lovable.
- Solicita confirmação por e-mail no cadastro com e-mail e senha.
- Protege as áreas internas e redireciona pessoas não autenticadas para o login.
- Permite registrar despesas por conversa e salvar salário, contas fixas, pagamentos de contas e despesas na conta autenticada.
- Mantém dados financeiros salvos no banco vinculados à conta do usuário.
- Permite sair da conta pelas configurações.

### Limites atuais

- O histórico das conversas permanece salvo apenas neste navegador.
- Dados antigos que estavam no armazenamento local antes da conexão ao banco ainda não foram migrados automaticamente.
- A integração atual contempla salário, contas fixas, pagamentos e despesas; não equivale à implementação completa de todas as telas e recursos financeiros planejados.
- Cartões, faturas, geração de parcelas, relatórios completos e recomendações financeiras por IA não estão implementados nesta etapa.
- Uma verificação de ponta a ponta de desenvolvimento encontrou que o texto de teste `teste-e2e` foi interpretado como valor de R$ 2,00 por causa do número `2` na expressão. É um caso de borda conhecido na interpretação de valores por linguagem natural e deve ser tratado em evolução futura.

## Prompt final (PRD) do projeto

> Quero desenvolver o **Finanças Inteligentes**, um aplicativo web responsivo de finanças pessoais, em português brasileiro, para ajudar pessoas a organizar receitas, despesas e compromissos financeiros de forma simples e natural.
>
> O produto deve permitir que usuários criem uma conta, façam login e registrem gastos por meio de uma conversa em linguagem natural. Deve oferecer uma visão clara das finanças, incluindo receitas, despesas, saldo do período, contas fixas e pagamentos. A experiência deve ser acessível para iniciantes sem impedir a evolução para recursos mais avançados.
>
> Preserve a estrutura, a stack e os componentes existentes do repositório. Use o Lovable Cloud/Supabase como backend oficial, Supabase Auth para autenticação e PostgreSQL para persistência. Os dados privados devem ser protegidos por RLS e por validações no banco; cada usuário deve acessar somente os próprios registros e categorias globais permitidas. Não use o armazenamento local do navegador como banco principal e não exponha secrets ou credenciais administrativas no frontend.
>
> Use interface em português brasileiro e valores em reais (BRL), com formulários acessíveis, estados de carregamento, erro, sucesso e ausência de dados. Planeje as áreas de dashboard, movimentações, categorias, orçamentos, relatórios, assistente financeira e configurações. Recursos complexos de cartão e parcelamento devem ser definidos antes de serem implementados.
>
> Não declare como concluída uma funcionalidade que esteja apenas representada visualmente ou usando dados fictícios. Evolua o produto por etapas, valide o comportamento real e documente claramente o que está implementado e o que permanece pendente.

## Stack e arquitetura

- **Interface:** React 19, TypeScript, TanStack Start e TanStack Router.
- **Estilos e componentes:** Tailwind CSS v4, Radix UI e componentes shadcn/ui.
- **Dados e visualização:** TanStack Query e Recharts.
- **Backend:** Lovable Cloud, com PostgreSQL e autenticação Supabase.
- **Testes:** Vitest e Testing Library.

A fundação de dados inclui as tabelas `profiles`, `categories`, `accounts`, `budgets` e `transactions`, além da view `vw_account_balances`. O banco usa políticas RLS para restringir registros privados por usuário, regras de integridade para referências financeiras e categorias padrão de sistema. Valores monetários são armazenados com precisão decimal (`NUMERIC(12,2)`).

O saldo das contas é calculado a partir do saldo inicial e das movimentações concluídas; transferências afetam as contas de origem e destino sem serem tratadas como receita ou despesa. Movimentações pendentes não entram no saldo calculado. A estrutura de cartão foi preparada parcialmente, mas fluxos de fatura e compra parcelada não foram entregues.

## Capturas de tela e demonstração

![Página inicial](docs/screenshots/home.png)
![Login](docs/screenshots/login.png)
![Cadastro](docs/screenshots/cadastro.png)
![Conversa](docs/screenshots/conversa.png)
![Contas](docs/screenshots/contas.png)

## Validação realizada

- A fundação do banco foi aplicada no Lovable Cloud.
- Testes SQL com rollback validaram isolamento entre usuários, regras de integridade, categorias globais, transferências e cálculo de saldos. Os dados temporários dos testes foram desfeitos.
- O build da aplicação passou após as etapas de autenticação e persistência.
- Os testes de rotas e interpretação de gastos existentes passaram após a etapa de autenticação.
- Na verificação de persistência, foram criados dados temporários de teste, conferidos após recarregar a página e removidos em seguida.

**Ainda não validado:** migração automática dos dados locais preexistentes; fluxo completo de login e gravação de todos os tipos de movimentação contra o backend; recursos completos de cartões e relatórios.

## Como executar

O projeto usa Node.js e npm. Para executar uma cópia do código:

```bash
npm install
npm run dev
```

A aplicação precisa das configurações de ambiente do Lovable Cloud/Supabase fornecidas para o projeto. Configure-as no ambiente local ou na plataforma de deploy; não publique arquivos `.env` com valores privados. Nunca coloque chaves administrativas ou secrets no frontend.

Comandos disponíveis:

```bash
npm run dev       # desenvolvimento
npm run build     # build de produção
npm run test      # testes automatizados
npm run lint      # análise estática
```

## Reflexão sobre o processo de Vibe Coding

Texto-base para personalizar antes de entregar:

- **O que funcionou bem?** Trabalhar por etapas e revisar requisitos antes de mudanças estruturais ajudou a preservar o projeto existente. A conversa com a IA também permitiu detalhar regras de segurança e validar a fundação do banco antes de conectar os fluxos de autenticação e persistência.
- **O que não funcionou como esperado?** A conexão ao banco não migrou automaticamente os dados que já estavam no navegador. Além disso, a interpretação de texto pode confundir números que aparecem fora de um valor financeiro, como ocorreu com a expressão de teste `teste-e2e`.
- **O que aprendi sobre conversar com IAs?** Instruções específicas sobre escopo, segurança, limitações e critérios de validação tornam as respostas mais úteis. Também é importante separar o que foi planejado do que foi realmente implementado e testar fluxos reais antes de declarar uma etapa concluída.

> Ajuste esta reflexão para representar sua experiência pessoal com o processo e com as ferramentas utilizadas.

## Próximos passos

1. Implementar a migração segura dos dados antigos do navegador, com revisão do usuário antes de importar.
2. Ampliar a integração das telas de contas, categorias, orçamentos e movimentações.
3. Definir e validar regras de negócio de cartões e faturas antes de construir esses fluxos.
4. Evoluir os relatórios e avaliar a assistente financeira com IA em uma fase posterior.
5. Adicionar capturas reais do aplicativo a este README.

## Repositório

Projeto de entrega: [dio-lab-vibe-coding-app-financas](https://github.com/luizkarlus/dio-lab-vibe-coding-app-financas)

---

Desenvolvido por **Luiz Carlos França** como projeto de aprendizagem do desafio de Vibe Coding da DIO.
