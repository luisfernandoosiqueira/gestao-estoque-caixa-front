# 💻 Frontend — Gestão de Caixa e Estoque

Aplicação **Angular 19** desenvolvida para consumo da API de **Gestão de Caixa e Estoque**, permitindo controlar produtos, vendas (caixa), movimentações de estoque e usuários (administradores e operadores), com interface simples, responsiva e moderna.

---

## ⚙️ Tecnologias utilizadas

- **Angular 19 (standalone components)**
- **PrimeNG + PrimeFlex + PrimeIcons**
- **SweetAlert2** para alertas e confirmações
- **TypeScript / HTML / SCSS**
- **RxJS + HttpClient**
- **Roteamento com Angular Router**
- Integração com backend via **proxy (`/api` → `http://localhost:8080`)**

---

## 📚 Funcionalidades principais

### 🔐 Autenticação & Perfis

- Tela de **login** com validação de e-mail e senha.
- **Cadastro de usuário** acessível a partir da tela de login (sem necessidade de estar autenticado).
- Perfis de usuário:
  - `ADMINISTRADOR`
  - `OPERADOR`
- Proteção de rotas com `authGuard`, `adminGuard` e `caixaGuard`.
- Usuários **inativos** não conseguem acessar o sistema.

### 👤 Usuários

- Tela de **listagem** com filtros por:
  - Perfil (`ADMINISTRADOR` / `OPERADOR`)
  - Status (`Ativo` / `Inativo`)
- **Cadastro e edição** de usuários com validações básicas.
- **Ativação/Inativação** de usuários:
  - Usuário **não é excluído fisicamente**, apenas ativado/inativado.
  - Usuários inativos não podem registrar vendas.
- Feedback visual com **SweetAlert2** para sucesso/erro/confirmações.

### 📦 Produtos

- Tela de **listagem** de produtos.
- **Cadastro e edição** com campos como:
  - Código
  - Nome
  - Categoria
  - Quantidade em estoque
  - Preço unitário
  - Status (ativo/inativo)
- Produtos inativos não devem ser usados em novas operações, conforme regras definidas no backend.

### 🧾 Vendas / Caixa

- Tela para **registrar vendas**:
  - Seleção do **usuário (operador)** responsável.
  - Seleção de produtos e quantidades.
  - Validação de **estoque disponível**.
  - Cálculo automático de **subtotal**, **total** e **troco**.
  - Validação de **valor recebido ≥ total da venda**.
- Listagem de vendas e tela de **detalhamento** de uma venda específica.
- Apenas usuários **ativos** podem ser selecionados na tela de venda.

### 📊 Movimentações de Estoque

- Tipos de movimentação:
  - `ENTRADA`
  - `SAIDA`
  - `AJUSTE`
- Tela de **cadastro de movimentação**:
  - Seleção de produto
  - Tipo de movimentação
  - Quantidade
  - Motivo (texto opcional)
- Tela de **listagem**, com:
  - Filtro por **período (data inicial/final)**
  - Filtro por **tipo de movimentação**
  - Exibição de:
    - Data/hora
    - Produto
    - Tipo (com destaque visual)
    - Quantidade
    - Motivo
- Movimentações são **registradas para histórico** (não há exclusão na interface).

### 🏠 Tela inicial / Home

- Após login, o usuário é redirecionado para a **Home / Dashboard**.
- Atalhos para:
  - Produtos
  - Vendas
  - Movimentações de estoque
  - Usuários (para administradores)

---

## 📁 Estrutura do projeto

```text
src/app/
 ├─ app.routes.ts                 → Definição das rotas principais
 ├─ app.config.ts (se aplicável)  → Configuração raiz (providers/Bootstrap)
 │
 ├─ core/
 │   ├─ auth/
 │   │   ├─ auth.service.ts       → Login, armazenamento de usuário logado
 │   │   ├─ auth.guard.ts         → Bloqueio de rotas sem login
 │   │   ├─ admin.guard.ts        → Restrição para ADMINISTRADOR
 │   │   ├─ caixa.guard.ts        → Restrição para ADMIN/OPERADOR em vendas
 │   │   └─ unsaved-changes.guard.ts → Guard para formulários com alterações não salvas
 │   │
 │   ├─ alert/
 │   │   └─ alert.service.ts      → Wrapper para SweetAlert2 (sucesso, erro, confirmação)
 │   │
 │   ├─ models/
 │   │   ├─ produto.model.ts
 │   │   ├─ usuario.model.ts
 │   │   ├─ item-venda.model.ts
 │   │   ├─ venda.model.ts
 │   │   └─ movimentacao.model.ts
 │   │
 │   └─ services/
 │       ├─ produtos.service.ts       → /api/produtos
 │       ├─ usuarios.service.ts       → /api/usuarios
 │       ├─ vendas.service.ts         → /api/vendas
 │       └─ movimentacoes.service.ts  → /api/movimentacoes
 │
 └─ pages/
     ├─ login/
     │   ├─ login.component.ts
     │   ├─ login.component.html
     │   └─ login.component.scss
     │
     ├─ home/
     │   ├─ home.component.ts
     │   ├─ home.component.html
     │   └─ home.component.scss
     │
     ├─ dashboard/
     │   ├─ dashboard.component.ts
     │   ├─ dashboard.component.html
     │   └─ dashboard.component.scss
     │
     ├─ produtos/
     │   ├─ lista-produto/
     │   └─ cadastro-produto/
     │
     ├─ usuarios/
     │   ├─ lista-usuario/
     │   └─ cadastro-usuario/
     │
     ├─ vendas/
     │   ├─ lista-venda/
     │   ├─ cadastro-venda/
     │   └─ detalhe-venda/
     │
     └─ movimentacoes/
         ├─ lista-movimentacao/
         └─ cadastro-movimentacao/
________________________________________
🔗 Integração com o backend
•	Base da API (via proxy): /api → http://localhost:8080
•	Exemplos de endpoints:
o	Usuários: /api/usuarios
o	Produtos: /api/produtos
o	Vendas: /api/vendas
o	Movimentações: /api/movimentacoes
o	Login/autenticação: conforme endpoint configurado no backend (ex.: /api/login)
O CORS é tratado no backend (por exemplo, via @CrossOrigin("*") ou configuração global).
________________________________________
▶️ Como executar o projeto
1.	Certifique-se de ter o Node.js 18+ e o Angular CLI 19 instalados.
2.	Na pasta do projeto, instale as dependências:
3.	npm install
4.	Certifique-se de que o backend (Spring Boot) esteja rodando em http://localhost:8080.
5.	Inicie o frontend:
6.	ng serve
ou, se desejar garantir o uso do proxy:
ng serve --proxy-config src/proxy.conf.json
7.	Acesse o app em: http://localhost:4200
________________________________________
🧠 Observações
•	Toda a UI foi construída com PrimeNG + PrimeFlex, usando componentes como p-table, p-dropdown, p-calendar, p-card, p-button etc.
•	As mensagens de confirmação, erro e sucesso utilizam SweetAlert2, centralizadas no AlertService.
•	Os formulários de cadastro/edição utilizam Reactive Forms, com validações e guard de alterações não salvas.
•	Usuários inativos:
o	Não conseguem fazer login.
o	Não podem ser usados para registrar vendas.
•	Movimentações de estoque e vendas impactam diretamente os estoques dos produtos, conforme regras implementadas no backend.
________________________________________
🧩 Projeto acadêmico desenvolvido em conjunto com o backend Gestão de Caixa e Estoque API (Spring Boot 3), seguindo arquitetura REST, boas práticas de Angular 19 e layout responsivo com PrimeNG.

