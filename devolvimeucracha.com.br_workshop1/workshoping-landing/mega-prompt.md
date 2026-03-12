# MEGA-PROMPT DEFINITIVO
# Landing Page — Workshop "Criando minha Consultoria Lucrativa"
# Dobras 2 a 6 — Implementação completa no index.html existente

---

## QUEM VOCÊ É E QUAL É A TAREFA

Você é um desenvolvedor front-end sênior especialista em landing pages de alta conversão
para produtos high ticket, com domínio profundo em performance web, animações premium e
design de alto padrão. Você recebe o index.html já existente com a hero section (dobra 1)
completa, e sua tarefa é implementar as DOBRAS 2, 3, 4, 5 e 6 dentro desse mesmo arquivo.

Ao final, o index.html deve ser um site completo, fluido, visualmente premium, com
connect rate mínimo de 90% e score Google PageSpeed ≥ 85 no mobile e ≥ 95 no desktop.

---

## ARQUIVOS DO PROJETO

```
/workshoping-landing/
  ├── index.html           ← hero section já pronta — EDITE este arquivo
  ├── design_system.html   ← lei absoluta de estética — LEIA antes de escrever CSS
  ├── tablets.webp         ← colagem de 4 tablets com pessoas em consultoria
  ├── caderno.webp         ← mão escrevendo em caderno com branding "Consultor Estratégico"
  ├── mentora-palco.jpeg   ← Juliana Paes Garcia no palco com microfone
  └── mega-prompt.md       ← este arquivo
```

**ANTES DE ESCREVER UMA LINHA DE CÓDIGO:**
1. Leia o design_system.html completo e liste todas as variáveis CSS do :root
2. Leia o index.html existente e identifique: variáveis já declaradas, CDNs já importados,
   estrutura do Lenis/ScrollTrigger já inicializado, classes CSS já existentes
3. Só então comece a implementar, dobra por dobra

---

## ══════════════════════════════════════════════════
## BLOCO 1 — PERFORMANCE CRÍTICA (connect rate ≥ 90%)
## ══════════════════════════════════════════════════

Esta landing page trafega via anúncios pagos. Connect rate abaixo de 90% significa
dinheiro desperdiçado em cliques que não carregam. Cada instrução abaixo é obrigatória.

### 1.1 — Estrutura do <head> — ORDEM EXATA E OBRIGATÓRIA

```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Workshop — Criando minha Consultoria Lucrativa</title>

  <!-- PASSO 1: DNS prefetch e preconnect — SEMPRE PRIMEIRO -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link rel="dns-prefetch" href="https://cdnjs.cloudflare.com">

  <!-- PASSO 2: Preload da imagem hero (impacto direto no LCP) -->
  <link rel="preload" as="image" href="mentora.webp">
  <!-- Nota: use o nome exato da imagem da hero que já está no index.html -->

  <!-- PASSO 3: CSS crítico inline — hero section apenas -->
  <!-- Extraia do CSS existente somente as regras que afetam a primeira tela -->
  <style>
    /* CRITICAL CSS — above the fold only */
    /* Cole aqui as regras do :root, body, .hero e elementos visíveis
       antes do primeiro scroll — isso garante FCP < 1.5s */
  </style>

  <!-- PASSO 4: Google Fonts — display=swap OBRIGATÓRIO -->
  <!-- Use exatamente as fontes definidas no design_system.html -->
  <link href="https://fonts.googleapis.com/css2?family=FONTE_DO_DESIGN_SYSTEM&display=swap"
        rel="stylesheet">

  <!-- PASSO 5: Scripts CDN — defer em TODOS, SEM EXCEÇÃO -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js" defer></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/ScrollTrigger.min.js" defer></script>
  <script src="https://unpkg.com/splitting/dist/splitting.min.js" defer></script>
  <script src="https://unpkg.com/@studio-freight/lenis/bundled/lenis.min.js" defer></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/countup.js/2.8.0/countup.umd.min.js" defer></script>

  <!-- NÃO importar Font Awesome CDN completo — ver seção 1.4 abaixo -->
  <!-- NÃO importar Alpine.js a menos que seja estritamente necessário -->
</head>
```

### 1.2 — Regras de carregamento de JavaScript

- **defer em todos os scripts CDN** — nenhum script no <head> sem defer
- **Splitting.js**: inicializar dentro de `document.addEventListener('DOMContentLoaded')`
  chamando UMA ÚNICA VEZ para toda a página:
  ```javascript
  document.addEventListener('DOMContentLoaded', () => {
    Splitting({ target: '[data-splitting]' });
  });
  ```
  Todos os títulos que usam Splitting devem ter o atributo `data-splitting` no HTML.

- **Lenis + ScrollTrigger**: inicializar dentro de `window.addEventListener('load')`:
  ```javascript
  window.addEventListener('load', () => {
    const lenis = new Lenis({ duration: 1.2, easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)) });
    gsap.registerPlugin(ScrollTrigger);
    lenis.on('scroll', ScrollTrigger.update);
    gsap.ticker.add((time) => lenis.raf(time * 1000));
    gsap.ticker.lagSmoothing(0);
    // Todas as timelines ScrollTrigger de todas as dobras vêm aqui dentro
  });
  ```

- **CountUp.js**: instanciar APENAS dentro do callback `onEnter` do ScrollTrigger
  da dobra 6 — não na inicialização da página

- **Nenhum `console.log()`** no código final

### 1.3 — Regras de imagens

| Imagem | Atributos obrigatórios |
|--------|------------------------|
| Hero (dobra 1, já existente) | fetchpriority="high" — SEM loading="lazy" |
| Todas as demais | loading="lazy" + decoding="async" + width + height explícitos |

Exemplo correto para dobras 2–6:
```html
<img
  src="tablets.webp"
  alt="Profissionais em sessão de consultoria"
  loading="lazy"
  decoding="async"
  width="600"
  height="500"
>
```

O width e height previnem layout shift (CLS). Use as dimensões reais de cada arquivo.
Se não souber as dimensões exatas, use proporções corretas (ex: 16:9, 4:3).

### 1.4 — Font Awesome — NÃO usar o CDN completo

O CDN completo do Font Awesome (`all.min.js` ou `all.min.css`) carrega 400KB+
e destrói a performance. Use exclusivamente SVG inline para cada ícone:

```html
<!-- Em vez de: <i class="fa-solid fa-calendar"></i> -->
<!-- Use SVG inline direto: -->
<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16"
     viewBox="0 0 448 512" aria-hidden="true" fill="currentColor">
  <path d="M..."/> <!-- path copiado de fontawesome.com/icons -->
</svg>
```

Para cada ícone necessário, copie o path SVG de fontawesome.com/icons → clique no ícone
→ "Start Using This Icon" → copie o código SVG completo.

### 1.5 — Animações — regras de performance

- **Animar APENAS** `transform` e `opacity` — nunca `width`, `height`, `top`, `left`,
  `margin`, `padding` ou qualquer propriedade que cause reflow
