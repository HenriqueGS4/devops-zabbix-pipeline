cat << 'EOF' > README.md
# 🚀 Esteira de CI/CD com Deploy Automatizado e Observabilidade em Tempo Real

Este projeto demonstra a implementação de uma infraestrutura moderna de **Integração e Entrega Contínua (CI/CD)** integrada com **Observabilidade em Tempo Real**, combinando **Jenkins**, **Docker**, **Nginx** e **Zabbix 7.0 (Agent 2 com LLD)**.

---

## 🛠️ Arquitetura do Ambiente

A solução foi projetada de forma distribuída em ambiente Linux, segmentando responsabilidades entre instâncias dedicadas:

* **Servidor Controlador / CI-CD (`pipeline` - IP):** Instância dedicada ao serviço **Jenkins**, responsável pela execução de tarefas de automação, clone do código fonte e orquestração de deploy via SSH.
* **Servidor de Aplicação (`aplicacao` - IP):** Host de destino onde o ambiente **Docker Engine** executa a aplicação web contêinerizada.
* **Servidor de Observabilidade (`zabbix` - IP):** Servidor central **Zabbix 7.0 Server** coletando métricas de infraestrutura e contêineres em tempo real.

---

## 🔄 Fluxo do Pipeline (CI/CD)

```text
[Desenvolvedor / Git Push] 
       │
       ▼
[GitHub Repository]
       │
       ▼
[Jenkins Pipeline] ──(SSH / Chave ed25519)──► [VM Aplicação (Docker Host)]
                                                        │
                                                        ▼
                                           [Container Nginx / App]
                                                        │
                                                        ▼ (Métricas / LLD)
                                              [Zabbix Agent 2]
