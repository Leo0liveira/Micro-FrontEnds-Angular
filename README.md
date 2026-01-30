# Micro Frontends (Angular) — Projetos de Estudo

**Propósito** ✅

Este repositório contém três projetos Angular criados como um **módulo didático** de um curso da **Udemy** sobre Angular, com foco em **Micro Frontends** usando Module Federation. O objetivo é entender conceitos práticos: exposição de módulos, consumo remoto (remotes), roteamento dinâmico e compartilhamento de dependências.

---

## Resumo dos projetos 🔧

- **vendas** (Host / Shell)
  - Porta: `4200`
  - Função: Aplicação host que roteia e carrega remotes dinamicamente.
  - Componentes principais: `App` (shell), `Navbar`, `Home`.
  - Observação: carrega remotes usando `loadRemoteModule` do pacote `@angular-architects/module-federation`.

- **produtos** (Remote)
  - Porta: `4201`
  - Função: Remote que expõe o módulo de produtos e um componente de carrinho.
  - Exports (webpack): `./Component` -> `src/app/app.ts` e `./Carrinho` -> `src/app/carrinho/carrinho.ts`.
  - Componentes principais: `App` (lista de produtos), `Carrinho` (componente simples de carrinho).

- **grafico** (Remote)
  - Porta: `4202`
  - Função: Remote que expõe uma visualização gráfica (Chart.js).
  - Exports (webpack): `./Component` -> `src/app/app.ts`.
  - Componentes principais: `App` (gráfico de barras criado com Chart.js).

---

## Arquitetura e pontos-chave 💡

- Técnica usada: **Module Federation** (via `@angular-architects/module-federation`).
- O `vendas` atua como host e, em `src/app/app.routes.ts`, carrega módulos remotos dinamicamente:

  `loadRemoteModule({ type: 'module', remoteEntry: 'http://localhost:4201/remoteEntry.js', exposedModule: './Component' })`

- Cada remote expõe seus módulos no `webpack.config.js` (ex.: `exposes: { './Component': './src/app/app.ts' }`).
- Dependências compartilhadas (Angular, etc.) são configuradas para **singleton** e versionamento controlado para evitar múltiplas instâncias.

---

## Componentes e comportamento (curto) 🎯

- `vendas`:
  - `Navbar` — links para rotas (home, produtos, carrinho, grafico);
  - `Home` — página inicial do host;
  - `App` — container com `RouterOutlet` para carregar remotes.

- `produtos`:
  - `App` — mostra uma lista de produtos com imagem e preço;
  - `Carrinho` — componente simples que representa o carrinho de compras (exposto como `./Carrinho`).

- `grafico`:
  - `App` — renderiza um gráfico de barras (Chart.js) com dados estáticos de exemplo.

---

## Como executar ▶️

1. Instale dependências em cada projeto (ou globalmente):

   ```bash
   cd vendas && npm install
   cd ../produtos && npm install
   cd ../grafico && npm install
   ```

2. Executar localmente cada app:

   - `npm start` dentro de cada pasta (vendas -> 4200, produtos -> 4201, grafico -> 4202)

3. Ou iniciar todos juntos com o script de desenvolvimento da arquitetura de microfrontends (dev server fornecido por `@angular-architects/module-federation`):

   ```bash
   npm run run:all
   ```

   (rodar a partir de qualquer dos projetos que possuem o script — ele iniciará os microfrontends nas portas configuradas).

4. Acesse o shell/host em: `http://localhost:4200` e navegue para `produtos`, `carrinho` ou `grafico` — o host irá carregar os remotes dinamicamente.

---

## Testes e build ⚙️

- Testes: `npm test` em cada projeto.
- Build: `npm run build` produz os artefatos em `dist/` para cada projeto.

---

> **Observação:** este repositório foi organizado como um exercício prático para compreensão de Micro Frontends com Angular.

---

**README criado por:** GitHub Copilot ✍️

**Contexto:** conteúdo referente a um módulo do curso da *Udemy* sobre Angular (projeto de estudo e demonstração de Module Federation/microfrontends).