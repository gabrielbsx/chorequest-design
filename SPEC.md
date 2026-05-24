# ChoreQuest · Design Spec (Handoff para o time)

> 🌐 **Live:** https://gabrielbsx.github.io/chorequest-design/
> 📦 **Repo:** https://github.com/gabrielbsx/chorequest-design
>
> Esse documento complementa o protótipo com detalhes de implementação, tokens e critérios de aceitação por tela.

---

## 1. Visão geral

**ChoreQuest** é uma plataforma gamificada de tarefas domésticas para famílias.

- **Usuários:**
  - **Parent** (administrador) — cria tasks, aprova entregas, gerencia recompensas e família
  - **Child** (jogador) — recebe tasks, submete entregas, ganha XP, resgata recompensas
- **Idade-alvo das crianças:** 6-17 anos
- **Plataforma:** web (mobile-first) + futuramente PWA
- **Stack:** React (Vite) + Tailwind + shadcn no frontend. NestJS + Prisma + Postgres no backend.

---

## 2. Direção visual — "Cozy Quest"

| Atributo      | Decisão                                                |
| ------------- | ------------------------------------------------------ |
| Tom           | Warm, familiar, playful sem ser infantil               |
| Inspiração    | Cal/Linear/Raycast (cleanness) + Duolingo (vivacidade) |
| Personalidade | Jogo de RPG cozy + revista premium                     |
| Densidade     | Generosa (whitespace, padding 16-32px)                 |
| Atmosfera     | Grain sutil no background, gradientes warm radiais     |

### 2.1 Tokens de cor

```css
--bg: #fff8f0 /* cream background */ --bg-2: #ffefd8 /* warm tint */
  --ink: #1a1410 /* deep brown text */ --ink-2: #5c4a3e /* secondary text */
  --line: #e8d5b7 /* borders */ --paper: #fffdf8 /* cards */ --sunset: #ff6b47
  /* primary action */ --honey: #ffb627 /* warning/highlight */ --leaf: #7fb069
  /* success/done */ --berry: #e94560 /* danger/review */ --sky: #4f86c6
  /* info */ --plum: #6b3f73 /* parent role */;
```

**Dark mode** (`.dark` no `<html>`):

```css
--bg: #14110f --bg-2: #1f1a16 --ink: #fff1dd --ink-2: #c9b89e --line: #2e251d
  --paper: #1a1612;
```

### 2.2 Tipografia

| Família                           | Uso                                   | Tamanhos |
| --------------------------------- | ------------------------------------- | -------- |
| **Bricolage Grotesque** (display) | Headings, hero numbers, stats grandes | 18-96px  |
| **Manrope** (body)                | Conteúdo geral, forms                 | 12-18px  |
| **DM Mono** (mono)                | XP/level/codes/stats inline           | 10-32px  |

Pesos: Display 700+, Body 400-700, Mono 400-500.

### 2.3 Spacing & radii

- Spacing scale: `4 · 8 · 12 · 16 · 20 · 24 · 32 · 48 · 64 · 96` px
- Border radius: `8 · 14 · 22 · 32 · full(9999)`
- Cards: `radius 22px` + shadow stack (inner highlight + outer drop + ambient)

### 2.4 Motion

- Page-load: stagger rise (`opacity 0 → 1` + `translateY 12px → 0`) com delays 50ms/120ms/200ms/280ms/360ms
- XP fill: `cubic-bezier(.4, 1.4, .4, 1)` em 1.4s (overshoot)
- Flame: flicker infinito 1.2s
- Hover lift: -3px translate em cards
- Confetti em level-up (CSS animation `burst`)

---

## 3. Componentes

### 3.1 UI primitives

| Componente               | Variants                             | Props                     |
| ------------------------ | ------------------------------------ | ------------------------- |
| `<Button>`               | primary, ghost, approve, danger, ink | size (sm/md/lg), disabled |
| `<Card>`                 | soft (padrão)                        | padding inherits          |
| `<Input>` / `<Textarea>` | base                                 | placeholder, type         |
| `<Label>`                | eyebrow style                        | children                  |

