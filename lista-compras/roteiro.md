# Hands-On: Lista de Compras Inteligente

**IBMEC BH — Desenvolvimento Web — 2026.1**

`querySelector` · `localStorage` · `CSS Grid` · `Barra de Progresso`

---

| | |
|---|---|
| **Modalidade** | Individual, em aula |
| **Duração** | 90 minutos |
| **Entrega** | GitHub Classroom — um único arquivo: `lista-compras.html` |
| **Pré-requisitos** | Aulas 01–08 (HTML semântico, CSS, Box Model, Flexbox, Grid) |

---

## Objetivos da atividade

- Usar `querySelector` e `querySelectorAll` para acessar o DOM
- Persistir dados com `localStorage` (`setItem`, `getItem`, JSON)
- Organizar o layout com CSS Grid
- Atualizar uma barra de progresso dinamicamente
- Aplicar delegação de eventos em listas dinâmicas

> 💡 **Como vamos trabalhar**
> O roteiro é dividido em 6 blocos. Cada bloco termina com um resultado visível no navegador.
> Antes de avançar, abra o arquivo no browser e confirme que o bloco funciona como esperado.
> As seções com ✏️ indicam trechos que você deve completar. Não copie e cole — digitar ajuda a fixar.

---

# Bloco 1 — Esqueleto HTML e primeiros estilos

*Tempo estimado: 10 min*

Neste bloco você cria o arquivo, define as variáveis CSS e monta a estrutura visual do cabeçalho. Ao final, já deve aparecer algo colorido no navegador.

---

## Passo 1 — Crie o arquivo

- Abra o VS Code e crie uma pasta chamada `lista-compras`.
- Dentro dela, crie o arquivo `lista-compras.html`.
- Cole a estrutura abaixo como ponto de partida:

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Lista de Compras</title>
  <style>
    /* Os estilos vão entrar aqui */
  </style>
</head>
<body>

  <!-- O HTML vai entrar aqui -->

  <script>
    // O JavaScript vai entrar aqui
  </script>
</body>
</html>
```

---

## Passo 2 — Design Tokens (variáveis CSS)

Custom Properties (ou "variáveis CSS") centralizam cores, tamanhos e espaçamentos. Mude uma variável e o efeito se propaga por todo o documento. Substitua o comentário dentro de `<style>` pelo bloco abaixo:

```css
:root {
  --azul:    #003366;
  --amarelo: #FFCC00;
  --bg:      #f2f4f8;
  --surface: #ffffff;
  --borda:   #dce1ea;
  --texto:   #1e2535;
  --muted:   #6b7590;
  --verde:   #1a7f4b;
  --perigo:  #c0392b;
}

*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

body {
  font-family: "Segoe UI", Arial, sans-serif;
  background: var(--bg);
  color: var(--texto);
  min-height: 100dvh;
  padding: clamp(16px, 4vw, 48px);
}
```

---

## Passo 3 — Cabeçalho da página

Dentro de `<body>`, adicione o cabeçalho. Ele usa Flexbox para alinhar a badge com o título:

```html
<!-- Cabeçalho -->
<header class="cabecalho">
  <span class="badge">IBMEC BH</span>
  <div>
    <h1>Lista de Compras</h1>
    <p>querySelector · localStorage · CSS Grid · Progresso</p>
  </div>
</header>
```

Adicione o estilo correspondente no `<style>`:

```css
.cabecalho {
  background: var(--azul);
  color: #fff;
  border-radius: 16px;
  padding: 24px 32px;
  margin-bottom: 28px;
  display: flex;
  align-items: center;
  gap: 20px;
}
.cabecalho h1 { font-size: clamp(1.1rem, 2.4vw, 1.6rem); }
.cabecalho p  { font-size: 0.85rem; opacity: .75; margin-top: 4px; }

