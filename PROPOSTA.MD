# 📌 Plano de Segurança - LojaZeta

Este documento descreve o plano de segurança da LojaZeta, visando reduzir riscos críticos, implementar defesa em camadas, centralizar monitoramento e estruturar resposta a incidentes seguindo o NIST.

---

## 1. Sumário Executivo
A LojaZeta, e-commerce em crescimento, enfrenta riscos como SQLi, XSS e brute-force em `/login`.  
O ambiente carece de SIEM consolidado, centralização de logs e testes de backup.  
Equipe enxuta (2 devs + 1 ops) exige estratégia simples, escalável e de baixo custo.  
**Objetivo:** reduzir riscos críticos com defesa em camadas, monitoramento centralizado e IR baseado no NIST.

---

## 2. Escopo e Metodologia
**Escopo:** segurança web/app, identidade, dados críticos, monitoramento centralizado, runbooks básicos.  
**Fora do escopo:** SOC 24/7, Red Team avançado, SIEM corporativo completo.  
**Metodologia:** análise do stack (Nginx, Node.js, PostgreSQL), boas práticas Blue Team, segurança em camadas e ciclo NIST IR.  
**Assunções:** IaaS flexível, equipe com conhecimento Linux básico.

---

## 3. Arquitetura de Defesa

A arquitetura de defesa da LojaZeta integra **Perímetro, Rede, Aplicação e Monitoramento** em um fluxo visual limpo.

```mermaid
flowchart TD
    Internet([🌐 Internet])
    WAF[🔒 WAF/CRS<br/>Proteção Perímetro]
    LB[⚙️ Load Balancer<br/>Distribuição de tráfego]
    APP[💻 App Node.js<br/>Lógica da Aplicação]
    DB[(🗄️ PostgreSQL<br/>Banco de Dados)]
    LOGS[📊 Logs Centralizados<br/>Loki + Grafana]
    IAM[🛡️ Gestão de Identidade<br/>Vault / Secret Manager]

    Internet --> WAF
    WAF --> LB
    LB --> APP
    APP --> DB
    APP --> LOGS
    DB --> LOGS
    IAM --> APP

    classDef perimeter fill:#1f2b45,stroke:#0a1a2b,stroke-width:2px,color:#ffffff;
    classDef core fill:#3b4b63,stroke:#1f2b45,stroke-width:2px,color:#ffffff;
    classDef monitoring fill:#6c7a95,stroke:#3b4b63,stroke-width:2px,color:#ffffff;

    class WAF,LB,IAM perimeter;
    class APP,DB core;
    class LOGS monitoring;