- `will-change: transform, opacity` nos elementos que recebem animação GSAP
- `backface-visibility: hidden` nos cards com animações de transform
- `transform: translateZ(0)` nos elementos com glow/blur (força GPU layer)
- ScrollTrigger com `fastScrollEnd: true` e `preventOverlaps: true` em todas as seções
- `backdrop-filter: blur()` APENAS na dobra 6 (pricing) — nunca em seções inteiras
- Efeito de partículas/tsParticles: desativar completamente em mobile:
  ```javascript
  let mm = gsap.matchMedia();
  mm.add("(min-width: 769px)", () => {
    // inicializa partículas aqui
  });
  ```
- Em mobile, reduzir todos os valores de translateY/X das animações à metade

### 1.6 — CSS — regras de performance

- `@media (prefers-reduced-motion: reduce)` obrigatório:
  ```css
  @media (prefers-reduced-motion: reduce) {
    *, *::before, *::after {
      animation-duration: 0.01ms !important;
      animation-iteration-count: 1 !important;
      transition-duration: 0.01ms !important;
    }
  }
  ```
- Fallback para `backdrop-filter` obrigatório:
  ```css
  @supports not (backdrop-filter: blur(1px)) {
    .glass-element { background: rgba(10, 10, 10, 0.95); }
  }
  ```
- Usar `clamp()` em todos os font-sizes de títulos para responsividade sem media queries
- Containers que recebem conteúdo dinâmico: definir `min-height` para evitar CLS

### 1.7 — Métricas alvo

| Métrica | Mobile | Desktop |
|---------|--------|---------|
| Performance Score | ≥ 85 | ≥ 95 |
| First Contentful Paint | < 2.5s | < 1.5s |
| Largest Contentful Paint | < 4.0s | < 2.5s |
| Cumulative Layout Shift | < 0.1 | < 0.1 |
| Total Blocking Time | < 300ms | < 100ms |

---

## ══════════════════════════════════════════════════
## BLOCO 2 — PRINCÍPIOS GLOBAIS DE DESIGN
## ══════════════════════════════════════════════════

### 2.1 — Visual

- **Todas as cores, fontes e espaçamentos VÊM DO design_system.html** — sem exceção
- Nenhum valor hard-coded de cor: use `var(--color-gold)`, nunca `#C9A84C`
- Fundo escuro consistente em todas as seções — mesma família de cor da hero
- Cards: borda em gradiente dourado via pseudo-elemento `::before` (1.5px padding)
- Border-radius generoso (16–20px) nos containers principais
- Checkmarks: SVG inline customizado — nunca emoji, nunca `<ul><li>` padrão
- Separador entre itens de lista: linha 1px, `opacity: 0.15`
- Hover nos itens: `translateX(6px)` + `scale(1.15)` no ícone, `transition: 250ms`
- Tipografia com `letter-spacing: 0.15–0.25em` nos títulos — sensação de luxo

### 2.2 — Animações — orquestração global

- **Nada anima antes do scroll** — zero animações que disparam no load fora da hero
- ScrollTrigger em todas as seções, trigger padrão: `"top 80%"` (entra quando 20% visível)
- Entradas: `translateY(30–40px)` ou `translateX(20–40px)` + `opacity: 0→1`
- Ease padrão de entrada: `power3.out`
- Títulos: Splitting.js com `stagger: 0.07–0.1s` por palavra
- Saídas (scrub ao sair): `translateY(-15–20px)` + `opacity → 0.6–0.7`
  (nunca some completamente — fica como "fantasma" saindo)
- GSAP `matchMedia()` para desativar/simplificar animações em mobile

### 2.3 — Estrutura de código por dobra

```html
<!-- ════════════════════════════════════ -->
<!-- DOBRA N — NOME DA SEÇÃO             -->
<!-- ════════════════════════════════════ -->

<section id="id-da-secao" aria-label="Nome da seção">
  <!-- HTML semântico da dobra -->
</section>

<style>
  /* ── Dobra N: Nome ─────────────────── */
  /* CSS exclusivo — usando var() do :root existente */
  /* Sem declarações de :root ou @import */
</style>

<!-- JS desta dobra fica DENTRO do window.addEventListener('load') já existente -->
<!-- Não criar novos listeners — apenas adicionar timelines ao bloco já aberto -->
```

### 2.4 — Responsividade — breakpoints globais

| Breakpoint | Layout |
|------------|--------|
| ≥ 1024px (Desktop) | Duas colunas conforme especificado em cada dobra |
| 768–1023px (Tablet) | Colunas compactadas ou empilhadas |
| < 768px (Mobile) | Coluna única, animações com translateY/X reduzidos à metade |

Mobile — regras adicionais:
- Elementos decorativos tipográficos de fundo (watermarks): desativados
- `display: none` em elementos puramente decorativos sem conteúdo semântico
- `font-size` ajustado com `clamp()` mas mantendo impacto visual
- Cards: `padding` interno reduzido, `border-radius: 12px`
- Números decorativos de fundo (01, 02...): `opacity` reduzida para 3–4%

---

## ══════════════════════════════════════════════════
## BLOCO 3 — DOBRA 2: "ESTE WORKSHOP É PARA VOCÊ QUE"
## ══════════════════════════════════════════════════

### Posição
Imediatamente após o `</section>` da hero.

### Separador de transição hero → dobra 2
```html
<div class="section-divider" aria-hidden="true"></div>
```
```css
.section-divider {
  width: 60%;
  height: 1px;
  margin: 0 auto;
  background: linear-gradient(90deg, transparent, var(--color-gold), transparent);
  transform: scaleX(0);
  transform-origin: center;
}
```
Animação GSAP: `scaleX: 0 → 1` ao entrar no viewport.

### Fundo da seção
- Mesma cor escura base da hero
- Textura noise CSS inline (SVG base64, opacidade 3–4%):
  ```css
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256'
    xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E
    %3CfeTurbulence type='fractalNoise' baseFrequency='0.9'
    numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E
    %3Crect width='100%25' height='100%25' filter='url(%23noise)'
    opacity='0.04'/%3E%3C/svg%3E");
  ```
- Elemento decorativo: círculo grande no canto superior direito,
  `opacity: 0.06`, cor dourada, `pointer-events: none`, `overflow: hidden`

### Título da seção
```html
<h2 data-splitting>
  <span class="text-light">Este workshop é</span>
  <span class="text-gold">para você que:</span>
</h2>
```
- `letter-spacing: 0.2em`
- Linha decorativa abaixo (~80px, centralizada, gradiente dourado)
  → GSAP: `scaleX: 0 → 1`
- Splitting.js: palavras entram com `translateY(40px) → 0`, `stagger: 0.1s`

### 4 Cards (grid 2×2 desktop, 1 coluna mobile)

