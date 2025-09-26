# PROPOSTA – Opção 2 (Consultoria)

**Cliente:** LojaZeta  
**Data:** <AAAA‑MM‑DD>

---

## 1. Sumário Executivo

A LojaZeta, e-commerce em crescimento, enfrenta riscos críticos como SQLi, XSS e brute‑force em `/login`.  
Atualmente não possui centralização de logs nem SIEM consolidado. Falta também rotina de backup e testes.  
Propomos uma abordagem simples, escalável e de baixo custo para implementar defesa em camadas, monitoramento centralizado  
e um processo estruturado de Resposta a Incidentes baseado no NIST.  

**Ganhos Esperados:**  
- Redução de riscos críticos  
- Visibilidade em tempo real  
- Redução do MTTR  
- Melhor postura de segurança e compliance

---

## 2. Escopo e Metodologia

**Escopo:**  
- Segurança web/app  
- Gestão de identidade  
- Proteção de dados críticos  
- Monitoramento centralizado  
- Runbooks básicos

**Fora do escopo:**  
- SOC 24/7  
- Red Team avançado  
- SIEM corporativo completo

**Metodologia:**  
- Revisão do stack: Nginx, Node.js, PostgreSQL  
- Avaliação de boas práticas Blue Team  
- Implementação em camadas  
- Ciclo de Resposta a Incidentes (NIST IR)

**Assunções:**  
- IaaS flexível  
- Equipe com conhecimento básico de Linux  
- Acesso root/admin aos recursos necessários

---

## 3. Arquitetura de Defesa (Camadas)

**Camadas propostas:**  
1. **Perímetro** — WAF, CRS, Rate Limiting  
2. **Rede** — Segmentação, firewall  
3. **Aplicação** — Hardening, validação de input, ORM  
4. **Dados** — Criptografia, backups  
5. **Identidade** — MFA, gestão de segredos  
6. **Monitoramento** — Logs centralizados, dashboards

**Diagrama:**

```mermaid
flowchart TD
    Internet([🌐 Internet])
    WAF[🔒 WAF/CRS<br/>Proteção Perímetro]
    LB[⚙️ Load Balancer]
    APP[💻 App Node.js]
    DB[(🗄️ PostgreSQL)]
    LOGS[📊 Logs Centralizados]
    IAM[🛡️ Gestão de Identidade]

    Internet --> WAF
    WAF --> LB
    LB --> APP
    APP --> DB
    APP --> LOGS
    DB --> LOGS
    IAM --> APP
