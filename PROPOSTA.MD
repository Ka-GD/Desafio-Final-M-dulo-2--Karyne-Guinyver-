# 📄 Plano de Segurança — LojaZeta

## 1. Sumário Executivo

A LojaZeta, um e-commerce em crescimento, opera em nuvem com Nginx, Node.js e PostgreSQL.  
Enfrenta ameaças como SQL Injection (SQLi), Cross-Site Scripting (XSS) e brute-force em `/login`.  
Esses riscos exigem reforço imediato na proteção de aplicações e dados sensíveis.  

Ambiente atual apresenta vulnerabilidades: ausência de SIEM consolidado, logs dispersos e falta de testes de restauração.  
Equipe enxuta: dois desenvolvedores e um profissional de operações exigem estratégia pragmática e econômica.  

Propõe-se arquitetura de defesa em camadas, priorizando mitigação de ameaças críticas.  
Criação de um monitoramento centralizado mínimo viável (MVP SIEM) para consolidar logs, gerar alertas e aumentar visibilidade.  

Será aplicado processo simplificado de Resposta a Incidentes (IR) baseado no framework NIST IR.  
Runbooks para SQLi, XSS, brute-force e indisponibilidade serão desenvolvidos.  

**Ganhos esperados:**  
- Redução de indisponibilidade  
- Maior visibilidade e controle  
- Melhoria em MTTD e MTTR  
- Maior confiança de clientes e stakeholders  

Abordagem 80/20: quick wins em 30 dias para elevar maturidade em segurança sem comprometer agilidade e orçamento.

---

## 2. Escopo e Metodologia

| Escopo Imediato | Fora do Escopo |
|-------------------|----------------|
| Segurança de aplicações web | SOC 24/7 |
| Proteção de identidade de usuários e equipe | Testes avançados de Red Team |
| Segurança de dados críticos | SIEM corporativo completo |
| Implementação de monitoramento mínimo viável | |
| Desenvolvimento de runbooks básicos | |

### Metodologia
1. Análise do ambiente atual (Nginx, Node.js, PostgreSQL)  
2. Aplicação de boas práticas de Blue Team  
3. Modelo de segurança em camadas  
4. Ciclo NIST IR:
   - Detecção
   - Contenção
   - Erradicação
   - Recuperação
   - Lições aprendidas  

**Assunções:**  
- Infraestrutura IaaS configurável  
- Equipe com conhecimento básico em Linux e administração de sistemas  

---

## 3. Arquitetura de Defesa (Camadas)

| Camada       | Medidas |
|--------------|---------|
| **Perímetro** | WAF gerenciado (Cloudflare/Nginx + ModSecurity CRS), rate limiting em `/login`, CAPTCHA |
| **Rede**      | Segmentação, banco em sub-rede privada, ACLs restritivas, Bastion host com MFA |
| **Host**      | Hardening CIS Benchmark, atualizações automáticas, Fail2ban |
| **Aplicação** | Validação/sanitização de inputs, ORM seguro, MFA painel administrativo |
| **Dados**     | Criptografia TLS/HTTPS e em repouso, backups criptografados, testes periódicos |
| **Identidade**| Gestão centralizada de segredos, princípio do menor privilégio |

---

## 4. Monitoramento & SIEM

- Logs centralizados: Nginx, Node.js, PostgreSQL, SO, WAF  
- Centralização via Loki + Grafana (possível evolução para ELK Stack)  
- Alertas automáticos:
  - Falhas de login
  - SQLi/XSS detectado
  - Erros críticos
  - Falha em backups  

**Métricas acompanhadas:**  
- MTTD, MTTR, ataques bloqueados, cobertura de logs  

---

## 5. Resposta a Incidentes

**Processo IR (NIST):**
1. **Detecção:** alertas WAF/SIEM, dashboards  
2. **Contenção:** isolar instância, bloquear IPs, revogar credenciais  
3. **Erradicação:** corrigir vulnerabilidades, aplicar patches  
4. **Recuperação:** restaurar backups, monitorar tráfego  
5. **Lições Aprendidas:** post-mortem em até 5 dias  

**Runbooks mínimos:**  
- **SQLi:** bloquear IP, revisar logs, corrigir ORM  
- **XSS:** identificar payload, reforçar validação, atualizar WAF  
- **Brute-force:** bloqueio temporário, ajustar limites, redefinir senha  
- **Indisponibilidade:** restaurar backup, comunicar status, monitorar sistema  

---

## 6. Recomendações

### Quick Wins (30 dias)
- [ ] Ativar WAF e rate limiting em `/login`  
- [ ] Centralizar logs em Loki + Grafana  
- [ ] Testar restauração de backup  
- [ ] Criar runbooks básicos  

### Médio Prazo (90 dias)
- [ ] Ativar MFA para devs/ops  
- [ ] Hardening automatizado  
- [ ] Dashboards de KPIs  
- [ ] Desenvolver playbooks expandidos  

### Longo Prazo (180 dias)
- [ ] Gestão centralizada de segredos  
- [ ] Regras customizadas no WAF  
- [ ] Treinamento contínuo em IR  

**Responsáveis:**  
- Devs: segurança do código, MFA, validação  
- Ops: hardening, logs, backup, monitoramento  
- Consultoria: configuração SIEM, runbooks, melhores práticas  

---

## 7. Riscos, Custos e Assunções

| Riscos | Custos | Assunções |
|--------|--------|-----------|
| Equipe pequena: necessidade de priorização | Loki + Grafana (open-source), Fail2ban | Infraestrutura IaaS integrada |
| Orçamento restrito: uso de ferramentas open-source | WAF gerenciado (~$20/mês) | Acesso root/admin |
| | Investimento em soluções de rápido retorno | Conhecimento básico em Linux e operações |

---

## 8. Conclusão

A implementação deste plano aumentará a resiliência da LojaZeta frente a ameaças críticas, protegendo dados, aplicações e infraestrutura.  
Abordagem 80/20 garante resultados rápidos em 30 dias e evolução em 90 e 180 dias.

**Próximos Passos:**  
- Validar escopo com equipe interna  
- Implementar quick wins  
- Testar resposta a incidentes  
- Monitorar KPIs e ajustar processos  

---

## Arquitetura de Defesa (Camadas)

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