Estrutura de cada card:
```html
<div class="audience-card">
  <span class="card-number" aria-hidden="true">01</span>
  <div class="card-icon"><!-- SVG inline do ícone --></div>
  <p class="card-text">
    <strong class="card-keyword">Você tem a expertise.</strong>
    O que ainda falta é um modelo de negócio sólido que sustente
    uma consultoria de verdade — não um bico bem pago.
  </p>
</div>
```

CSS de cada card:
```css
.audience-card {
  position: relative;
  border-radius: 16px;
  padding: 2rem;
  overflow: hidden;
  /* Borda gradiente via ::before */
}
.audience-card::before {
  content: '';
  position: absolute;
  inset: 0;
  border-radius: inherit;
  padding: 1.5px;
  background: linear-gradient(135deg, var(--color-gold), transparent, var(--color-gold));
  -webkit-mask: linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0);
  -webkit-mask-composite: xor;
  mask-composite: exclude;
  pointer-events: none;
}
.card-number {
  position: absolute;
  font-size: clamp(4rem, 8vw, 7rem);
  font-weight: 900;
  opacity: 0.06;
  color: var(--color-gold);
  right: 1rem;
  top: 0.5rem;
  line-height: 1;
  pointer-events: none;
  user-select: none;
}
```

Hover (CSS puro):
```css
.audience-card {
  transition: transform 300ms ease, box-shadow 300ms ease;
  will-change: transform;
}
.audience-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 32px color-mix(in srgb, var(--color-gold) 25%, transparent);
}
.audience-card:hover .card-icon { transform: rotate(5deg) scale(1.1); }
.audience-card:hover .card-number { opacity: 0.12; }
```

**Conteúdo dos 4 cards:**

**Card 01** — Ícone: SVG de bússola/compass
Keyword: `"Você tem a expertise."`
Texto: O que ainda falta é um modelo de negócio sólido que sustente
uma consultoria de verdade — não um bico bem pago.

**Card 02** — Ícone: SVG de lâmpada/cérebro
Keyword: `"Conhecimento não falta."`
Texto: O que falta é transformá-lo em uma oferta estruturada, com
posicionamento claro e proposta de valor que justifique cobrar alto.

**Card 03** — Ícone: SVG de gráfico crescente
Keyword: `"Você já consultoria."`
Texto: Mas de forma inconsistente, cobrando menos do que merece —
e sabe que falta método para chegar ao próximo nível com previsibilidade.

**Card 04** — Ícone: SVG de rota/caminho
Keyword: `"Você quer um plano real."`
Texto: Direto, aplicável, construído por quem já trilhou esse caminho
e formou centenas de consultores que faturam alto.

### CTA ao final da dobra
```html
<a href="#pricing" class="btn-primary btn-pulse" aria-label="Quero criar minha consultoria">
  QUERO CRIAR MINHA CONSULTORIA
</a>
```
- Centralizado, `max-width: 420px`
- Pulse animation contínua (CSS keyframes, escala + glow)

### Animação de entrada (ScrollTrigger)
```javascript
gsap.timeline({
  scrollTrigger: {
    trigger: "#para-voce",
    start: "top 80%",
    fastScrollEnd: true,
    preventOverlaps: true,
    once: true
  }
})
.to(".section-divider", { scaleX: 1, duration: 0.5 })
.to(".section-title-line", { scaleX: 1, duration: 0.5 }, "-=0.2")
// Splitting.js: palavras do título
.from("[data-splitting='dobra2'] .word", {
  y: 40, opacity: 0, duration: 0.6, stagger: 0.1, ease: "power3.out"
}, "-=0.3")
// Cards com stagger
.from(".audience-card", {
  x: -40, opacity: 0, duration: 0.6, stagger: 0.15, ease: "power3.out"
}, "-=0.2")
// Números decorativos escalam junto
.from(".card-number", {
  scale: 1.3, duration: 0.6, stagger: 0.15
}, "<")
// CTA
.from(".dobra2-cta", {
  scale: 0.9, opacity: 0, duration: 0.5, ease: "elastic.out(1, 0.5)"
}, "-=0.1");
```

Saída (scrub):
```javascript
gsap.to("#para-voce", {
  scrollTrigger: {
    trigger: "#para-voce",
    start: "bottom 80%",
    end: "bottom top",
    scrub: 1
  },
  y: -20, opacity: 0.6
});
```

---

## ══════════════════════════════════════════════════
## BLOCO 4 — DOBRA 3: "O QUE É O WORKSHOP"
## ══════════════════════════════════════════════════

### Separador de transição
Mesmo `.section-divider` da dobra anterior — componente reutilizável.

### Fundo
- Mesma cor escura base
- Noise texture levemente mais intensa (4–5%)
- Shape decorativo canto superior direito, `opacity: 0.07`

### Cabeçalho (centralizado, acima do card)

```html
<h2 data-splitting>
  <span class="text-light">O QUE É O</span>
  <span class="text-gold">WORKSHOP</span>
</h2>
<p class="section-subtitle">
  Uma manhã de imersão densa e prática — sem teoria genérica, sem slides vazios.
  Você sai com portfólio definido, público ideal, proposta clara e estrutura
  profissional validada para entrar no mercado cobrando o que merece.
</p>
```

- Linha decorativa SVG/gradient abaixo do título (~80px, centralizada)
- `letter-spacing: 0.2em` no título

### Card principal

Container com borda gradiente (mesma técnica `::before` da dobra 2).
No desktop: grid de duas colunas internas.

**COLUNA ESQUERDA (~45%) — `tablets.webp`**

```html
<div class="media-frame media-frame--float">
  <img
    src="tablets.webp"
    alt="Profissionais em sessão de consultoria estratégica"
    loading="lazy"
    decoding="async"
    width="520"
    height="480"
    class="media-img"
  >
</div>
```

CSS:
```css
.media-frame--float {
  position: relative;
}
.media-frame--float .media-img {
  transform: rotate(-2deg);
  border-radius: 12px;
  will-change: transform;
  /* Glow dourado pulsante */
  animation: float-glow 4s ease-in-out infinite,
             glow-pulse 3s ease-in-out infinite;
}
/* Vignette overlay */
.media-frame--float::after {
  content: '';
  position: absolute;
  inset: 0;
  border-radius: 12px;
  background: radial-gradient(ellipse at center,
    transparent 50%,
    color-mix(in srgb, var(--color-bg) 40%, transparent) 100%
  );
  pointer-events: none;
}
@keyframes float-glow {
  0%, 100% { transform: translateY(0) rotate(-2deg); }
  50%       { transform: translateY(-8px) rotate(-2deg); }
}
@keyframes glow-pulse {
  0%, 100% { box-shadow: 0 0 20px color-mix(in srgb, var(--color-gold) 40%, transparent); }
  50%       { box-shadow: 0 0 40px color-mix(in srgb, var(--color-gold) 70%, transparent); }
}
/* Mobile: amplitude reduzida */
@media (max-width: 768px) {
  @keyframes float-glow {
    0%, 100% { transform: translateY(0) rotate(-1deg); }
    50%       { transform: translateY(-4px) rotate(-1deg); }
  }
}
```