### 3.2 Domain components

| Componente        | Função                                                                |
| ----------------- | --------------------------------------------------------------------- |
| `<Avatar>`        | Initial + tone (gradient bg), tamanhos sm/md/lg/xl                    |
| `<XpProgressBar>` | XP atual + level + barra animada                                      |
| `<StreakFlame>`   | SVG chama evolutiva (0d→gray, 1-6d→amarela, 7-29d→laranja, 30d+→azul) |
| `<BadgeChip>`     | Chip com emoji + label + tone                                         |
| `<TaskCard>`      | Card com status, título, assignee, dificuldade, XP                    |
| `<ApprovalCard>`  | Task pendente + nota opcional + approve/reject inline                 |
| `<RewardCard>`    | Reward com blob decorativo, afford/locked state                       |
| `<RankRow>`       | Linha do leaderboard com avatar + você highlight                      |
| `<MemberCard>`    | Card de membro da família com role pill                               |
| `<ActivityRow>`   | Item de feed de atividades                                            |
| `<FlowStep>`      | Etapa numerada em diagramas de fluxo                                  |
| `<BadgeFull>`     | Badge card grande com unlocked/locked state + progresso               |
| `<XpEvent>`       | Linha do XP history com type (approve/streak/redeem/bonus)            |
| `<Heatmap>`       | Grid 28 dias contribution-style (parent dashboard)                    |

---

## 4. Inventário de telas (33 telas no protótipo)

| #   | Tela                                  | Persona | Status | Notas                                                               |
| --- | ------------------------------------- | ------- | ------ | ------------------------------------------------------------------- |
| 1   | Intro/landing                         | público | ✅     | Pode virar landing externa                                          |
| 2   | Design System                         | dev     | ✅     | Doc viva — referencia tokens                                        |
| 3   | Login                                 | ambos   | ✅     | Side card decorativo mostra valor                                   |
| 4   | Registro família                      | parent  | ✅     | Wizard 3 passos                                                     |
| 5   | Onboarding                            | parent  | ✅     | Step 2 (nome+emoji+categorias)                                      |
| 6   | Dashboard child                       | child   | ✅     | Hero gamificado                                                     |
| 7   | Dashboard parent                      | parent  | ✅     | Métricas + approval queue + heatmap                                 |
| 8   | Tasks list                            | ambos   | ✅     | Agrupado por status + filtros                                       |
| 9   | Create Task modal                     | parent  | ✅     | Form com keyboard shortcuts                                         |
| 10  | Approval Queue                        | parent  | ✅     | Bulk + comentários                                                  |
| 11  | Rewards Shop                          | child   | ✅     | Catálogo com afford state                                           |
| 12  | Ranking                               | ambos   | ✅     | Pódio + lista completa                                              |
| 13  | Família                               | parent  | ✅     | Membros + convites                                                  |
| 14  | Level Up celebration                  | child   | ✅     | Modal com confetti                                                  |
| 15  | Notifications drawer                  | ambos   | ✅     | Painel lateral + settings                                           |
| 16  | Settings                              | ambos   | ✅     | Perfil, aparência, sons                                             |
| 17  | Estados (empty/loading/error/success) | dev     | ✅     | Para react-query QueryWrapper                                       |
| 18  | Mobile mockups                        | ambos   | ✅     | 6 phones (Home/Tasks/Loja/Approval/Ranking/Profile) + bottom sheets |
| 19  | User flows                            | dev/PO  | ✅     | 4 fluxos críticos diagramados                                       |
| 20  | Task Detail (drill-down)              | ambos   | ✅     | Histórico, comments, submissão                                      |
| 21  | Badge Gallery                         | child   | ✅     | 22 badges (8 unlocked, 14 locked)                                   |
| 22  | XP History timeline                   | ambos   | ✅     | Gráfico SVG + event log audit                                       |
| 23  | Invite acceptance (child)             | child   | ✅     | Magic link → join family                                            |
| 24  | Forgot password / reset               | ambos   | ✅     | 3 estados: form, sent, reset                                        |
| 25  | Email verification                    | ambos   | ✅     | Checklist + reenvio com cooldown                                    |
| 26  | Profile público                       | ambos   | ✅     | View other user + parent admin                                      |
| 27  | Reward pós-resgate                    | ambos   | ✅     | Status timeline + confirm delivery                                  |
| 28  | Weekly report (print-friendly)        | parent  | ✅     | Imprimível com stats e insights                                     |
| 29  | Família vazia (empty state)           | parent  | ✅     | Onboarding pós-signup, invite focus                                 |
| 30  | Streak em risco (modal urgente)       | child   | ✅     | Lembrete 20h + tasks rápidas                                        |
| 31  | Confirmação destrutiva (2 variants)   | ambos   | ✅     | Médio + crítico (type-to-confirm)                                   |
| 32  | 404 Not Found                         | ambos   | ✅     | Atalhos pras seções principais                                      |
| 33  | Maintenance / offline                 | ambos   | ✅     | Progress bar + changelog                                            |

