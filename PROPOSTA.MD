# 📄 Plano de Segurança — LojaZeta

## 1. Sumário Executivo

A LojaZeta, um e-commerce em crescimento, opera em nuvem com Nginx, Node.js e PostgreSQL.  
Recentemente, enfrenta ameaças como SQL Injection (SQLi), Cross-Site Scripting (XSS) e ataques brute-force em `/login`.  
Esses riscos exigem reforço imediato na proteção de aplicações e dados sensíveis.  

O ambiente apresenta vulnerabilidades: ausência de SIEM consolidado, logs dispersos e falta de testes de restauração.  
A equipe é pequena: dois desenvolvedores e um profissional de operações. Isso exige uma estratégia pragmática e econômica.  

Propõe-se uma arquitetura de defesa em camadas, priorizando mitigação das ameaças mais críticas.  
Criação de um monitoramento centralizado mínimo viável (MVP SIEM) para consolidar logs, gerar alertas e aumentar visibilidade.  

Será aplicado um processo simplificado de Resposta a Incidentes (IR) com base no framework NIST IR.  
Runbooks serão criados para SQLi, XSS, brute-force e indisponibilidade.  

**Ganhos esperados:**  
- Redução de indisponibilidade  
- Maior visibilidade e controle  
- Melhoria em MTTD e MTTR  
- Maior confiança de clientes e stakeholders  

Com abordagem 80/20, priorizando quick wins em 30 dias, a LojaZeta elevará sua maturidade em segurança sem comprometer agilidade e orçamento.

---

## 2. Escopo e Metodologia

### Escopo
- Segurança das aplicações web  
- Proteção da identidade de usuários e equipe interna  
- Segurança de dados críticos (clientes e transações)  
- Implementação de monitoramento centralizado mínimo viável  
- Desenvolvimento de runbooks básicos para principais incidentes  

**Fora do escopo imediato:**  
- SOC 24/7  
- Testes avançados de Red Team  
- SIEM corporativo completo  

### Metodologia
- Análise do ambiente atual (Nginx, Node.js, PostgreSQL)  
- Aplicação de boas práticas de Blue Team  
- Modelo de segurança em camadas  
- Ciclo de resposta a incidentes NIST IR:  
  1. Detecção  
  2. Contenção  
  3. Erradicação  
  4. Recuperação  
  5. Lições aprendidas  

**Assunções:**  
- Infraestrutura IaaS permite configuração avançada  
- Equipe com conhecimentos básicos em Linux e administração de sistemas  

---

## 3. Arquitetura de Defesa (Camadas)

### Perímetro
- WAF gerenciado (Cloudflare ou Nginx + ModSecurity CRS)  
- Rate limiting em `/login`  
- CAPTCHA em fluxos críticos  

### Rede
- Segmentação da rede  
- Banco de dados em sub-rede privada  
- ACLs restritivas  
- Bastion host com MFA  

### Host
- Hardening conforme CIS Benchmark  
- Atualizações automáticas críticas  
- Fail2ban configurado  

### Aplicação
- Validação e sanitização de entradas  
- Uso de ORM seguro  
- MFA para painel administrativo  

### Dados
- Criptografia TLS/HTTPS e em repouso  
- Backups criptografados  
- Testes periódicos de restauração  

### Identidade
- Gestão centralizada de segredos (Vault ou Secret Manager)  
- Princípio do menor privilégio  

---

## 4. Monitoramento & SIEM

- Logs centralizados: Nginx, Node.js, PostgreSQL, SO, WAF  
- Centralização via Loki + Grafana (possível evolução para ELK Stack)  
- Alertas automáticos para:  
  - Múltiplas falhas de login  
  - Detecção de SQLi/XSS  
  - Erros críticos  
  - Falha em backups  

**Métricas acompanhadas:**  
- MTTD, MTTR, ataques bloqueados, cobertura de logs  

---

## 5. Resposta a Incidentes

Processo IR baseado em NIST:  
1. **Detecção:** alertas WAF/SIEM, dashboards  
2. **Contenção:** isolar instância, bloquear IPs, revogar credenciais  
3. **Erradicação:** corrigir vulnerabilidades, aplicar patches  
4. **Recuperação:** restaurar backups, monitorar tráfego  
5. **Lições Aprendidas:** post-mortem em até 5 dias  

**Runbooks mínimos:**  
- SQLi: bloquear IP, revisar logs, corrigir ORM  
- XSS: identificar payload, reforçar validação, atualizar WAF  
- Brute-force: bloqueio temporário, ajustar limites, redefinir senha  
- Indisponibilidade: restaurar backup, comunicar status, monitorar sistema  

---

## 6. Recomendações

### Quick Wins (30 dias)
- Ativar WAF e rate limiting no login  
- Centralizar logs em Loki + Grafana  
- Testar restauração de backup  
- Criar runbooks básicos  

### Médio Prazo (90 dias)
- Ativar MFA para devs/ops  
- Hardening automatizado  
- Dashboards de KPIs  
- Desenvolver playbooks expandidos  

### Longo Prazo (180 dias)
- Gestão centralizada de segredos  
- Regras customizadas no WAF  
- Treinamento contínuo em IR  

**Responsáveis:**  
- Devs: segurança do código, MFA, validação  
- Ops: hardening, logs, backup, monitoramento  
- Consultoria: configuração SIEM, runbooks, melhores práticas  

---

## 7. Riscos, Custos e Assunções

**Riscos:**  
- Equipe pequena: necessidade de priorização  
- Orçamento restrito: uso de ferramentas open-source e serviços gerenciados de baixo custo  

**Custos:**  
- Ferramentas open-source: Loki + Grafana, Fail2ban  
- WAF gerenciado (~$20/mês)  
- Investimento em soluções de rápido retorno  

**Assunções:**  
- Infraestrutura IaaS integrada a monitoramento  
- Acesso root/admin da equipe  
- Conhecimento básico em Linux e operações  

---

## 8. Conclusão

A implementação deste plano aumentará a resiliência da LojaZeta frente a ameaças críticas, protegendo dados, aplicações e infraestrutura.  
A abordagem 80/20 garante resultados rápidos em 30 dias e evolução em 90 e 180 dias.  

**Próximos Passos:**  
- Validar escopo com equipe interna  
- Implementar quick wins  
- Testar resposta a incidentes  
- Monitorar KPIs e ajustar processos  

**Critérios de Sucesso:**  
- Redução de incidentes críticos  
- 100% de cobertura mínima de logs  
- MTTR menor que 4 horas  
- Execução efetiva de runbooks  