Parallax no scroll:
```javascript
gsap.to(".tablets-img", {
  scrollTrigger: {
    trigger: "#o-workshop",
    start: "top bottom",
    end: "bottom top",
    scrub: 1.5
  },
  y: -40
});
```

**COLUNA DIREITA (~55%) — Lista de entregáveis**

```html
<h3 class="card-title text-gold" data-splitting>O QUE VOCÊ VAI CONSTRUIR:</h3>
<div class="deliverables-list">
  <div class="deliverable-item">
    <div class="check-icon" aria-hidden="true">
      <!-- SVG inline do checkmark -->
      <svg width="20" height="20" viewBox="0 0 20 20" fill="none"
           xmlns="http://www.w3.org/2000/svg">
        <circle cx="10" cy="10" r="9" stroke="var(--color-gold)" stroke-width="1.5"/>
        <path d="M6 10.5L8.5 13L14 7.5" stroke="var(--color-gold)"
              stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
    </div>
    <p class="deliverable-text">
      <strong class="deliverable-keyword">Diagnóstico preciso</strong>
      da sua bagagem e do que a torna única e valiosa no mercado
    </p>
  </div>
  <!-- Repetir para os demais itens -->
  <div class="deliverable-separator" aria-hidden="true"></div>
</div>
```

CSS do hover (CSS puro, sem GSAP):
```css
.deliverable-item {
  display: flex;
  gap: 1rem;
  align-items: flex-start;
  padding: 0.75rem 0;
  transition: transform 250ms ease;
  cursor: default;
}
.deliverable-item:hover { transform: translateX(6px); }
.deliverable-item:hover .check-icon { transform: scale(1.15); }
.check-icon { transition: transform 250ms ease; flex-shrink: 0; }
.deliverable-separator {
  height: 1px;
  background: currentColor;
  opacity: 0.15;
}
```

**6 itens da lista:**
1. **Diagnóstico preciso** da sua bagagem e do que a torna única e valiosa no mercado
2. **Nicho lucrativo** definido com base no seu perfil — não no que "todo mundo faz"
3. **Portfólio e método proprietário** estruturados para vender com autoridade real
4. **Precificação baseada em valor e resultado** — nunca mais cobrar por hora trabalhada
5. **Funis de vendas** validados para consultorias que fecham contratos de alto valor
6. **Modelo de negócio** para ir ao ar com previsibilidade de faturamento desde o início

### Animação de entrada
```javascript
gsap.timeline({
  scrollTrigger: {
    trigger: "#o-workshop",
    start: "top 80%",
    fastScrollEnd: true,
    preventOverlaps: true,
    once: true
  }
})
.to(".workshop-divider", { scaleX: 1, duration: 0.5 })
.from("[data-splitting='dobra3'] .word", {
  y: 40, opacity: 0, duration: 0.6, stagger: 0.1, ease: "power3.out"
}, "-=0.2")
.from(".section-subtitle", { y: 20, opacity: 0, duration: 0.6 }, "-=0.3")
.from(".workshop-card", { scale: 0.97, opacity: 0, duration: 0.5, ease: "power2.out" }, "-=0.2")
.from(".tablets-frame", { x: -30, opacity: 0, duration: 0.7 }, "-=0.3")
.from("[data-splitting='dobra3-inner'] .word", {
  opacity: 0, duration: 0.5, stagger: 0.07
}, "-=0.4")
.from(".deliverable-item", {
  x: 20, opacity: 0, duration: 0.5, stagger: 0.12, ease: "power2.out"
}, "-=0.2")
.from(".check-icon", {
  scale: 0, duration: 0.4, stagger: 0.12, ease: "elastic.out(1, 0.5)"
}, "<0.1");
```

---

## ══════════════════════════════════════════════════
## BLOCO 5 — DOBRA 4: "PARA QUEM É ESTE WORKSHOP?"
## ══════════════════════════════════════════════════

### Consistência ABSOLUTA com a dobra 3

**Esta dobra é a irmã gêmea da dobra 3.** Copie exatamente:
- Classe do container/card
- Técnica de borda gradiente (`::before`)
- Classe e SVG do checkmark
- Tamanhos de fonte e pesos tipográficos
- Separadores entre itens
- Comportamento de hover

O leitor deve perceber consistência de design, não repetição descuidada.

### Separador entre dobras 3 e 4
Mesmo `.section-divider`. Espaçamento vertical de 2.5–3rem entre os cards.

### Layout — COLUNAS INVERTIDAS em relação à dobra 3

Desktop: texto à **esquerda**, imagem à **direita**
(cria ritmo visual alternado: tablets→esquerda, caderno→direita)

**COLUNA ESQUERDA (~55%) — Texto**

```html
<h3 class="card-title text-gold" data-splitting>PARA QUEM É ESTE WORKSHOP?</h3>
<div class="deliverables-list">
  <!-- 3 itens com mesma estrutura da dobra 3 -->
</div>
```

**3 itens — tom executivo sênior, diagnóstico preciso:**

**Item 1:**
Keyword: `"Executivos e ex-executivos"`
Texto: que acumularam décadas de resultado mas ainda não traduziram isso
em um negócio próprio de consultoria — e sentem que o tempo está passando.

**Item 2:**
Keyword: `"Profissionais com bagagem comprovada"`
Texto: que dominam a entrega, têm cases reais, mas ainda não dominam
o modelo de negócio que os faz cobrar o que de fato merecem.

**Item 3:**
Keyword: `"Consultores em operação"`
Texto: que faturam, mas de forma inconsistente — e sabem que falta
método, posicionamento e estrutura para o próximo nível de receita.

**COLUNA DIREITA (~45%) — `caderno.webp`**

```html
<div class="media-frame media-frame--static">
  <img
    src="caderno.webp"
    alt="Caderno com método estruturado de consultoria estratégica"
    loading="lazy"
    decoding="async"
    width="480"
    height="520"
    class="media-img"
  >
</div>
```

CSS — SEM float animation (imagem estática transmite método e seriedade):
```css
.media-frame--static .media-img {
  border-radius: 12px;
  border-left: 1px solid color-mix(in srgb, var(--color-gold) 40%, transparent);
  will-change: transform;
}
.media-frame--static::after {
  /* mesma vignette da dobra 3 */
}
```

Parallax (mais lento que a dobra anterior — variação de ritmo):
```javascript
gsap.to(".caderno-img", {
  scrollTrigger: { scrub: 2 },
  y: -30
});
```

### Animação de entrada
```javascript
gsap.timeline({
  scrollTrigger: {
    trigger: "#para-quem",
    start: "top 80%",
    fastScrollEnd: true,
    preventOverlaps: true,
    once: true
  }
})
.to(".paraquem-divider", { scaleX: 1, duration: 0.5 })
.from("[data-splitting='dobra4'] .word", {
  y: 30, opacity: 0, duration: 0.6, stagger: 0.08, ease: "power3.out"
})
// Imagem entra da DIREITA (simétrica: tablets entraram da esquerda)
.from(".caderno-frame", { x: 30, opacity: 0, duration: 0.7 }, "-=0.4")
.from(".paraquem .deliverable-item", {
  x: 20, opacity: 0, duration: 0.5, stagger: 0.13, ease: "power2.out"
}, "-=0.3")
.from(".paraquem .check-icon", {
  scale: 0, duration: 0.4, stagger: 0.13, ease: "elastic.out(1, 0.5)"
}, "<0.1");
```

