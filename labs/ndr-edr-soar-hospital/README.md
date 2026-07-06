# NDR + EDR + SOAR - Hospital VidaPlena

**Instituição:** SENAI Cyber e IA
**Equipe:** AptX Team
**Autor:** Leonardo Oliveira Feitosa
**Ano:** 2026

---

## Descrição

Simulação completa de um ambiente de segurança corporativo para o Hospital VidaPlena, integrando quatro camadas de defesa em um pipeline NDR + EDR + SOAR. O projeto simula a detecção e resposta a um ataque de ransomware em ambiente hospitalar, cobrindo desde o monitoramento de rede até a automação de resposta a incidentes.

---

## Arquitetura

    Rede do Hospital VidaPlena
          ↓ espelhamento de tráfego (port mirroring via tc)
    Security Onion (NDR) — detecção de anomalias de rede
          ↓ forwarding de logs
    Wazuh (SIEM/EDR) — correlação e alertas
          ↓ ElastAlert webhook
    n8n (SOAR) — automação de resposta
          ↓ notificação
    Gmail SMTP — alerta ao analista

---

## Stack utilizada

| Componente | Tecnologia |
|---|---|
| Hipervisor | Proxmox |
| NDR | Security Onion (Suricata + Zeek) |
| EDR / SIEM | Wazuh + Elastic Security |
| SOAR | n8n |
| Detecção de alertas | ElastAlert |
| Automação de resposta | Wazuh API + n8n Webhooks |
| Notificação | Gmail SMTP |
| Sistema operacional | Linux (Debian/Ubuntu) |

---

## O que foi implementado

### 1. Infraestrutura
- Ambiente virtualizado no Proxmox com múltiplas VMs simulando rede hospitalar
- Port mirroring via `tc` para espelhamento de tráfego para o Security Onion

### 2. NDR — Security Onion
- Captura e análise de tráfego de rede com Suricata e Zeek
- Regras customizadas de detecção de ransomware
- Centralização de alertas do Security Onion no Wazuh

### 3. EDR / SIEM — Wazuh + Elastic Security
- Regras personalizadas de detecção no Wazuh
- Correlação de eventos e geração de alertas
- Integração com Elastic Security para visualização

### 4. SOAR — n8n
- Workflow de automação com trigger via webhook
- Integração com a API do Wazuh para resposta automática
- Notificação por e-mail via Gmail SMTP

### 5. Hardening
- Hardening do Windows Server antes e depois do ataque simulado
- Mitigações implementadas pós-incidente

### 6. Relatório Post-Mortem
- Análise forense completa do incidente
- Mapeamento de táticas e técnicas via MITRE ATT&CK
- Documentação de Indicadores de Comprometimento (IoC)

---

## Evidências

As capturas de tela de cada etapa estão disponíveis na pasta [evidencias/](./evidencias/).

| Arquivo | Descrição |
|---|---|
| alerta-security-onion.png | Alerta gerado pelo Security Onion |
| antes-hardening.png | Estado do ambiente antes do hardening |
| log-windows.png | Log do Windows Server |
| n8n-security.png | Interface do n8n com workflow de segurança |
| rule-ransomware.png | Regra de detecção de ransomware |
| rule-script-ransomware.png | Regra de script de ransomware |
| rules-centralizando-alertas-security-onion-wazuh.png | Centralização de alertas no Wazuh |
| rules-personalizadas-wazuh.png | Regras personalizadas no Wazuh |
| windows-server-atualizado.png | Windows Server após hardening |
| workflow-n8n.png | Workflow completo do n8n |

---

## Demonstração em vídeo

As demonstrações do ambiente em funcionamento estão disponíveis na playlist:
https://www.youtube.com/playlist?list=PLXdOeT6e2DqY

---

## Relatórios técnicos

| Documento | Descrição |
|---|---|
| [relatorio-ambiente-construido.pdf](./docs/relatorio-ambiente-construido.pdf) | Arquitetura e configuração do ambiente |
| [relatorio-report-automatizado.pdf](./docs/relatorio-report-automatizado.pdf) | Report automatizado pelo SOAR |
| [relatorio-post-mortem-forense.pdf](./docs/relatorio-post-mortem-forense.pdf) | Análise forense post-mortem |
| [mitigacoes-implementadas-ambiente.pdf](./docs/mitigacoes-implementadas-ambiente.pdf) | Mitigações implementadas pós-incidente |

---

## Conceitos demonstrados

- Pipeline completo NDR + EDR + SOAR em ambiente corporativo
- Detecção e resposta a ransomware em tempo real
- Automação de resposta a incidentes com n8n e Wazuh API
- Correlação de eventos entre múltiplas ferramentas de segurança
- Hardening de sistemas Windows Server
- Análise forense e mapeamento MITRE ATT&CK
