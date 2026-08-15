# Talvrix

> Matching inteligente entre currículos e vagas — feito para candidatos.

**Talvrix** é um SaaS que automatiza a busca, filtragem e ranqueamento de vagas por compatibilidade de perfil e salário — coletando simultaneamente de múltiplas fontes que candidatos hoje varrem manualmente: GitHub, Telegram, RSS, career pages e WhatsApp.

O problema que resolve: o profissional hoje gasta 3–5 horas por semana em 5–10 sites diferentes para encontrar vagas relevantes. O Talvrix faz isso em uma ação.

🌐 [English](./README.en.md) · [Español](./README.es.md)

---

## Modos de uso

### Modo Matching — para quem já sabe o que quer
1. Faz upload do currículo (PDF)
2. IA extrai e estrutura: skills, experiência, senioridade, stack principal
3. Sistema coleta vagas das fontes configuradas
4. IA faz matching semântico + keyword e gera score 0–100 por vaga
5. Resultados ordenados por score e salário

### Modo Explorar Funções — para quem está decidindo o próximo passo
1. Digita uma função de interesse (ex: "Customer Success", "AI Automation")
2. Talvrix agrega dados do pool de vagas: demanda, faixa salarial, skills mais pedidas
3. (V2) Comparação entre até 3 funções com matriz ponderada

---

## Fontes de vagas

| Fonte | Versão |
|---|---|
| GitHub repos (`frontendbr/vagas`, `backend-br/vagas`, `remotemobr/remote-jobs`) | V1 |
| Canais Telegram públicos de vagas | V1 |
| RSS feeds (Trampos.co, Programathor) | V1 |
| Career pages de empresas (SSR e JS-heavy) | V1 |
| WhatsApp forward: usuário encaminha vaga → IA extrai | V1 |
| Grandes agregadores (Indeed BR, Vagas.com, Catho) | V2 |
| API partnerships (LinkedIn, sites latinos) | V4 |

---

## Modelo de receita

| Plano | Preço | O que inclui |
|---|---|---|
| Free | R$ 0 | 1 busca/semana · 5 resultados · sem IA |
| Basic | R$ 29/mês | Buscas ilimitadas · 20 resultados · matching IA |
| Pro | R$ 79/mês | 5 fontes · resultados ilimitados · matching avançado · alertas por e-mail |
| Enterprise | R$ 299/mês | Fontes ilimitadas · API de integração · suporte prioritário |

---

## Status

| Milestone | Descrição | Status |
|---|---|---|
| M1 | Foundation (scaffold, auth, banco) | ⬜ Não iniciado |
| M2 | Core MVP (upload, scraping, resultados) | ⬜ Não iniciado |
| M3 | IA & Monetização (matching, Stripe) | ⬜ Não iniciado |
| M4 | Lançamento (produção, landing, beta) | ⬜ Não iniciado |
| M5 | Crescimento (multi-site, alertas, LATAM) | ⬜ Não iniciado |

---

## Stack

| Camada | Tecnologia |
|---|---|
| Framework | Next.js (App Router) · TypeScript |
| Styling | Tailwind CSS |
| Backend | Supabase (PostgreSQL · Auth · pgvector · RLS) |
| Scraping | @sparticuz/chromium (Vercel-compatible headless Chrome) |
| IA | Anthropic Claude API (extração · matching semântico) |
| Pagamentos | Stripe |
| CI/CD | Vercel · GitHub Actions |
| Qualidade | SonarCloud |

---

## Roadmap

Veja [ROADMAP.md](./ROADMAP.md) para o detalhamento de cada milestone.

---

## Contato

Sugestões e parcerias via site oficial da Kairos Labs:
**[kairos-labs-lake.vercel.app/pt](https://kairos-labs-lake.vercel.app/pt)**

---

## Licença

**Todos os direitos reservados** — Cesar Antonio Brito Pizarro / Kairos Labs

Veja [LICENSE](./LICENSE) · [LICENSE.en](./LICENSE.en) · [LICENSE.es](./LICENSE.es)