.badge {
  background: var(--amarelo);
  color: var(--azul);
  font-weight: 900;
  font-size: 0.8rem;
  padding: 6px 14px;
  border-radius: 6px;
  white-space: nowrap;
}
```

> ✅ **Resultado esperado — Bloco 1**
> - Cabeçalho azul escuro com badge amarela e texto branco.
> - Fundo da página cinza claro (`#f2f4f8`).
> - Se algo não aparecer, abra o Console do DevTools (`F12`) e verifique erros.

---

# Bloco 2 — Layout com CSS Grid

*Tempo estimado: 15 min*

Aqui você cria o grid de duas colunas que organiza o painel principal (coluna 1) e o painel de resumo (coluna 2). Em telas pequenas, o grid automaticamente colapsa para uma coluna.

> 📖 **Como funciona o CSS Grid aqui**
>
> ```
> grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
> ```
>
> - **`auto-fit`**: o navegador decide quantas colunas cabem.
> - **`minmax(320px, 1fr)`**: cada coluna tem no mínimo 320px e no máximo 1 fração do espaço disponível.
> - **Resultado**: 2 colunas em telas largas, 1 coluna em telas estreitas — sem `@media query`.

---

## Passo 4 — Container do grid e cards

Adicione o HTML abaixo, logo após o `</header>`. Os dois `<section>` serão as colunas do grid:

```html
<main class="grid-app">

  <!-- Coluna 1: painel principal -->
  <section class="card" id="painel-principal">
    <h2 class="titulo-card">Minha Lista de Compras</h2>
    <!-- Conteúdo vem nos próximos blocos -->
    <p style="color: var(--muted);">Conteúdo do painel principal aqui.</p>
  </section>

  <!-- Coluna 2: resumo -->
  <section class="card" id="painel-resumo">
    <h2 class="titulo-card">Resumo</h2>
    <!-- Conteúdo vem nos próximos blocos -->
    <p style="color: var(--muted);">Conteúdo do resumo aqui.</p>
  </section>

</main>
```

Agora adicione o CSS dos cards e do grid:

```css
.grid-app {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
  gap: clamp(16px, 2.5vw, 28px);
  align-items: start;
}

.card {
  background: var(--surface);
  border-radius: 16px;
  box-shadow: 0 2px 12px rgba(0,0,0,.08);
  padding: clamp(18px, 3vw, 28px);
}

.titulo-card {
  font-size: clamp(0.95rem, 1.5vw, 1.1rem);
  font-weight: 700;
  color: var(--azul);
  border-left: 4px solid var(--amarelo);
  padding-left: 10px;
  margin-bottom: 16px;
}
```

---

## Passo 5 — Grid interno: painel de estatísticas

Dentro do card de resumo, substitua o parágrafo provisório por três caixas de números. Este é um segundo uso do Grid, com colunas fixas iguais:

```html
<!-- Dentro do <section id="painel-resumo"> -->
<div class="grid-stats">

  <div class="stat-box">
    <span class="numero" id="stat-total">0</span>
    <span class="rotulo">Total</span>
  </div>

  <div class="stat-box">
    <span class="numero" id="stat-pendentes">0</span>
    <span class="rotulo">Pendentes</span>
  </div>

  <div class="stat-box destaque">
    <span class="numero" id="stat-comprados">0</span>
    <span class="rotulo">Comprados</span>
  </div>

</div>
```

```css
.grid-stats {
  display: grid;
  grid-template-columns: repeat(3, 1fr);  /* 3 colunas iguais */
  gap: 12px;
  margin-bottom: 20px;
}

.stat-box {
  background: #f4f6fb;
  border-radius: 10px;
  padding: 14px 10px;
  text-align: center;
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.stat-box .numero {
  font-size: clamp(1.4rem, 2.5vw, 1.8rem);
  font-weight: 800;
  color: var(--azul);
}

.stat-box .rotulo {
  font-size: 0.72rem;
  color: var(--muted);
  text-transform: uppercase;
  letter-spacing: .04em;
}

.stat-box.destaque .numero { color: var(--verde); }
```

> ✅ **Resultado esperado — Bloco 2**
> - Dois cards brancos lado a lado em tela larga.
> - Card da direita com três caixas de número "0" em grid de 3 colunas.
> - Reduza a janela: os dois cards devem empilhar verticalmente.
> - Inspecione o `.grid-app` no DevTools e observe as linhas do grid.

