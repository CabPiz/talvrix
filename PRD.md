# Talvrix — Product Requirements Document (PRD)

**Produto:** Talvrix  
**Empresa:** Kairos Labs  
**Versão:** 1.0  
**Data:** 2026-08-07  

---

## 1. Visão do Produto

Talvrix é uma plataforma SaaS que usa IA para fazer matching inteligente entre o currículo do usuário e vagas de emprego em múltiplos sites, ordenando os resultados por salário e compatibilidade de perfil — em segundos, sem esforço manual.

> "O falcão que enxerga a oportunidade certa antes de qualquer outro."

---

## 2. Problema

Encontrar vagas relevantes é manual, fragmentado e ineficiente:

- O candidato acessa múltiplos sites separadamente
- As buscas por palavra-chave ignoram o perfil real do currículo
- Não há ordenação por salário agregada entre sites
- O candidato não sabe se é competitivo para a vaga antes de se candidatar
- Ferramentas existentes como Monster não operam no Brasil de forma acessível

---

## 3. Solução

Talvrix automatiza todo o processo:

1. Usuário faz upload do currículo (PDF)
2. IA extrai e estrutura skills, experiência e nível de senioridade
3. Scraper coleta vagas dos sites configurados
4. IA faz o match e gera score de compatibilidade por vaga
5. Resultados são exibidos ordenados por salário + score

---

## 4. Usuários-alvo

| Perfil | Necessidade principal |
|---|---|
| Dev em busca de recolocação | Encontrar vagas tech bem pagas sem perder tempo |
| Profissional sênior | Vagas compatíveis com experiência, sem filtrar manualmente |
| Recém-formado | Entender onde se encaixa no mercado |
| Qualquer área | Automatizar busca em múltiplos sites simultaneamente |

---

## 5. MVP — Escopo Fase 1

### Funcionalidades incluídas
- Upload de currículo em PDF
- Configuração de 1 site de vagas pelo usuário
- Scraping automatizado do site configurado
- Match IA entre currículo e vagas encontradas
- Exibição de até 10 vagas ranqueadas por salário + score de compatibilidade

### Fora do escopo do MVP
- App mobile
- Candidatura automática
- Chat com recrutadores
- ATS (Applicant Tracking System)
- Recomendação automática de sites

---

## 6. Funcionalidades por Plano

### Free
- 1 busca por semana
- 1 site de vagas
- Até 5 resultados ordenados por salário
- Sem matching por IA (listagem por salário apenas)
- Sem exportação

### Basic — R$ 29/mês
- Buscas ilimitadas
- 1 site de vagas
- Até 20 resultados
- Exportação CSV

### Pro — R$ 79/mês
- Até 5 sites de vagas simultâneos
- Resultados ilimitados
- Alertas automáticos por e-mail (novas vagas compatíveis)
- Score de compatibilidade detalhado por vaga

### Enterprise — R$ 299/mês
- Sites ilimitados
- IA recomenda sites com base no perfil do usuário
- API de integração para empresas e headhunters
- Suporte prioritário

---

## 7. Critérios de Sucesso do MVP

| Métrica | Meta |
|---|---|
| Tempo de resposta (upload → resultados) | < 60 segundos |
| Relevância percebida pelo usuário | > 70% nas buscas |
| Conversão Free → Basic | > 5% no primeiro mês |
| NPS após primeira busca | > 40 |

---

## 8. Jornada do Usuário (MVP)

```
Acessa talvrix.com
    → Cria conta (e-mail ou Google)
    → Faz upload do currículo (PDF)
    → Informa o site de vagas a buscar
    → Aguarda processamento (< 60s)
    → Visualiza lista de vagas ranqueadas
    → Clica na vaga → redireciona para o site original
```

---

## 9. Riscos e Mitigações

| Risco | Mitigação |
|---|---|
| Site bloqueia scraping | Rotação de user-agent + rate limiting respeitoso |
| Currículo em formato não legível | Validação de PDF na entrada + fallback para texto |
| Custo de IA por busca | IA restrita a planos pagos — receita da assinatura cobre o custo |
| Concorrente lança produto similar | Velocidade de execução + foco no mercado BR |

---

*Talvrix — Kairos Labs | PRD v1.0*
