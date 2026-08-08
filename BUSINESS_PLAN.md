# Talvrix — Business Plan

**Produto:** Talvrix  
**Empresa:** Kairos Labs  
**Versão:** 1.1 — Zero-cost MVP  
**Data:** 2026-08-07  

---

## 1. Resumo Executivo

Talvrix é um SaaS de matching de currículos com vagas de emprego usando IA. O produto automatiza o que candidatos fazem manualmente — buscar, filtrar e ranquear vagas por salário e compatibilidade de perfil — em múltiplos sites simultaneamente.

**Mercado-alvo inicial:** Brasil (mercado de ~215 milhões de pessoas, Monster.com bloqueado)  
**Expansão:** América Latina e mercado hispanófono  
**Modelo de receita:** Assinatura recorrente (freemium → planos pagos)

---

## 2. Problema de Mercado

- Monster.com, maior plataforma global de HR tech, **não opera acessivelmente no Brasil**
- Ferramentas brasileiras existentes (Gupy, Vagas.com) são focadas em recrutadores, não em candidatos
- Nenhuma plataforma faz **matching automático currículo ↔ vaga com ranking de salário** para o candidato
- O processo manual consome 3-5 horas por semana de um candidato ativo

---

## 3. Solução e Diferenciais

| Diferencial | Talvrix | Concorrentes |
|---|---|---|
| Upload de currículo real | ✅ | ❌ (buscam por palavras-chave) |
| Matching semântico por IA | ✅ | ❌ |
| Ranking por salário | ✅ | ❌ |
| Multi-site simultâneo | ✅ (Pro) | ❌ |
| Funciona no Brasil | ✅ | ❌ (Monster bloqueado) |

---

## 4. Modelo de Receita

### Planos mensais

| Plano | Preço | Público-alvo |
|---|---|---|
| Free | R$ 0 | Teste / aquisição |
| Basic | R$ 29/mês | Candidatos ativos |
| Pro | R$ 79/mês | Profissionais em recolocação |
| Enterprise | R$ 299/mês | Headhunters, empresas de RH |

### Projeção de receita (conservadora)

| Mês | Usuários Free | Pagantes | MRR estimado |
|---|---|---|---|
| M1 | 100 | 5 | R$ 395 |
| M3 | 500 | 30 | R$ 2.370 |
| M6 | 2.000 | 150 | R$ 11.850 |
| M12 | 8.000 | 600 | R$ 47.400 |
| M18 | 20.000 | 1.800 | R$ 142.200 |

> Taxa de conversão estimada: 7,5% Free → Pago (média SaaS BR)  
> Churn estimado: 8% ao mês nos primeiros 6 meses, caindo para 4% após M6

---

## 5. Estrutura de Custos (MVP)

| Item | Solução | Custo mensal |
|---|---|---|
| Frontend + Backend | Vercel Free (Next.js full-stack) | **R$ 0** |
| Banco de dados + Auth | Supabase Free (500MB, 50k usuários) | **R$ 0** |
| Claude API (IA) | Exclusivo para planos pagos — coberto pela receita | **R$ 0** |
| Scraping | axios + cheerio nas API Routes do Vercel | **R$ 0** |
| Redis / filas | Não necessário no MVP | **R$ 0** |
| Domínio talvrix.com | ~R$ 90/ano (custo único de setup, não operacional) | **R$ 0/mês** |
| **Total operacional MVP** | | **R$ 0/mês** |

> **Break-even imediato:** qualquer assinante pagante já é lucro puro no MVP.  
> O domínio (~R$ 90/ano) é o único desembolso inicial — um custo único de setup, não recorrente.

### Estratégia de custo zero com IA

O plano Free não inclui matching por IA — apenas listagem de vagas ordenada por salário.  
O matching semântico é exclusivo de planos pagos (Basic+). Como cada busca com IA custa ~R$ 0,50 e o Basic custa R$ 29/mês, a margem é garantida desde o primeiro assinante.

### Roadmap de custos por fase

| Fase | Receita mensal | Infra adicionada | Custo |
|---|---|---|---|
| MVP | R$ 0–2.000 | Vercel + Supabase free | **R$ 0** |
| Fase 2 | R$ 2.000+ | Render (FastAPI + Playwright) | ~R$ 150/mês |
| Fase 3 | R$ 10.000+ | Railway dedicado + Redis | ~R$ 800/mês |
| Fase 4 | R$ 50.000+ | AWS/GCP multi-região | escalável |

> A infraestrutura escala junto com a receita. Nunca antes.

---

## 6. Go-to-Market

### Fase 1 — Lançamento BR (M1-M3)
- Produto no ar com MVP funcional
- Distribuição orgânica: LinkedIn, grupos de desenvolvedores no WhatsApp/Telegram
- Nicho inicial: desenvolvedores de software (público que usa Nerdin, vagas.com)
- Meta: 100 usuários free no primeiro mês

### Fase 2 — Crescimento BR (M4-M12)
- Parcerias com bootcamps e cursos online (indicação de ferramenta para alunos)
- SEO: páginas de vagas por stack ("vagas React SP", "vagas Python remoto")
- Afiliados: influenciadores tech no YouTube/Instagram
- Meta: 2.000 usuários, 150 pagantes

### Fase 3 — Expansão LATAM (M13+)
- Suporte a sites de vagas em outros países (getmanfred.com, computrabajo.com)
- Interface em espanhol
- Meta: 20.000 usuários, 1.800 pagantes

---

## 7. Concorrentes

| Empresa | País | Diferença |
|---|---|---|
| Monster | EUA | Bloqueado no BR, focado em recrutadores |
| JobScan | EUA | ATS-focused, não faz scraping |
| Simplify | EUA | Candidatura automática, não ranking |
| Gupy | BR | Plataforma de recrutadores, não de candidatos |
| Vagas.com | BR | Agregador passivo, sem matching |

**Posicionamento:** Talvrix é o único produto focado no candidato, com matching por IA e ranking de salário, operando no Brasil.

---

## 8. Estratégia de Saída (Exit Strategy)

Referência: Trovix (matching de currículos) foi adquirida pela Monster Worldwide por **US$ 72,5 milhões** em 2008.

Potenciais adquirentes de Talvrix:
- **Monster / Randstad** — expandir para LATAM com produto local pronto
- **Gupy / Vagas.com** — adicionar IA de matching ao produto existente
- **LinkedIn / Microsoft** — expansão de capacidades para candidatos BR

**Meta de valuation para saída:** US$ 10-50M (Série A) ou US$ 100M+ (aquisição estratégica)

---

## 9. Próximos Passos Imediatos

| Ação | Prazo |
|---|---|
| Criar repositório GitHub `CabPiz/talvrix` | Hoje |
| Registrar domínio talvrix.com | Esta semana |
| Configurar repositório com PRD + Architecture | Hoje |
| Iniciar desenvolvimento do MVP (Next.js full-stack) | Semana 1 |
| Primeiro scraper funcional (Nerdin) | Semana 2 |
| Integração Claude API para matching (planos pagos) | Semana 3 |
| MVP em produção (Vercel + Supabase — custo zero) | Semana 4 |
| Primeiros 10 usuários beta | Mês 2 |
| **Registrar marca TALVRIX no INPI (Classe 42)** | Antes do lançamento público |

---

*Talvrix — Kairos Labs | Business Plan v1.0*