---

# Bloco 3 — Barra de progresso e formulário

*Tempo estimado: 15 min*

Neste bloco você adiciona a barra de progresso e o formulário de adição ao painel principal. Ainda sem JavaScript — apenas a estrutura visual.

---

## Passo 6 — Barra de progresso

Adicione o HTML abaixo dentro do `<section id="painel-principal">`, logo após o `.titulo-card`. A barra é construída com dois divs: a trilha (fundo cinza) e o preenchimento (azul, largura dinâmica):

```html
<!-- Barra de progresso -->
<div class="secao-progresso">
  <div class="cabec-progresso">
    <span>Progresso</span>
    <strong id="label-progresso">0 de 0 itens</strong>
  </div>
  <div class="trilha-progresso">
    <div class="preenchimento" id="barra-progresso"></div>
  </div>
</div>
```

```css
.secao-progresso  { margin-bottom: 20px; }

.cabec-progresso {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  font-size: 0.85rem;
  color: var(--muted);
  margin-bottom: 8px;
}

.cabec-progresso strong { color: var(--texto); font-size: 1rem; }

.trilha-progresso {
  width: 100%;
  height: 14px;
  background: #e4e8f0;
  border-radius: 99px;
  overflow: hidden;
}

.preenchimento {
  height: 100%;
  width: 0%;   /* JavaScript vai alterar este valor */
  background: linear-gradient(90deg, #003366, #0055aa);
  border-radius: 99px;
  transition: width .5s ease, background .4s ease;
}

.preenchimento.concluido {
  background: linear-gradient(90deg, #1a7f4b, #22a060);
}
```

> 💡 **Por que não usar o elemento `<progress>` nativo?**
> O `<progress>` do HTML tem suporte inconsistente para estilização entre navegadores.
> A abordagem profissional é usar dois divs: um como trilha e outro como preenchimento
> controlado por CSS + JavaScript.

---

## Passo 7 — Formulário de adição

Logo após a barra de progresso, adicione o formulário. O atributo `novalidate` desativa a validação nativa — você fará a validação manualmente no próximo bloco:

```html
<!-- Formulário de adição -->
<form class="form-adicao" id="form-adicao" novalidate>
  <input
    type="text"
    id="input-item"
    placeholder="Ex.: Arroz, Feijão, Leite..."
    autocomplete="off"
    maxlength="60"
    aria-label="Nome do item"
    required
  >
  <button type="submit" class="btn btn-primario">Adicionar</button>
</form>
```

```css
.form-adicao {
  display: flex;
  gap: 8px;
  margin-bottom: 20px;
}

.form-adicao input[type="text"] {
  flex: 1;
  padding: 10px 14px;
  border: 2px solid var(--borda);
  border-radius: 6px;
  font-size: 0.92rem;
  font-family: inherit;
  outline: none;
  transition: border-color .2s;
}
.form-adicao input[type="text"]:focus { border-color: var(--azul); }

.btn {
  padding: 10px 18px;
  border: none;
  border-radius: 6px;
  font-family: inherit;
  font-size: 0.88rem;
  font-weight: 600;
  cursor: pointer;
  transition: opacity .15s, transform .1s;
}
.btn:active { transform: scale(.97); }
.btn-primario { background: var(--azul); color: #fff; }
.btn-primario:hover { opacity: .88; }

.btn-perigo {
  background: transparent;
  color: var(--perigo);
  border: 1.5px solid var(--perigo);
  padding: 5px 10px;
  font-size: 0.78rem;
}
.btn-perigo:hover { background: #fdf0ee; }

.btn-limpar {
  background: transparent;
  color: var(--muted);
  border: 1.5px solid var(--borda);
  width: 100%;
  margin-top: 12px;
  padding: 8px;
  font-size: 0.82rem;
}
.btn-limpar:hover { background: #f5f6fa; }
```

---

## Passo 8 — Lista e mensagem de vazio

