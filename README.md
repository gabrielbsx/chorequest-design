# ChoreQuest · Design Prototype

> 🌐 **Web preview:** https://gabrielbsx.github.io/chorequest-design/
> 📱 **Mobile preview:** https://gabrielbsx.github.io/chorequest-design/mobile.html

Protótipo navegável de **ChoreQuest** — plataforma gamificada de tarefas domésticas para famílias.

## 📁 Dois protótipos

| Arquivo       | Pra que serve                                                | Como abrir                                                 |
| ------------- | ------------------------------------------------------------ | ---------------------------------------------------------- |
| `index.html`  | Visão completa pra desktop · 33 telas + design system + spec | Desktop browser                                            |
| `mobile.html` | App mobile fullscreen interativo · 5 abas + modais + toasts  | **Abra no celular** ou simulador (iPhone frame em desktop) |

## 📦 O que tem aqui

- **`index.html`** — Protótipo navegável de 33 telas com sistema visual completo
- **`SPEC.md`** — Doc de handoff: tokens, componentes, critérios de aceitação, user flows

## 🎨 Direção visual

**"Cozy Quest"** — warm gamified family hub. Tipografia distintiva (Bricolage Grotesque + Manrope + DM Mono), paleta sunset (Sunset/Honey/Leaf/Berry/Sky/Plum), chunky rounded shapes (radius 8/14/22/32), motion suave com spring overshoot.

## 🗂️ 33 telas

### Auth (5)

Login · Register família · Onboarding · Forgot password · Email verify · Invite acceptance (child)

### Core (8)

Dashboard child · Dashboard parent · Tasks list · Task Detail · Create Task modal · Approval Queue · Rewards Shop · Ranking

### Gamificação (4)

Level Up celebration · Badge Gallery · XP History · Streak em risco

### Família + Profile (4)

Família · Família vazia · Profile público · Settings

### Sistema (12)

Intro · Design System · Notifications drawer · Estados (empty/loading/error/success) · Mobile mockups (6 phones + bottom sheets) · User Flows · Weekly Report · Reward pós-resgate · Confirmação destrutiva · 404 · Maintenance

## 🛠️ Stack futuro

- **Frontend:** React (Vite) + Tailwind + shadcn
- **Backend:** NestJS + Prisma + Postgres
- **Auth:** JWT via Passport Strategy
- **Deploy:** Vercel (web) + Railway (api)
- **Arquitetura:** Clean Architecture sobre NestJS — domain/application puros

## 👥 Como usar com o time

1. **Walkthrough em call** — abrir https://gabrielbsx.github.io/chorequest-design/ e navegar (~30min)
2. **Coletar feedback** — issues no Monday board ChoreQuest
3. **Sprint Planning** — usar o board Monday (96 cards) pra alocar implementação
4. **Pair programming inaugural** — começar por `<StreakFlame>` ou `<XpProgressBar>` (alto valor pedagógico, baixa complexidade)

## 📋 Veja também

- [SPEC.md](./SPEC.md) — Doc de handoff completo
- Monday board (privado): ChoreQuest 🏠✨ — Família Gamificada