---

## ══════════════════════════════════════════════════
## BLOCO 6 — DOBRA 5: "SOBRE A MENTORA"
## ══════════════════════════════════════════════════

### Ruptura visual intencional
Esta dobra cria uma quebra de ritmo deliberada para destacar a mentora como
elemento central e aumentar credibilidade antes da dobra de conversão.

Fundo: mesma cor escura com noise mais intensa (5–6%) OU gradiente diagonal
muito sutil diferenciando do fundo das dobras anteriores.

### Elemento watermark de fundo
```html
<span class="section-watermark" aria-hidden="true">MENTORA</span>
```
```css
.section-watermark {
  position: absolute;
  font-size: clamp(6rem, 15vw, 14rem);
  font-weight: 900;
  color: var(--color-gold);
  opacity: 0.04;
  transform: rotate(-5deg);
  pointer-events: none;
  user-select: none;
  white-space: nowrap;
  left: -2%;
  top: 10%;
  z-index: 0;
}
/* Desativar no mobile */
@media (max-width: 768px) {
  .section-watermark { display: none; }
}
```

### Card container
Mesmo estilo das dobras 3 e 4. Padding interno 3rem (mais generoso).

### Layout — TEXTO À ESQUERDA (55%), FOTO À DIREITA (45%)

**COLUNA DIREITA — `mentora-palco.jpeg`**

```html
<div class="mentor-photo-frame">
  <img
    src="mentora-palco.jpeg"
    alt="Juliana Paes Garcia apresentando em palco"
    loading="lazy"
    decoding="async"
    width="480"
    height="600"
    class="mentor-photo"
  >
  <div class="mentor-photo-overlay" aria-hidden="true"></div>
</div>
```

CSS — border-radius assimétrico para romper o retângulo padrão:
```css
.mentor-photo-frame {
  position: relative;
  border-left: 3px solid var(--color-gold);
  transition: border-left-width 400ms ease;
}
.mentor-photo-frame:hover { border-left-width: 5px; }
.mentor-photo {
  border-radius: 4px 20px 4px 20px;
  width: 100%;
  height: auto;
  will-change: transform;
  transition: transform 400ms ease, box-shadow 400ms ease;
}
.mentor-photo:hover {
  transform: scale(1.02);
  box-shadow: 0 12px 48px color-mix(in srgb, var(--color-gold) 30%, transparent);
}
/* Overlay que emerge do card */
.mentor-photo-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 80px;
  background: linear-gradient(
    to bottom,
    var(--color-card-bg),
    transparent
  );
  pointer-events: none;
}
```

Entrada GSAP — a mentora "avança" em direção ao leitor:
```javascript
.from(".mentor-photo", {
  scale: 1.05, x: 20, opacity: 0, duration: 0.8, ease: "power3.out"
})
```

**COLUNA ESQUERDA — Bio de autoridade**

```html
<div class="mentor-bio">
  <p class="mentor-label text-gold" data-splitting>SOBRE A MENTORA</p>
  <h2 class="mentor-name">Juliana Paes Garcia</h2>
  <div class="mentor-name-line" aria-hidden="true"></div>

  <div class="mentor-paragraphs">
    <p>Fundadora do Devolvi Meu Crachá, Juliana Paes Garcia é hoje referência
    nacional em estruturação de consultorias de alto valor — o tipo de negócio
    que não depende de sorte, visibilidade ou networking, mas de método.</p>

    <p>Depois de mais de uma década liderando no mundo corporativo, chegou um ponto
    onde o salário alto não compensava mais o custo de ser outra pessoa todos os dias.
    Escolheu apostar na própria bagagem — e transformar esse movimento em um negócio
    real, com propósito e resultado.</p>

    <p>Desde então, já formou centenas de consultores que saíram do zero ao primeiro
    contrato de alto valor — profissionais que tinham o mesmo ponto de partida que
    você tem hoje. Este workshop é a síntese desse método.</p>
  </div>

  <!-- Micro-stats -->
  <div class="mentor-stats">
    <div class="stat-item">
      <span class="stat-number" data-target="500">0</span><!-- SUBSTITUIR PELO NÚMERO REAL -->
      <span class="stat-suffix">+</span>
      <p class="stat-label">consultores formados</p>
    </div>
    <div class="stat-divider" aria-hidden="true"></div>
    <div class="stat-item">
      <span class="stat-number" data-target="10">0</span><!-- SUBSTITUIR PELO NÚMERO REAL -->
      <span class="stat-suffix">+</span>
      <p class="stat-label">anos de experiência</p>
    </div>
    <div class="stat-divider" aria-hidden="true"></div>
    <div class="stat-item">
      <span class="stat-number stat-text">Método</span><!-- SUBSTITUIR PELO VALOR REAL -->
      <p class="stat-label">validado 2024/2025</p>
    </div>
  </div>
</div>
```

CSS dos micro-stats:
```css
.mentor-stats {
  display: flex;
  align-items: center;
  gap: 1.5rem;
  margin-top: 2rem;
  padding-top: 1.5rem;
  border-top: 1px solid color-mix(in srgb, var(--color-gold) 20%, transparent);
}
.stat-number {
  font-size: clamp(1.8rem, 3vw, 2.5rem);
  font-weight: 900;
  color: var(--color-gold);
  line-height: 1;
}
.stat-suffix { color: var(--color-gold); font-weight: 700; }
.stat-label {
  font-size: 0.75rem;
  opacity: 0.7;
  margin-top: 0.25rem;
}
.stat-divider {
  width: 1px;
  height: 2rem;
  background: var(--color-gold);
  opacity: 0.4;
}
.mentor-name-line {
  width: 50px;
  height: 2px;
  background: var(--color-gold);
  transform: scaleX(0);
  transform-origin: left;
  margin: 0.5rem 0 1.5rem;
}
```

### Animação de entrada (ScrollTrigger, trigger 15% — dispara mais cedo)
```javascript
gsap.timeline({
  scrollTrigger: {
    trigger: "#mentora",
    start: "top 85%",
    fastScrollEnd: true,
    preventOverlaps: true,
    once: true
  }
})
.from(".section-watermark", {
  opacity: 0, scale: 1.1,
  filter: "blur(4px)", duration: 1
})
.from("[data-splitting='dobra5'] .word", {
  y: 25, opacity: 0, duration: 0.5, stagger: 0.07, ease: "power3.out"
}, "-=0.6")
.from(".mentor-name", { y: 20, opacity: 0, duration: 0.5 }, "-=0.3")
.to(".mentor-name-line", { scaleX: 1, duration: 0.5 }, "-=0.2")
.from(".mentor-photo", {
  scale: 1.05, x: 20, opacity: 0, duration: 0.8, ease: "power3.out"
}, "-=0.5")
.from(".mentor-paragraphs p", {
  y: 15, opacity: 0, duration: 0.5, stagger: 0.15
}, "-=0.5")
.from(".stat-item", {
  scale: 0.9, opacity: 0, duration: 0.4, stagger: 0.15
}, "-=0.2");
```