Adicione a lista e o botão de limpeza logo após o formulário, ainda dentro do painel principal:

```html
<!-- Lista de itens -->
<ul class="lista-itens" id="lista-itens" aria-live="polite">
  <li class="msg-vazia" id="msg-vazia">Nenhum item adicionado ainda.</li>
</ul>

<button class="btn btn-limpar" id="btn-limpar">
  Limpar lista completa
</button>
```

```css
.lista-itens {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 8px;
  max-height: 360px;
  overflow-y: auto;
  padding-right: 4px;
  scrollbar-width: thin;
}

.msg-vazia {
  text-align: center;
  color: var(--muted);
  font-size: 0.88rem;
  padding: 28px 0;
}

.item-linha {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px 12px;
  border: 1.5px solid var(--borda);
  border-radius: 6px;
  background: #fafbfd;
  transition: background .2s, border-color .2s;
}
.item-linha:hover { border-color: #b0bbcf; }
.item-linha.marcado { background: #f0faf4; border-color: #a8dfc0; }

.item-linha input[type="checkbox"] {
  width: 18px; height: 18px;
  accent-color: var(--verde);
  cursor: pointer; flex-shrink: 0;
}

.item-rotulo {
  flex: 1;
  font-size: 0.92rem;
  cursor: pointer;
  transition: color .2s;
}
.item-linha.marcado .item-rotulo {
  text-decoration: line-through;
  color: var(--muted);
}
```

> ✅ **Resultado esperado — Bloco 3**
> - Barra de progresso cinza visível no topo do painel esquerdo (largura 0% por enquanto).
> - Campo de texto + botão "Adicionar" funcionando visualmente (sem ação ainda).
> - Mensagem "Nenhum item adicionado ainda." centralizada na área da lista.
> - Botão "Limpar lista completa" no rodapé do card.

---

# Bloco 4 — JavaScript: estado, localStorage e renderização

*Tempo estimado: 20 min*

Aqui entra o JavaScript. A estratégia é separar três responsabilidades: (1) manter o estado em memória, (2) persistir no `localStorage`, (3) renderizar o HTML a partir do estado.

---

## Passo 9 — Referências DOM com querySelector

Sempre que precisa de um elemento do HTML no JavaScript, usa-se `querySelector`. A boa prática é centralizar todas as referências no topo do script, em vez de repetir o seletor espalhado pelo código. Dentro do `<script>`, comece assim:

```js
// ── 1. REFERÊNCIAS DOM ──────────────────────────────────────
// querySelector retorna O PRIMEIRO elemento que bate com o seletor CSS.
// querySelectorAll retorna UMA LISTA de todos os elementos que batem.

const form      = document.querySelector('#form-adicao');
const inputEl   = document.querySelector('#input-item');
const listaEl   = document.querySelector('#lista-itens');
const msgVazia  = document.querySelector('#msg-vazia');
const barra     = document.querySelector('#barra-progresso');
const labelProg = document.querySelector('#label-progresso');
const btnLimpar = document.querySelector('#btn-limpar');

const statTotal = document.querySelector('#stat-total');
const statPend  = document.querySelector('#stat-pendentes');
const statComp  = document.querySelector('#stat-comprados');
```

---

## Passo 10 — Estado e localStorage

> 📖 **O que é "estado" em uma aplicação?**
> Estado é o conjunto de dados que define o que a interface deve mostrar em um dado momento.
> Nesta aplicação, o estado é um array de objetos — cada objeto representa um item da lista.
>
> Exemplo: `[{ id: 1716300000000, nome: "Arroz", checked: false }, ...]`
>
> Toda vez que o estado muda, re-renderizamos a lista. Nunca manipulamos o DOM diretamente.

