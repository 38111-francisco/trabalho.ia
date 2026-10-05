# Contexto do Projeto - Mini Sistema de Gerenciamento de Roupas

## Objetivo do Projeto
Desenvolver um mini sistema web completo para gerenciamento e consulta de roupas (CRUD), com arquitetura dividida em dois projetos independentes:
1. **Backend**: API REST em Node.js, Express, MongoDB (Mongoose), arquitetura serverless compatível com Vercel.
2. **Frontend**: Aplicação web SPA leve e moderna em HTML5, CSS3 puro e JavaScript Vanilla, consumindo a API REST.

## Tecnologias Utilizadas
- **Backend**: Node.js v24, Express.js v4, MongoDB / Mongoose v8, CORS, dotenv, mongodb-memory-server v10 (para testes e desenvolvimento local autônomo), Jest v29 e Supertest v7 para testes de integração.
- **Frontend**: HTML5 semântico, CSS3 moderno (design system exclusivo, dark mode sofisticado, glassmorphism, responsividade, micro-animações, Google Fonts Outfit & Plus Jakarta Sans), JavaScript puro (Fetch API, manipulação reativa do DOM, modais acessíveis e notificações toast).
- **Hospedagem / Infraestrutura**: Vercel (`vercel.json` em ambos os projetos), Git para controle de versão.

## Estrutura Atual
```text
lula2026/
├── backend/
│   ├── .git/
│   ├── api/
│   │   └── index.js
│   ├── config/
│   │   └── db.js
│   ├── controllers/
│   │   └── roupasController.js
│   ├── models/
│   │   └── Roupa.js
│   ├── routes/
│   │   └── roupasRoutes.js
│   ├── tests/
│   │   └── roupas.test.js
│   ├── .env
│   ├── .env.example
│   ├── .gitignore
│   ├── app.js
│   ├── server.js
│   ├── package.json
│   ├── vercel.json
│   ├── README.md
│   ├── Roadmap.md
│   ├── Contexto.md
│   └── api.md
├── frontend/
│   ├── .git/
│   ├── index.html
│   ├── style.css
│   ├── script.js
│   ├── vercel.json
│   ├── .gitignore
│   └── README.md
├── Roadmap.md
├── Contexto.md
└── api.md
```

## Funcionalidades Implementadas e Testadas
- [x] **Backend Completo**:
  - Modelo Mongoose `Roupa` com campos `id`, `marca`, `modelo`, `preco`, `foto` e timestamps.
  - Conexão MongoDB com suporte a serverless cache e `mongodb-memory-server` para desenvolvimento e testes autônomos.
  - CRUD completo nos endpoints `/api/roupas`:
    - `GET /api/roupas` (Listar)
    - `GET /api/roupas/:id` (Buscar)
    - `POST /api/roupas` (Cadastrar com validações)
    - `PUT /api/roupas/:id` (Atualizar com validações)
    - `DELETE /api/roupas/:id` (Excluir)
  - Validações implementadas e testadas:
    - Marca e Modelo obrigatórios e não-vazios.
    - Preço obrigatório e maior que zero (`preco > 0`).
    - URL da Foto validada com formato http/https quando informada.
    - Formato de ID inválido retorna 400.
    - ID inexistente retorna 404.
    - Mensagens de erro em JSON padronizadas: `{ "erro": "..." }`.
  - CORS ativado com suporte a qualquer origem.
  - 16 testes de integração automatizados com Jest + Supertest (100% aprovados).
  - Testes manuais via comandos HTTP no PowerShell confirmando inserção, atualização, consulta e exclusão em tempo real.
- [x] **Frontend Completo**:
  - HTML5 semântico com cabeçalho, barra de ferramentas, catálogo e modais.
  - CSS3 puro responsivo com paleta moderna, suporte a telas menores e computadores, efeitos glassmorphism e micro-animações.
  - Formulário de cadastro com validação em tempo real e visualização de imagem ao digitar a URL.
  - Listagem em cards com fotos, marcas em destaque, modelos e preços formatados em Real (`R$ 149,90`).
  - Modal de edição rápida reaproveitando o formulário com dados pré-preenchidos.
  - Modal de confirmação para exclusão de roupas.
  - Gerenciamento de todos os estados:
    - Carregando (spinner animado)
    - Vazio (aviso estilizado quando não há roupas)
    - Notificações Toast ("Cadastro realizado com sucesso!", "Alteração realizada com sucesso!", "Exclusão realizada com sucesso!")
    - Erro de API (com mensagem clara e botão para tentar novamente)
  - Busca textual e filtro dinâmico de marcas com ordenação por preço e data.
  - Seletor e configurador de URL da API integrado com indicador luminoso (online/offline).
- [x] **Documentação & Versionamento**:
  - `api.md` com documentação detalhada da API e exemplos cURL/Fetch.
  - `README.md` completo para Backend e Frontend.
  - Repositórios Git inicializados de forma independente (`backend` e `frontend`) com histórico de commits semânticos.
  - Arquivos `vercel.json` configurados para deploy serverless do backend e estático do frontend.

## Problemas Encontrados e Corrigidos
1. *Problema:* No primeiro ciclo de testes do backend, a biblioteca `mongodb-memory-server` precisava baixar os binários do MongoDB no Windows e o `deleteMany` inicial no `beforeAll` tentava rodar antes da conexão estar pronta.
   *Solução:* Adicionada a chamada explícita `await connectDB()` no `beforeAll` dos testes.
2. *Problema:* Padronização exata da mensagem de erro do preço exigida pelo edital do projeto.
   *Solução:* Ajustado o retorno para `O preço deve ser maior que zero.`, passando em todos os 16 testes.
3. *Problema:* O download do driver do Playwright pelo subagente de navegador falhou com 404 na CDN pública da Microsoft/Azure.
   *Solução:* Validação de ponta a ponta executada com sucesso diretamente via requisições HTTP locais no servidor ativo (portas 3000 e 8080).

## Última Tarefa Realizada
- Conclusão da documentação, sincronização dos Roadmaps e Contexto, e validação dos critérios de entrega.

## Próxima Tarefa
- Informar o usuário e disponibilizar os detalhes de acesso local, instruções de deploy na Vercel e estrutura dos repositórios GitHub.