CountUp nos stats (dentro do `onEnter` do ScrollTrigger):
```javascript
scrollTrigger: {
  onEnter: () => {
    document.querySelectorAll('.stat-number[data-target]').forEach(el => {
      new CountUp.CountUp(el, parseInt(el.dataset.target), {
        duration: 2,
        useEasing: true
      }).start();
    });
  }
}
```

---

## ══════════════════════════════════════════════════
## BLOCO 7 — DOBRA 6: PRICING / "GARANTA SEU INGRESSO"
## ══════════════════════════════════════════════════

Esta é a seção de conversão — o ápice do site. Deve ser a mais elaborada.
ScrollTrigger dispara mais cedo: `start: "top 90%"`.

### Cabeçalho da seção (centralizado, acima do card)

```html
<h2 data-splitting>GARANTA SEU INGRESSO</h2>
<p class="pricing-subtitle">Vagas limitadas · Lote atual com encerramento previsto</p>
<div class="section-line" aria-hidden="true"></div>
```

### Card principal — Dual Gradient Border + Glow

```css
.pricing-card {
  position: relative;
  border-radius: 20px;
  overflow: hidden; /* para dividir as colunas */
  /* Glow externo pulsante */
  animation: card-glow 3s ease-in-out infinite;
}
@keyframes card-glow {
  0%, 100% {
    box-shadow: 0 0 40px color-mix(in srgb, var(--color-gold) 25%, transparent);
  }
  50% {
    box-shadow: 0 0 70px color-mix(in srgb, var(--color-gold) 45%, transparent);
  }
}
/* Borda gradiente dupla */
.pricing-card::before {
  content: '';
  position: absolute;
  inset: 0;
  border-radius: inherit;
  padding: 1.5px;
  background: linear-gradient(
    135deg,
    var(--color-gold),
    transparent 30%,
    var(--color-cta) 50%,
    transparent 70%,
    var(--color-gold)
  );
  -webkit-mask: linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0);
  -webkit-mask-composite: xor;
  mask-composite: exclude;
  pointer-events: none;
  z-index: 1;
}
/* Layout duas colunas */
.pricing-inner {
  display: grid;
  grid-template-columns: 45% 55%;
}
@media (max-width: 1023px) {
  .pricing-inner { grid-template-columns: 1fr; }
}
```

### COLUNA ESQUERDA — Glass Effect

```css
.pricing-left {
  padding: 3rem 2.5rem;
  background: rgba(0, 0, 0, 0.75);
  backdrop-filter: blur(12px) saturate(150%);
  -webkit-backdrop-filter: blur(12px) saturate(150%);
  position: relative;
}
@supports not (backdrop-filter: blur(1px)) {
  .pricing-left { background: rgba(8, 8, 8, 0.97); }
}
```

HTML da coluna esquerda:
```html
<div class="pricing-left">

  <!-- [1] Branding -->
  <div class="workshop-brand">
    <p class="brand-label">WORKSHOP</p>
    <svg class="brand-icon" width="24" height="24" aria-hidden="true">
      <!-- SVG inline de pena/feather -->
    </svg>
    <p class="brand-subtitle">Criando minha</p>
    <h2 class="brand-title">Consultoria<br>Lucrativa</h2>
  </div>

  <!-- [2] Separador de pontos -->
  <div class="dots-separator" aria-hidden="true">
    <span>· · · · · · · · · · ·</span>
  </div>

  <!-- [3] Ancoragem de preço -->
  <div class="price-anchor">
    <p class="price-from">de <s>R$697,00</s> por apenas:</p>
    <div class="price-main-wrapper">
      <p class="price-main">
        R$<span class="price-number" id="price-countup">49</span>,00
      </p>
      <!-- Sparkles decorativos -->
      <span class="sparkle sparkle-1" aria-hidden="true"></span>
      <span class="sparkle sparkle-2" aria-hidden="true"></span>
      <span class="sparkle sparkle-3" aria-hidden="true"></span>
      <span class="sparkle sparkle-4" aria-hidden="true"></span>
    </div>
  </div>

  <!-- [4] CTA -->
  <a href="LINK_DE_CHECKOUT" class="btn-cta btn-glow" aria-label="Garantir meu ingresso no workshop">
    GARANTIR MEU INGRESSO
    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" aria-hidden="true">
      <path d="M5 12h14M12 5l7 7-7 7" stroke="currentColor"
            stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
    </svg>
  </a>

  <!-- [5] Detalhes do evento -->
  <div class="event-details">
    <div class="event-badge">
      <!-- SVG calendar -->
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" aria-hidden="true">
        <rect x="3" y="4" width="18" height="18" rx="2" stroke="var(--color-gold)"
              stroke-width="1.5"/>
        <path d="M16 2v4M8 2v4M3 10h18" stroke="var(--color-gold)"
              stroke-width="1.5" stroke-linecap="round"/>
      </svg>
      <span>Sábado, 28 de Março</span>
    </div>
    <div class="event-badge">
      <!-- SVG clock -->
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" aria-hidden="true">
        <circle cx="12" cy="12" r="9" stroke="var(--color-gold)" stroke-width="1.5"/>
        <path d="M12 7v5l3 3" stroke="var(--color-gold)"
              stroke-width="1.5" stroke-linecap="round"/>
      </svg>
      <span>Das 8h30 às 12h30</span>
    </div>
    <div class="event-badge">
      <!-- SVG globe -->
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" aria-hidden="true">
        <circle cx="12" cy="12" r="9" stroke="var(--color-gold)" stroke-width="1.5"/>
        <path d="M12 3c-4 3-4 15 0 18M12 3c4 3 4 15 0 18M3 12h18"
              stroke="var(--color-gold)" stroke-width="1.5"/>
      </svg>
      <span>Online e ao vivo</span>
    </div>
  </div>

</div>
```

CSS do botão CTA com glow:
```css
.btn-cta.btn-glow {
  width: 100%;
  padding: 1rem 1.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.75rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  border-radius: 8px;
  background: var(--color-cta);
  color: var(--color-cta-text);
  text-decoration: none;
  animation: cta-pulse 2s ease-in-out infinite;
  will-change: transform, box-shadow;
  transition: box-shadow 200ms ease, transform 200ms ease;
}
@keyframes cta-pulse {
  0%, 100% {
    box-shadow: 0 0 20px color-mix(in srgb, var(--color-cta) 60%, transparent),
                0 0 60px color-mix(in srgb, var(--color-cta) 30%, transparent);
    transform: scale(1);
  }
  50% {
    box-shadow: 0 0 30px color-mix(in srgb, var(--color-cta) 80%, transparent),
                0 0 80px color-mix(in srgb, var(--color-cta) 50%, transparent);
    transform: scale(1.01);
  }
}
.btn-cta.btn-glow:hover {
  animation-play-state: paused;
  box-shadow: 0 0 50px color-mix(in srgb, var(--color-cta) 90%, transparent),
              0 0 120px color-mix(in srgb, var(--color-cta) 60%, transparent);
  transform: scale(1.02);
}
```