```js
// ── 2. ESTADO ────────────────────────────────────────────────
const CHAVE = 'lista_compras';   // chave usada no localStorage

let itens  = carregarStorage();  // inicia com o que estava salvo
let filtro = 'todos';

// ── 3. PERSISTÊNCIA ──────────────────────────────────────────
function carregarStorage() {
  const salvo = localStorage.getItem(CHAVE);
  // getItem devolve null se a chave não existe.
  // O operador ternário evita JSON.parse(null), que geraria erro.
  return salvo ? JSON.parse(salvo) : [];
}

function salvarStorage() {
  // localStorage só armazena strings.
  // JSON.stringify transforma o array em texto; JSON.parse reverte.
  localStorage.setItem(CHAVE, JSON.stringify(itens));
}
```

---

## Passo 11 — Função de progresso

Esta função lê o array de itens, calcula a porcentagem de concluídos e atualiza a barra e os contadores:

```js
// ── 4. PROGRESSO ─────────────────────────────────────────────
function atualizarProgresso() {
  const total  = itens.length;
  const feitos = itens.filter(i => i.checked).length;
  const pct    = total === 0 ? 0 : Math.round((feitos / total) * 100);

  barra.style.width = pct + '%';
  barra.classList.toggle('concluido', pct === 100 && total > 0);
  labelProg.textContent = `${feitos} de ${total} ${total === 1 ? 'item' : 'itens'}`;

  statTotal.textContent = total;
  statPend.textContent  = total - feitos;
  statComp.textContent  = feitos;
}
```

---

## Passo 12 — Função de renderização

A função `renderizar()` apaga os itens existentes na lista e os redesenha a partir do array. Ela é chamada sempre que o estado muda:

```js
// ── 5. RENDERIZAÇÃO ──────────────────────────────────────────
function renderizar() {
  // Remove <li> antigos (preserva a msgVazia)
  listaEl.querySelectorAll('.item-linha').forEach(el => el.remove());

  const filtrados = itens.filter(item => {
    if (filtro === 'pendentes') return !item.checked;
    if (filtro === 'comprados') return  item.checked;
    return true;
  });

  if (filtrados.length === 0) {
    msgVazia.style.display = 'block';
  } else {
    msgVazia.style.display = 'none';

    filtrados.forEach(item => {
      const li = document.createElement('li');
      li.className  = 'item-linha' + (item.checked ? ' marcado' : '');
      li.dataset.id = item.id;

      const checkbox = document.createElement('input');
      checkbox.type    = 'checkbox';
      checkbox.checked = item.checked;
      checkbox.id      = 'chk-' + item.id;

      const label = document.createElement('label');
      label.className   = 'item-rotulo';
      label.htmlFor     = 'chk-' + item.id;
      label.textContent = item.nome;

      const btnDel = document.createElement('button');
      btnDel.className   = 'btn btn-perigo';
      btnDel.textContent = 'Remover';

      li.append(checkbox, label, btnDel);
      listaEl.appendChild(li);
    });
  }

  atualizarProgresso();
}

// Primeira renderização ao carregar a página
renderizar();
```

> ✅ **Resultado esperado — Bloco 4**
> - A página carrega sem erros no console (`F12 → Console`).
> - Teste no console: `itens.push({id:1, nome:"Teste", checked:false}); renderizar()` — o item deve aparecer na lista.
> - `localStorage.getItem("lista_compras")` no console deve retornar `"[]"`.
> - A barra e os contadores mostram "0 de 0 itens".

---

# Bloco 5 — Eventos: adicionar, marcar e remover

*Tempo estimado: 20 min*

Agora a aplicação ganha vida. Você vai conectar os elementos HTML às funções do Bloco 4 por meio de event listeners.

---

## Passo 13 — Adicionar item (submit do formulário)

```js
// ── 6. EVENTOS ───────────────────────────────────────────────

// 6a. Adicionar item
form.addEventListener('submit', function(event) {
  event.preventDefault();  // impede o recarregamento da página

  const nome = inputEl.value.trim();
  if (!nome) { inputEl.focus(); return; }

  const novoItem = {
    id:      Date.now(),  // timestamp como ID único
    nome:    nome,
    checked: false,
  };

  itens.push(novoItem);
  salvarStorage();
  renderizar();

  inputEl.value = '';
  inputEl.focus();
});
```

