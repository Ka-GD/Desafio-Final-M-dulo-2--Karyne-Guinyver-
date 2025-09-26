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

A arquitetura de defesa segue três pilares: **Perímetro, Rede e Aplicação**, integrados a monitoramento contínuo e gestão de identidade.

```mermaid
flowchart TD
    Internet([Internet])
    WAF[WAF/CRS<br/>Proteção Perímetro]
    LB[Load Balancer<br/>Distribuição de tráfego]
    APP[App Node.js<br/>Lógica da Aplicação]
    DB[(PostgreSQL<br/>Banco de Dados)]
    LOGS[Logs Centralizados<br/>Loki + Grafana]
    IAM[Gestão de Identidade<br/>Vault / Secret Manager]

    Internet --> WAF
    WAF --> LB
    LB --> APP
    APP --> DB
    APP --> LOGS
    DB --> LOGS
    IAM --> APP

    classDef perimeter fill:#f9f,stroke:#333,stroke-width:2px;
    classDef core fill:#9f9,stroke:#333,stroke-width:2px;
    classDef monitoring fill:#ff9,stroke:#333,stroke-width:2px;

    class WAF perimeter;
    class LB perimeter;
    class APP core;
    class DB core;
    class LOGS monitoring;
    class IAM perimeter;