CSS dos sparkles:
```css
.sparkle {
  position: absolute;
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: var(--color-gold);
  pointer-events: none;
}
.sparkle-1 { top: -8px; right: 20%; animation: sparkle-anim 2s ease-in-out 0s infinite; }
.sparkle-2 { top: 20%; right: -8px; animation: sparkle-anim 2s ease-in-out 0.5s infinite; }
.sparkle-3 { bottom: -8px; right: 35%; animation: sparkle-anim 2s ease-in-out 1s infinite; }
.sparkle-4 { top: 40%; left: -8px; animation: sparkle-anim 2s ease-in-out 1.5s infinite; }
@keyframes sparkle-anim {
  0%, 100% { transform: scale(0); opacity: 0; }
  50%       { transform: scale(1); opacity: 1; }
}
/* Desativar sparkles no mobile */
@media (max-width: 768px) {
  .sparkle { display: none; }
}
```

CSS dos badges de evento:
```css
.event-badge {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.6rem 1rem;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 8px;
  font-size: 0.9rem;
  color: rgba(255, 255, 255, 0.9);
}
```

### COLUNA DIREITA — Lista de entregáveis

```html
<div class="pricing-right">
  <h3 class="card-title text-gold" data-splitting>O QUE VOCÊ VAI RECEBER</h3>
  <div class="deliverables-list pricing-deliverables">

    <!-- Itens 1–4: estrutura padrão (mesma das dobras 3 e 4) -->

    <!-- Itens 5–6: BÔNUS com badge -->
    <div class="deliverable-item">
      <div class="check-icon"><!-- SVG checkmark --></div>
      <p class="deliverable-text">
        <span class="badge-bonus">BÔNUS</span>
        Guia dos 15 nichos de consultoria com maior demanda e margem em 2026 —
        filtrados por viabilidade e potencial de escala
      </p>
    </div>

    <!-- Item 7: GARANTIA com destaque especial -->
    <div class="deliverable-item deliverable-guarantee">
      <div class="check-icon check-guarantee">
        <!-- SVG de escudo -->
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none">
          <path d="M12 3L4 7v6c0 4.4 3.4 8.5 8 9.5 4.6-1 8-5.1 8-9.5V7l-8-4z"
                stroke="var(--color-cta)" stroke-width="1.5"
                stroke-linejoin="round"/>
        </svg>
      </div>
      <div>
        <span class="badge-guarantee">GARANTIA TOTAL</span>
        <p class="deliverable-text">
          Se em 7 dias você sentir que não valeu cada centavo, devolvemos
          tudo. Sem perguntas. Sem burocracia.
        </p>
      </div>
    </div>

    <!-- Badge de urgência -->
    <div class="urgency-badge">
      <svg width="14" height="14" viewBox="0 0 24 24" fill="currentColor" aria-hidden="true">
        <path d="M13 2L4.09 12.97 11 12l-2 9 8.91-10.97L11 11l2-9z"/>
      </svg>
      <span>Oferta de lote atual encerra em breve · Últimas vagas disponíveis</span>
    </div>

  </div>
</div>
```

CSS dos badges e urgência:
```css
.badge-bonus {
  display: inline-block;
  padding: 2px 8px;
  background: color-mix(in srgb, var(--color-gold) 20%, transparent);
  border: 1px solid var(--color-gold);
  border-radius: 4px;
  font-size: 0.7rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  color: var(--color-gold);
  margin-right: 0.5rem;
  vertical-align: middle;
}
.badge-guarantee {
  display: inline-block;
  padding: 2px 8px;
  background: color-mix(in srgb, var(--color-cta) 20%, transparent);
  border: 1px solid var(--color-cta);
  border-radius: 4px;
  font-size: 0.7rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  color: var(--color-cta);
  margin-bottom: 0.4rem;
}
.deliverable-guarantee {
  background: color-mix(in srgb, var(--color-cta) 5%, transparent);
  border-radius: 8px;
  padding: 0.75rem;
  border: 1px solid color-mix(in srgb, var(--color-cta) 30%, transparent);
}
.urgency-badge {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.75rem 1rem;
  background: var(--color-gold);
  color: var(--color-bg); /* texto escuro sobre fundo dourado */
  border-radius: 8px;
  font-size: 0.85rem;
  font-weight: 600;
  margin-top: 1.5rem;
  animation: urgency-pulse 2.5s ease-in-out infinite;
}
@keyframes urgency-pulse {
  0%, 100% { background: var(--color-gold); }
  50%       { background: color-mix(in srgb, var(--color-gold) 80%, white); }
}
```

**7 itens completos:**
1. Uma manhã de imersão densa e prática — sem teoria genérica, direto ao que funciona para consultorias de alto valor
2. Método passo a passo: nicho, portfólio e precificação baseada em valor — não em hora trabalhada
3. Material exclusivo de apoio para sair do workshop com sua estrutura de consultoria pronta para apresentar ao mercado
4. Acesso ao método proprietário validado por centenas de consultores que saíram do zero ao primeiro contrato de alto valor
5. **[BÔNUS]** Guia dos 15 nichos de consultoria com maior demanda e margem em 2026 — filtrados por viabilidade e potencial de escala
6. **[BÔNUS]** Aula exclusiva sobre os bastidores do mercado — o que ninguém conta sobre como clientes reais tomam decisões de contratar
7. **[GARANTIA]** Se em 7 dias você sentir que não valeu cada centavo, devolvemos tudo. Sem perguntas. Sem burocracia.

### CountUp — preço animado (697 → 49)
```javascript
scrollTrigger: {
  trigger: "#pricing",
  start: "top 90%",
  once: true,
  onEnter: () => {
    // Preço desce de 697 até 49 — efeito dramático de desconto
    const priceEl = document.getElementById('price-countup');
    new CountUp.CountUp(priceEl, 49, {
      startVal: 697,
      duration: 2,
      useEasing: true,
      easingFn: (t, b, c, d) => c * (1 - Math.pow(2, -10 * t/d)) + b
    }).start();
    // Micro-stats da dobra 5 (se ainda não disparou)
    document.querySelectorAll('.stat-number[data-target]').forEach(el => {
      new CountUp.CountUp(el, parseInt(el.dataset.target), {
        duration: 2, useEasing: true
      }).start();
    });
  }
}
```

