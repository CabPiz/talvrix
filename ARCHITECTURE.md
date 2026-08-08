# Talvrix — Architecture Document

**Produto:** Talvrix  
**Empresa:** Kairos Labs  
**Versão:** 1.1 — Zero-cost MVP  
**Data:** 2026-08-07  

---

## 1. Princípio Arquitetural do MVP

> **Custo zero de infraestrutura até o break-even.**  
> O MVP usa exclusivamente tiers gratuitos. A receita das assinaturas cobre os custos de IA e financia a escalabilidade a partir da Fase 2.

---

## 2. Stack Tecnológica — MVP Zero-Cost

| Camada | Tecnologia | Custo | Limite gratuito |
|---|---|---|---|
| Frontend + Backend | Next.js 14 + TypeScript | **R$ 0** | Vercel Free: 100GB bandwidth/mês |
| API Routes | Next.js API Routes | **R$ 0** | 100k invocações/mês |
| Auth | Supabase Auth | **R$ 0** | 50.000 usuários ativos |
| Banco de dados | Supabase PostgreSQL | **R$ 0** | 500MB storage |
| Scraping | Playwright via API Route | **R$ 0** | Executa no servidor Vercel |
| IA / Matching | Claude API (Anthropic) | **R$ 0\*** | Pago apenas por usuários pagantes |
| PDF parsing | pdf-parse (npm) | **R$ 0** | Biblioteca local, sem API externa |
| Deploy | Vercel | **R$ 0** | Tier gratuito permanente |

> \* **Estratégia de custo zero com IA:** usuários do plano Free não recebem matching por IA — apenas listagem de vagas por salário. O matching semântico por IA é exclusivo dos planos pagos. A receita da assinatura (R$29+/mês) cobre o custo médio de ~R$0,50 por busca com IA. Margem garantida desde o primeiro assinante.

---

## 3. Por que Next.js full-stack em vez de FastAPI separado?

No MVP, eliminar o backend Python elimina o custo de servidor (Railway ~R$100/mês).  
As API Routes do Next.js no Vercel são suficientes para:
- Receber upload de PDF e parsear com `pdf-parse`
- Fazer scraping de sites estáticos com `axios` + `cheerio`
- Chamar a Claude API para matching
- Gerenciar sessão via Supabase

**FastAPI + Playwright** entra na Fase 2, quando a receita justificar o servidor dedicado e o volume de scraping exigir sites JavaScript-heavy com Chromium.

---

## 4. Diagrama de Componentes (MVP)

```
┌─────────────────────────────────────────┐
│            USUÁRIO (Browser)            │
└──────────────────┬──────────────────────┘
                   │ HTTPS
┌──────────────────▼──────────────────────┐
│         Vercel — Next.js 14             │
│                                         │
│  ┌────────────────────────────────────┐ │
│  │           App Router               │ │
│  │  /dashboard  /pricing  /results    │ │
│  └────────────────────────────────────┘ │
│                                         │
│  ┌────────────────────────────────────┐ │
│  │         API Routes (/api)          │ │
│  │                                    │ │
│  │  /upload   → pdf-parse             │ │
│  │  /search   → axios + cheerio       │ │
│  │  /match    → Claude API            │ │
│  │  /webhook  → Stripe                │ │
│  └────────────────────────────────────┘ │
└──────────┬──────────────────────────────┘
           │
    ┌──────┴──────────────────┐
    │                         │
┌───▼────────┐      ┌────────▼────────┐
│  Supabase  │      │   Claude API    │
│ Auth + DB  │      │  (só pagantes)  │
└────────────┘      └─────────────────┘
    │
┌───▼────────┐
│   Stripe   │
│ (webhooks) │
└────────────┘
```

---

## 5. Estrutura de Diretórios

```
talvrix/
├── app/
│   ├── (auth)/
│   │   ├── login/
│   │   └── register/
│   ├── dashboard/
│   │   ├── upload/         # Upload do currículo
│   │   ├── search/         # Configurar busca
│   │   └── results/        # Vagas ranqueadas
│   ├── pricing/            # Planos e checkout
│   └── api/
│       ├── upload/         # POST: recebe PDF → extrai texto
│       ├── search/         # POST: scraping do site
│       ├── match/          # POST: Claude API matching
│       └── webhooks/
│           └── stripe/     # Eventos de pagamento
├── components/
│   ├── ui/                 # Shadcn/UI
│   ├── resume-upload/
│   ├── job-card/
│   └── results-list/
├── lib/
│   ├── supabase.ts
│   ├── stripe.ts
│   ├── claude.ts           # Wrapper Claude API
│   ├── pdf-parser.ts       # pdf-parse wrapper
│   └── scraper.ts          # axios + cheerio
├── LICENSE
├── PRD.md
├── ARCHITECTURE.md
├── BUSINESS_PLAN.md
└── .env.example
```

---

## 6. Fluxo de Dados Principal

```
1. POST /api/upload
   → Recebe PDF (multipart/form-data)
   → pdf-parse extrai texto bruto
   → Salva texto no Supabase (tabela resumes)
   → Retorna resume_id

2. POST /api/search
   → Recebe { resume_id, site_url }
   → Verifica plano do usuário no Supabase
   → axios busca HTML do site de vagas
   → cheerio extrai: título, salário, link, requisitos
   → Salva vagas brutas no Supabase (tabela jobs)
   → Retorna lista de vagas ordenada por salário

3. POST /api/match  (somente planos pagos)
   → Recebe { resume_id, job_ids[] }
   → Busca texto do currículo + vagas no Supabase
   → Envia para Claude API → score 0-100 por vaga
   → Atualiza tabela jobs com compatibility_score
   → Retorna vagas reordenadas por score + salário
```

---

## 7. Modelo de Dados (Supabase PostgreSQL)

```sql
users        (id, email, plan, stripe_customer_id, created_at)
resumes      (id, user_id, raw_text, created_at)
searches     (id, user_id, site_url, status, created_at)
jobs         (id, search_id, title, salary_raw, salary_parsed,
              url, requirements, compatibility_score, created_at)
```

---

## 8. Variáveis de Ambiente

```env
# Supabase
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=

# Stripe
STRIPE_SECRET_KEY=
STRIPE_WEBHOOK_SECRET=
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=

# Claude API (Anthropic)
ANTHROPIC_API_KEY=
```

---

## 9. Roadmap de Infraestrutura

| Fase | Receita mensal | Infra adicionada | Custo |
|---|---|---|---|
| MVP | R$ 0 | Vercel + Supabase free | **R$ 0** |
| Fase 2 | R$ 2.000+ | FastAPI no Render + Playwright | ~R$ 150/mês |
| Fase 3 | R$ 10.000+ | Railway dedicado + Redis + CDN | ~R$ 800/mês |
| Fase 4 | R$ 50.000+ | AWS/GCP gerenciado, multi-região | escalável |

> A infraestrutura escala junto com a receita. Nunca antes.

---

*Talvrix — Kairos Labs | Architecture v1.1*
