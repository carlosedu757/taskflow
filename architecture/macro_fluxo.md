
---

## **📡 Fluxo de Comunicação**
1. **Usuário acessa** o *TaskFlow* via navegador → Frontend (React+TS) está hospedado em um container no AKS, servido via **Ingress Controller**.
2. Frontend **chama a API** (.NET 8) via HTTPS para todas as operações (login, CRUD de tarefas, chat).
3. **Autenticação** é feita no Backend → JWT + Refresh Token, armazenado no localStorage/secure cookie.
4. Chat em tempo real via **SignalR** → WebSockets.
5. Backend se comunica com:
   - **PostgreSQL** (armazenamento principal) via EF Core.
   - **Redis** para cache de dados e Pub/Sub do chat/notificações.
   - **Azure Blob Storage** para upload de arquivos.
6. Integrações externas (Google Calendar, GitHub) são feitas pelo **Integration Service** do backend.
7. **CI/CD no Azure DevOps**:
   - Push no `main` → build frontend + backend → gera imagens Docker → envia para o Azure Container Registry.
   - Deploy automatizado no AKS via pipeline YAML.

---

## **🔑 Pontos de Decisão de Arquitetura**
- **Frontend e Backend independentes** → cada um com seu Dockerfile, facilitando escalabilidade.
- **Banco e cache rodando como containers** no AKS em ambiente de dev, mas podendo migrar para **Azure Database for PostgreSQL** e **Azure Cache for Redis** em produção.
- **Ingress Controller no Kubernetes** para roteamento HTTPS.
- **Secrets gerenciados pelo Azure Key Vault**.
- **Horizontal Pod Autoscaler** para escalar backend e frontend conforme carga.
- **Logs centralizados** via Azure Monitor + Application Insights (Fostaria de usar um diferente, por isso a não escolha do Elastic).

---