### CTA Sticky no Mobile
```javascript
// Ativar botão sticky bottom no mobile enquanto a seção de pricing está no viewport
let mm = gsap.matchMedia();
mm.add("(max-width: 768px)", () => {
  ScrollTrigger.create({
    trigger: "#pricing",
    start: "top center",
    end: "bottom top",
    onEnter: () => document.querySelector('.sticky-cta-mobile')?.classList.add('active'),
    onLeave: () => document.querySelector('.sticky-cta-mobile')?.classList.remove('active'),
    onEnterBack: () => document.querySelector('.sticky-cta-mobile')?.classList.add('active'),
    onLeaveBack: () => document.querySelector('.sticky-cta-mobile')?.classList.remove('active')
  });
});
```

HTML do sticky (adicionar antes do `</body>`):
```html
<div class="sticky-cta-mobile" aria-hidden="true">
  <a href="LINK_DE_CHECKOUT" class="btn-cta btn-glow">
    GARANTIR MEU INGRESSO
  </a>
</div>
```

CSS:
```css
.sticky-cta-mobile {
  display: none;
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 1rem;
  background: color-mix(in srgb, var(--color-bg) 95%, transparent);
  backdrop-filter: blur(8px);
  z-index: 1000;
  transform: translateY(100%);
  transition: transform 300ms ease;
}
@media (max-width: 768px) {
  .sticky-cta-mobile { display: block; }
  .sticky-cta-mobile.active { transform: translateY(0); }
}
```

### Animação de entrada completa (ScrollTrigger, trigger 10%)
```javascript
gsap.timeline({
  scrollTrigger: {
    trigger: "#pricing",
    start: "top 90%",
    fastScrollEnd: true,
    preventOverlaps: true,
    once: true
  }
})
.from("[data-splitting='dobra6'] .word", {
  y: 30, opacity: 0, duration: 0.6, stagger: 0.08, ease: "power3.out"
})
.from(".pricing-subtitle", { opacity: 0, duration: 0.4 }, "-=0.2")
.to(".pricing-line", { scaleX: 1, duration: 0.5 }, "-=0.2")
.from(".pricing-card", { scale: 0.95, opacity: 0, duration: 0.6, ease: "power3.out" }, "-=0.2")
// Coluna esquerda
.from(".brand-label", { y: 15, opacity: 0, duration: 0.4 }, "-=0.2")
.from(".brand-subtitle", { y: 15, opacity: 0, duration: 0.4 }, "-=0.25")
.from(".brand-title", { y: 15, opacity: 0, duration: 0.4 }, "-=0.25")
.from(".dots-separator", { opacity: 0, duration: 0.3 }, "-=0.1")
.from(".price-from", { x: -10, opacity: 0, duration: 0.3 })
// CountUp dispara via onEnter
.from(".price-main", { scale: 0.8, opacity: 0, duration: 0.5, ease: "elastic.out(1, 0.5)" })
.from(".btn-cta", { scale: 0.9, opacity: 0, duration: 0.5, ease: "elastic.out(1, 0.5)" })
.from(".event-badge", { y: 10, opacity: 0, duration: 0.4, stagger: 0.1 })
// Coluna direita
.from("[data-splitting='dobra6-inner'] .word", {
  opacity: 0, duration: 0.4, stagger: 0.06
}, "<0.3")
.from(".pricing-deliverables .deliverable-item", {
  x: 15, opacity: 0, duration: 0.4, stagger: 0.08, ease: "power2.out"
}, "-=0.2")
.from(".pricing-deliverables .check-icon", {
  scale: 0, duration: 0.3, stagger: 0.08, ease: "elastic.out(1, 0.5)"
}, "<0.08")
.from(".urgency-badge", { scale: 0.95, opacity: 0, duration: 0.4 });
```

---

## ══════════════════════════════════════════════════
## BLOCO 8 — CHECKLIST FINAL DE QUALIDADE
## ══════════════════════════════════════════════════

Antes de considerar a implementação concluída, verifique ITEM A ITEM:

### Performance
- [ ] Todos os scripts CDN têm `defer`
- [ ] `<link rel="preload">` na imagem da hero
- [ ] `<link rel="preconnect">` para Google Fonts e CDNs
- [ ] Google Fonts com `&display=swap`
- [ ] CSS crítico inline no `<head>` (hero only)
- [ ] Font Awesome substituído por SVG inline (nenhum CDN do FA)
- [ ] Todas as imagens das dobras 2–6 com `loading="lazy"` e `decoding="async"`
- [ ] Todas as imagens com `width` e `height` explícitos
- [ ] `@media (prefers-reduced-motion: reduce)` declarado
- [ ] `@supports not (backdrop-filter)` com fallback
- [ ] Sparkles desativados em mobile com `display: none`
- [ ] Animações GSAP usando apenas `transform` e `opacity`
- [ ] `will-change: transform, opacity` nos elementos animados
- [ ] Nenhum `console.log()` no código

### Consistência visual
- [ ] Todas as cores via `var()` — zero valores hard-coded
- [ ] design_system.html consultado antes de qualquer decisão CSS
- [ ] Mesma técnica `::before` de borda gradiente nas dobras 3, 4, 5 e 6
- [ ] Mesmo SVG de checkmark nas dobras 3, 4 e 6
- [ ] Mesmos separadores de 1px entre itens
- [ ] Mesmo comportamento de hover (`translateX(6px)` + `scale(1.15)`)
- [ ] Dobras 3 e 4 visualmente idênticas na estrutura (irmãs gêmeas)
- [ ] Ritmo alternado confirmado: imagem esquerda (D3) / direita (D4) / direita (D5)

### Animações
- [ ] Splitting.js inicializado uma única vez com `[data-splitting]`
- [ ] Lenis + ScrollTrigger dentro de `window.addEventListener('load')`
- [ ] Nenhuma animação dispara sem scroll
- [ ] ScrollTrigger com `fastScrollEnd: true` e `preventOverlaps: true` em todas as dobras
- [ ] CountUp instanciado apenas no `onEnter` do ScrollTrigger da dobra 6
- [ ] Float animation ativa na imagem dos tablets (dobra 3)
- [ ] SEM float animation na imagem do caderno (dobra 4)
- [ ] Glow pulse loop ativo no card de pricing (dobra 6)
- [ ] CTA sticky mobile funciona via `toggleClass` do ScrollTrigger

### Conteúdo
- [ ] `<!-- SUBSTITUIR PELO NÚMERO REAL -->` nos 3 micro-stats da dobra 5
- [ ] `href="LINK_DE_CHECKOUT"` marcado para substituição nos dois botões CTA
- [ ] `alt` descritivo e semântico em todas as imagens
- [ ] `aria-hidden="true"` em todos os elementos puramente decorativos
- [ ] `aria-label` descritivo nos links/botões sem texto suficiente

---

*Mega-prompt definitivo — versão com performance integrada.*
*Forneça junto: index.html (hero pronta) + design_system.html + todas as imagens.*
*Implementar dobra por dobra, validando no browser após cada uma.*