---

## 5. Critérios de aceitação por tela (resumido)

### Dashboard (child)

- [ ] Hero mostra nome + saudação contextual (manhã/tarde/noite)
- [ ] Card de XP com `<XpProgressBar>` animada
- [ ] Card de streak com `<StreakFlame>` proporcional
- [ ] Card de tasks hoje (count + status pills)
- [ ] Side card "Próxima recompensa" com botão direto
- [ ] Grid de 3 tasks pendentes
- [ ] Lista de atividades recentes (4 últimas)
- [ ] Mini ranking família (top 5)
- [ ] Responsivo: stack vertical < 768px

### Tasks list

- [ ] Filtros chips por status com counts
- [ ] Busca inline (debounce 300ms)
- [ ] Agrupamento por status
- [ ] Empty state amigável quando filtro vazio
- [ ] Botão "+ Nova task" só visível para parent
- [ ] Mobile: scroll horizontal nos filtros, single column grid

### Approval Queue (parent)

- [ ] Header com count "X tarefas aguardando você"
- [ ] Botão "Aprovar todas" só ativa quando ≥ 1 selecionada
- [ ] ApprovalCard com checkbox pra bulk
- [ ] Nota da criança em destaque (se houver)
- [ ] Approve = transação atomic (status + XP)
- [ ] Rejeição abre modal de razão

### Rewards Shop

- [ ] Card mostra estoque (∞ ou número)
- [ ] Botão "Resgatar" desabilita se XP < cost
- [ ] Confirm dialog antes de gastar
- [ ] Toast pós-resgate
- [ ] Saldo XP visível no header

### Level Up celebration

- [ ] Trigger: ApproveTask que cruza threshold de level
- [ ] Backdrop blur (12px) + confetti
- [ ] Mostra: novo level, nova badge, próximo perk
- [ ] Som opcional (toggle em settings)
- [ ] CTA "Continuar quest" volta ao dashboard

### Onboarding

- [ ] 3 steps obrigatórios (parent, família, primeira task)
- [ ] Step 3 sugere tasks por categoria escolhida
- [ ] Skip permitido apenas no step 3 (com aviso)
- [ ] Progress indicator visível em todos steps

---

## 6. Fluxos críticos (cobertura ~90% dos casos)

### F1 · Child completa tarefa

`Dashboard → Aceita task → In Progress → Submete → Aguarda → Aprovado + XP credita 🎉`

### F2 · Parent aprova

`Notificação → Approval Queue → Lê nota → Approve/Reject → XP transação atomic`

### F3 · Resgate de recompensa

