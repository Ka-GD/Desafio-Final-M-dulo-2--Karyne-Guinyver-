# 📌 Plano de Segurança - LojaZeta

Este documento apresenta o plano de segurança da LojaZeta, focado em reduzir riscos críticos, implementar defesa em camadas, centralizar monitoramento e estruturar resposta a incidentes conforme o NIST.

---

## 📑 Índice
1. [Sumário Executivo](#1-sumário-executivo)  
2. [Escopo e Metodologia](#2-escopo-e-metodologia)  
3. [Arquitetura de Defesa](#3-arquitetura-de-defesa)  
4. [Monitoramento & SIEM](#4-monitoramento--siem)  
5. [Resposta a Incidentes](#5-resposta-a-incidentes)  
6. [Recomendações (80/20)](#6-recomendações-8020)  
7. [Riscos, Custos e Assunções](#7-riscos-custos-e-assunções)  
8. [Conclusão](#8-conclusão)  

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
- **Perímetro:** WAF + rate limit em `/login` + CAPTCHA.  
- **Rede:** banco em subnet privada + ACL mínima + bastion host com MFA.  
- **Host:** hardening (CIS), patches automáticos, Fail2ban.  
- **Aplicação:** sanitização inputs, ORM seguro, painel admin com MFA.  
- **Dados:** TLS, criptografia em disco/db, backups criptografados e testados.  
- **Identidade:** gestão de segredos (Vault/Secret Manager), menor privilégio.  

```mermaid
flowchart LR
  Internet --> WAF[WAF/CRS]
  WAF --> LB[Load Balancer]
  LB --> APP[App Node.js]
  APP --> DB[(PostgreSQL)]
  SIEM[Coleta/Correlação] -->|Logs| APP & DB
  IAM[Gestão de Identidade] --> APP
