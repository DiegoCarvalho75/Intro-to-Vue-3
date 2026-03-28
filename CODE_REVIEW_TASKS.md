# Revisão da base de código (Intro-to-Vue-3)

## Resumo rápido dos problemas encontrados

1. **Erro de digitação/consistência de naming:** classe CSS `disabledButton` usa padrão camelCase, enquanto o restante usa kebab-case.
2. **Bug funcional:** `main.js` só declara `const product = 'Socks'` e nunca monta uma aplicação Vue, então a UI não usa estado reativo nem renderização dinâmica.
3. **Discrepância de comentário/documentação:** comentário em `index.html` diz `Import Js`, enquanto o arquivo efetivamente importa JavaScript (`main.js`), sem consistência de capitalização e terminologia com outros comentários.
4. **Teste inexistente para comportamento essencial:** não há nenhum teste automatizado para validar carregamento básico da aplicação e exibição do título/produto.

---

## Tarefa 1 — Corrigir erro de digitação/consistência

**Título sugerido:** Padronizar nome da classe CSS `disabledButton` para `disabled-button`

**Problema:**
`assets/styles.css` usa `.disabledButton`, destoando do padrão predominante de classes em kebab-case (`.nav-bar`, `.product-display`, `.out-of-stock-img`, etc.).

**Ação:**
- Renomear `.disabledButton` para `.disabled-button` no CSS.
- Atualizar todos os pontos de uso no HTML/templates Vue (atuais e futuros exemplos do curso).

**Critério de aceite:**
- Não existir mais referência a `disabledButton` no projeto.
- Elementos desabilitados manterem exatamente o mesmo visual.

---

## Tarefa 2 — Corrigir bug funcional

**Título sugerido:** Inicializar corretamente app Vue em `main.js`

**Problema:**
A página importa Vue 3 e `main.js`, mas o script não cria/monta aplicação. Isso impede evolução da base para o comportamento esperado do curso (renderização dinâmica em `#app`).

**Ação:**
- Substituir código atual por uma inicialização mínima:
  - `const app = Vue.createApp({ data() { return { product: 'Socks' } } })`
  - `app.mount('#app')`
- Atualizar `index.html` para usar binding (`{{ product }}`) no lugar de texto estático.

**Critério de aceite:**
- O título do produto renderiza via Vue (não hardcoded).
- Alterações no estado refletirem no DOM.

---

## Tarefa 3 — Ajustar comentário/documentação

**Título sugerido:** Revisar comentários de importação no `index.html` para nomenclatura consistente

**Problema:**
Comentário `<!-- Import Js -->` é inconsistente com `<!-- Import Vue.js -->` e com estilo recomendado (`JS`/`JavaScript`).

**Ação:**
- Trocar para um padrão único e claro, por exemplo:
  - `<!-- Import application script -->` ou
  - `<!-- Import JavaScript -->`
- Revisar demais comentários do arquivo para consistência de linguagem (idealmente tudo em inglês, já que `lang="en"`).

**Critério de aceite:**
- Todos os comentários de `index.html` seguem o mesmo padrão terminológico.
- Sem abreviações ambíguas.

---

## Tarefa 4 — Melhorar teste automatizado

**Título sugerido:** Adicionar teste smoke de renderização inicial da aplicação

**Problema:**
Não existe cobertura de teste para o fluxo mais básico: carregar a página e validar conteúdo principal.

**Ação:**
- Configurar teste simples de front-end (ex.: Vitest + jsdom, ou Playwright para E2E).
- Criar teste que valide ao menos:
  - presença de `#app`;
  - renderização do texto do produto;
  - ausência de erro de execução no carregamento inicial.

**Critério de aceite:**
- Novo teste executa em CI/local com comando único (`npm test` ou equivalente).
- Falha quando renderização inicial é quebrada.

---

## Priorização sugerida

1. **Bug funcional (`main.js`)** — impacto imediato no comportamento.
2. **Teste smoke** — evita regressão do bug após correção.
3. **Padronização de classe CSS** — manutenção e legibilidade.
4. **Comentários/documentação** — consistência e onboarding.
