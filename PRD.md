# Talvrix — Product Requirements Document (PRD)

**Produto:** Talvrix
**Empresa:** Kairos Labs
**Versão:** 2.0
**Data:** 2026-08-13

---

## 1. Visão do Produto

Talvrix é uma plataforma SaaS que faz matching inteligente entre o currículo do usuário e vagas coletadas de múltiplas fontes — GitHub, Telegram, RSS, career pages e input direto do usuário — ordenando por salário e compatibilidade de perfil.

> "O falcão que enxerga a oportunidade certa antes de qualquer outro."

**Posicionamento:** candidato-first. Nenhuma ferramenta brasileira faz "currículo ↔ vaga" com matching semântico + keyword híbrido e ranking de salário para o candidato.

---

## 2. Problema

- Candidatos acessam 5–10 sites manualmente: 3–5 horas por semana desperdiçadas
- Busca por palavra-chave ignora o perfil real do currículo
- Vagas relevantes aparecem em fontes fragmentadas (GitHub, Telegram, RSS, sites de empresa) sem agregação
- Ferramentas brasileiras existentes (Gupy, Vagas.com) são focadas em recrutadores

---

## 3. Solução

1. Upload do currículo em PDF
2. IA extrai e estrutura: skills, experiência, senioridade, stack principal
3. Sistema coleta vagas de fontes cooperativas automaticamente
4. Hybrid Search (semântico + keyword com RRF) ranqueia vagas por compatibilidade
5. Resultados exibidos ordenados por score + salário

**Por que Hybrid Search (não só semântico):** termos técnicos como "Node.js 18", "CLT", "PJ", siglas de stacks precisam de match exato — busca vetorial pura erra esses casos.

---

## 4. Usuários-alvo

| Perfil | Necessidade |
|---|---|
| Dev em busca de recolocação | Vagas tech bem pagas sem varrer 10 sites |
| Recém-formado | Entender onde se encaixa no mercado |
| Profissional sênior (V2+) | Alertas automáticos de vagas compatíveis |
| Headhunter independente (V3+) | Rastrear candidatos para múltiplos clientes |

---

## 5. Escopo por Versão

### V1 MVP — custo R$0/mês

**Incluído:**
- Upload de currículo em PDF → parsing → `ResumeStructured` (Zod)
- Coleta de vagas de fontes cooperativas:
  - GitHub repos públicos de vagas (`frontendbr/vagas`, `backend-br/vagas`, `remotemobr/remote-jobs`)
  - Canais Telegram públicos via Bot API oficial
  - RSS feeds de sites que expõem feed
  - Career pages estáticas e JS-heavy via `@sparticuz/chromium` no Vercel
  - WhatsApp forward: usuário encaminha mensagem → IA extrai a vaga
- Hybrid Search: embedding do CV + `hybrid_search_jobs()` no Supabase
- Matching IA: `MatchScore` por vaga (Haiku para Basic, Sonnet para Pro)
- Listagem de vagas ranqueadas por score + salário
- Planos Free (sem IA) + Basic (R$29/mês)
- Integração Stripe

**Fora do escopo do V1:**
- Sites com anti-scraping agressivo (Indeed, Catho, LinkedIn)
- Alertas por e-mail
- Candidatura automática
- App mobile
- ATS

### V2 (desbloqueado com R$2.000 MRR)

- Scraping de Indeed BR, Vagas.com, Catho via Playwright dedicado (FastAPI no Render)
- Inngest para jobs de scraping em background
- Alertas por e-mail de novas vagas compatíveis
- Plano Pro (R$79/mês)

### V3 (desbloqueado com R$10.000 MRR)

- XML feed partnerships negociadas com job boards
- API pública para headhunters
- Plano Enterprise (R$299/mês)

---

## 6. Funcionalidades por Plano

### Free — R$0
- 1 busca por semana
- 5 resultados ordenados por salário (sem IA)
- Fontes: GitHub + Telegram + RSS

### Basic — R$29/mês
- Buscas ilimitadas
- 20 resultados com matching IA (Claude Haiku)
- Exportação CSV
- Todas as fontes V1

### Pro — R$79/mês
- Resultados ilimitados com matching IA (Claude Sonnet)
- Até 5 fontes simultâneas (V2)
- Alertas automáticos por e-mail (V2)
- Score de compatibilidade detalhado com breakdown

### Enterprise — R$299/mês (V3)
- Fontes ilimitadas
- API de integração
- Suporte prioritário

---

## 7. Requisitos Não-Funcionais

| Requisito | Meta |
|---|---|
| Tempo de resposta (upload → resultados) | < 60 segundos |
| Relevância percebida pelo usuário | > 70% nas buscas |
| Custo por busca com IA (Basic) | < R$1,00 (margem positiva desde o primeiro assinante) |
| Uptime | > 99% (Vercel SLA) |
| Segurança | RLS no Supabase por user_id; rate limiting por plano; input sanitization |
| LGPD | Currículo armazenado por usuário autenticado; vagas são dados públicos |

---

## 8. Requisitos de IA e Guardrails

- Limite de tamanho do PDF: 10MB
- Limite de tokens enviados ao LLM: 8.000 por chamada
- Timeout: 30s por chamada ao LLM
- Rate limiting: Free = 1 busca/semana; Basic = 100 buscas/dia; Pro = ilimitado
- Toda chamada ao LLM registrada em `agent_runs` (observabilidade de custo)
- `routeModel()` verifica plano do usuário antes de qualquer chamada: Free → null, Basic → Haiku, Pro → Sonnet

---

## 9. Jornada do Usuário (V1 MVP)

```
Acessa talvrix.com
  → Cria conta (e-mail ou Google)
  → Faz upload do currículo (PDF)
  → Sistema coleta vagas das fontes configuradas
  → [Free] Visualiza top 5 vagas por salário (sem IA)
  → [Basic/Pro] Aguarda matching IA (<60s)
  → Visualiza lista de vagas ranqueadas por score + salário
  → Clica na vaga → redireciona para a fonte original
```

---

## 10. Status do Repositório

**O que já existe:**
- Repositório `CabPiz/talvrix` criado
- 12 issues abertas (M1–M5)
- 5 milestones criadas
- CLAUDE.md no padrão v4.0
- LICENSE proprietário

**O que precisa ser implementado:**
- Tudo — nenhum código foi escrito ainda
- Issues existentes serão revisadas/fechadas/atualizadas na Fase 4 da re-inicialização

---

## 11. Riscos e Mitigações

| Risco | Mitigação |
|---|---|
| Fonte muda estrutura e quebra scraper | Testes E2E por fonte; alertas de falha; priorizar fontes API-first |
| Anti-scraping bloqueia IP | Respeitar robots.txt; user-agent rotation; fontes API-first no V1 |
| Custo de IA excede receita | `routeModel()` + `agent_runs` + margem de R$28,50 por assinante Basic |
| LGPD | Dados do CV ficam no Supabase do usuário autenticado; RLS isola por user_id |

---

## 12. Próximos Passos

Ver `concentrador/talvrix/architecture.md § Roadmap de Implementação` para issues do MVP por milestone.

---

*Talvrix · PRD v2.0 · 2026-08-13*