`Loja → Escolhe → Confirma → XP debita + estoque -1 → Parent vê pendente`

### F4 · Onboarding (família nova)

`Sign up → Nomeia família → Primeira task demo → Convida child → Child ganha 10 XP boas-vindas`

---

## 7. Estados padrão (todos as listas)

| Estado  | Componente       | Trigger                       |
| ------- | ---------------- | ----------------------------- |
| Loading | Skeleton shimmer | `isFetching` true e sem dados |
| Empty   | Ilustração + CTA | `data.length === 0`           |
| Error   | Card com retry   | `isError` true                |
| Success | Toast (sonner)   | Mutation success              |

---

## 8. Considerações de acessibilidade

- Contraste mínimo AA (4.5:1) em todos textos
- `aria-label` em botões só com ícone
- Focus ring custom: 4px outline sunset @ 0.15 opacity
- Keyboard nav: Tab pelos cards, Enter aciona ação principal
- `prefers-reduced-motion`: desativar animações de chama e XP fill
- Tamanho mínimo de touch: 44×44px (botões mobile)

---

## 9. Notas de implementação

### Frontend (apps/web)

- **Routing:** react-router-dom v6, rotas mapeadas em `src/App.tsx`
- **Data fetching:** `@tanstack/react-query` com `<QueryWrapper>` global
- **Forms:** `react-hook-form` + `zod` schemas em `packages/shared`
- **State global:** mínimo (`useAuth` context só pra sessão)
- **Tema:** CSS custom props em `:root` + `.dark`
- **Animações:** CSS only (preferido). Framer Motion só se precisar physics

### Backend (apps/api)

- ApproveTask **sempre** em transação com GrantXP (atomic)
- Status transitions validadas no domain (`canTransitionTo()`)
- Notification on submit/approve/reject → tabela `notifications` + websocket (stretch)
- Idempotency em redeem: chave `(userId, rewardId, requestId)`

---

## 10. Arquivos de referência

| Arquivo             | Conteúdo                                          |
| ------------------- | ------------------------------------------------- |
| `design/index.html` | Protótipo navegável (19 telas) — abrir no browser |
| `design/SPEC.md`    | Este documento                                    |

### Como compartilhar com o time

1. **Local:** `xdg-open design/index.html` e mostrar em call
2. **GitHub Pages:** push `design/` numa branch `gh-pages` → link público
3. **Vercel:** `vercel design/` → preview URL em < 30s
4. **Print/PDF:** Chrome → Imprimir → Salvar como PDF (cobre todas telas)

---

## 11. Próximos passos sugeridos

1. **Review do time** — 30min walking through cada tela
2. **Ajustes** — coletar feedback, iterar 1-2 vezes
3. **Sprint Planning S1** — usar Monday board (96 cards) pra alocar implementação
4. **Pair programming inaugural** — primeiro componente: `<TaskCard>` ou `<XpProgressBar>`
5. **Workshop "Design Tokens 101"** — 30min explicando como ler design tokens e replicar no Tailwind

---

## 12. Glossário do domínio

| Termo          | Definição                                                  |
| -------------- | ---------------------------------------------------------- |
| **XP**         | Experience Points · ganho ao completar tasks               |
| **Level**      | Nível derivado de XP: `floor(sqrt(xp/100))`                |
| **Streak**     | Dias consecutivos com ao menos 1 task aprovada             |
| **Family**     | Aggregate root contendo Users (parents + children)         |
| **Quest**      | Sinônimo de Task no marketing/UI (não no código)           |
| **Approval**   | Ato do parent validar que child fez a task                 |
| **Redeem**     | Trocar XP por recompensa real                              |
| **Multiplier** | Bônus de XP por dificuldade: easy ×1, medium ×1.5, hard ×2 |
| **Badge**      | Achievement desbloqueado em milestone                      |

---

**Versão:** 1.0 · **Data:** 2026-05-24 · **Status:** Pronto pra review do time