> 💡 **Por que `event.preventDefault()`?**
> O comportamento padrão de um `<form>` ao ser submetido é recarregar a página.
> `preventDefault()` cancela esse comportamento, permitindo que o JavaScript trate o envio.

---

## Passo 14 — Marcar e remover (delegação de eventos)

> 📖 **Delegação de eventos**
> Se você adicionar um listener diretamente em cada `<li>`, terá um problema: novos itens criados dinamicamente não existiam quando os listeners foram registrados.
>
> A solução é colocar **um único listener no elemento pai** (`<ul>`) e usar `event.target` para descobrir qual filho foi clicado. Isso se chama **delegação de eventos**.

```js
// 6b. Marcar como comprado (delegado ao <ul>)
listaEl.addEventListener('change', function(event) {
  if (event.target.type !== 'checkbox') return;

  const id   = Number(event.target.closest('.item-linha').dataset.id);
  const item = itens.find(i => i.id === id);
  if (!item) return;

  item.checked = event.target.checked;
  salvarStorage();
  renderizar();
});

// 6c. Remover item (delegado ao <ul>)
listaEl.addEventListener('click', function(event) {
  if (!event.target.classList.contains('btn-perigo')) return;

  const id = Number(event.target.closest('.item-linha').dataset.id);
  itens = itens.filter(i => i.id !== id);
  salvarStorage();
  renderizar();
});

// 6d. Limpar lista
btnLimpar.addEventListener('click', function() {
  if (itens.length === 0) return;
  if (!confirm('Deseja apagar todos os itens?')) return;
  itens = [];
  salvarStorage();
  renderizar();
});
```

> ✅ **Resultado esperado — Bloco 5**
> - Digitar um item e clicar em "Adicionar" (ou pressionar Enter) insere o item na lista.
> - A barra de progresso e os contadores atualizam imediatamente.
> - Marcar o checkbox risca o texto e muda o fundo do item para verde claro.
> - Clicar em "Remover" apaga o item. "Limpar lista" pede confirmação e zera tudo.
> - **Feche a aba e reabra: todos os itens ainda estão lá** (localStorage funcionando).

---

# Bloco 6 — Filtros e polimentos finais

*Tempo estimado: 10 min*

O último bloco adiciona os botões de filtro (Todos / Pendentes / Comprados) e uma notificação visual (toast) de feedback ao usuário.

---

## Passo 15 — Filtros

Adicione os botões no HTML, entre o formulário e a lista:

```html
<!-- Filtros — cole após o </form> do form-adicao -->
<div class="filtros" role="group" aria-label="Filtrar itens">
  <button class="btn-filtro ativo" data-filtro="todos">Todos</button>
  <button class="btn-filtro"       data-filtro="pendentes">Pendentes</button>
  <button class="btn-filtro"       data-filtro="comprados">Comprados</button>
</div>
```

```css
.filtros { display: flex; gap: 6px; margin-bottom: 14px; flex-wrap: wrap; }

.btn-filtro {
  padding: 5px 14px;
  border: 1.5px solid var(--borda);
  border-radius: 99px;
  background: transparent;
  font-family: inherit;
  font-size: 0.8rem;
  cursor: pointer;
  color: var(--muted);
  transition: all .15s;
}
.btn-filtro.ativo {
  background: var(--azul);
  border-color: var(--azul);
  color: #fff;
}
```

Conecte os filtros no JavaScript:

```js
// 6e. Filtros
const btnsFiltro = document.querySelectorAll('.btn-filtro');

btnsFiltro.forEach(btn => {
  btn.addEventListener('click', function() {
    btnsFiltro.forEach(b => b.classList.remove('ativo'));
    this.classList.add('ativo');
    filtro = this.dataset.filtro;
    renderizar();
  });
});
```

---

## Passo 16 — Toast de notificação

O toast é uma mensagem temporária que aparece por 2 segundos. Adicione o HTML antes de `</body>`:

```html
<!-- Toast — adicione antes de </body> -->
<div class="toast" id="toast" role="status" aria-live="assertive"></div>
```

```css
.toast {
  position: fixed;
  bottom: 28px;
  left: 50%;
  transform: translateX(-50%) translateY(80px);
  background: var(--azul);
  color: #fff;
  padding: 10px 24px;
  border-radius: 99px;
  font-size: 0.88rem;
  font-weight: 600;
  opacity: 0;
  transition: transform .3s ease, opacity .3s ease;
  pointer-events: none;
  z-index: 999;
}
.toast.visivel {
  transform: translateX(-50%) translateY(0);
  opacity: 1;
}
```

No JavaScript, adicione a função e chame-a nos eventos de adicionar, marcar e remover:

```js
// ── 7. TOAST ─────────────────────────────────────────────────
const toastEl = document.querySelector('#toast');
let timerToast;

function mostrarToast(msg) {
  toastEl.textContent = msg;
  toastEl.classList.add('visivel');
  clearTimeout(timerToast);
  timerToast = setTimeout(() => toastEl.classList.remove('visivel'), 2200);
}

// Onde chamar:
// Após itens.push(novoItem)  → mostrarToast(`"${nome}" adicionado!`);
// Após item.checked = true   → mostrarToast(`"${item.nome}" comprado!`);
// Após itens = []            → mostrarToast('Lista limpa!');
```

> ✅ **Resultado esperado — Bloco 6**
> - Os três botões de filtro aparecem entre o formulário e a lista.
> - "Pendentes" mostra apenas itens não marcados; "Comprados" mostra os marcados.
> - Ao adicionar um item, aparece uma notificação azul no rodapé por 2 segundos.
> - Toda a aplicação funciona offline, sem nenhuma requisição de rede.

---

# Desafios para quem terminar cedo

Estes itens não são avaliados, mas são ótimas práticas para consolidar o aprendizado.

### Desafio A — Editar um item

Adicione um botão "Editar" em cada item. Ao clicar, substitua o texto por um `<input>` preenchido com o nome atual. Ao pressionar Enter ou perder o foco (`blur`), salve o novo nome e re-renderize.

### Desafio B — Data de adição

Guarde um campo `createdAt: new Date().toLocaleDateString("pt-BR")` em cada objeto e exiba-o em fonte menor abaixo do nome do item.

### Desafio C — Drag & drop para reordenar

Use os atributos `draggable="true"` e os eventos `dragstart`, `dragover` e `drop` para permitir que o usuário reordene os itens arrastando-os. Atualize o array `itens` na nova ordem e salve.

---

# Rubrica de Avaliação

| Critério | Peso |
|---|:---:|
| Estrutura HTML válida (`DOCTYPE`, `lang`, `meta charset`, `meta viewport`) | 1,0 pt |
| CSS Grid aplicado corretamente no layout principal (`auto-fit`/`minmax`) | 1,5 pt |
| CSS Grid aplicado no painel de estatísticas (3 colunas) | 0,5 pt |
| Barra de progresso atualiza dinamicamente conforme itens são marcados | 2,0 pt |
| `querySelector` usado para todas as referências DOM | 1,0 pt |
| `localStorage` salva e restaura a lista ao recarregar a página | 2,0 pt |
| Delegação de eventos para marcar/remover itens da lista dinâmica | 1,0 pt |
| Filtros Todos / Pendentes / Comprados funcionam corretamente | 1,0 pt |
| **TOTAL** | **10 pts** |

---

# Checklist antes de entregar

Confira cada item antes de fazer o commit no GitHub Classroom:

- [ ] O arquivo se chama `lista-compras.html` (sem acentos, sem espaços)
- [ ] Não há erros no Console do DevTools (`F12`)
- [ ] A barra de progresso se move ao marcar itens
- [ ] Os dados persistem após fechar e reabrir a aba
- [ ] O layout usa duas colunas em tela larga e uma coluna em tela estreita
- [ ] Os três filtros funcionam sem erros
- [ ] Não foi usada nenhuma biblioteca externa (sem jQuery, sem Bootstrap)
- [ ] O código tem comentários explicando as partes principais

---

*Desenvolvimento Web · Ibmec BH · 2026.1*
