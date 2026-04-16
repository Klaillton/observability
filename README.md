# 📊 Observability

Repositório centralizado de observabilidade para configuração, gerenciamento e monitorização de projetos locais — voltado para estudo, treinamento e demais necessidades que surjam ao longo do tempo.

---

## 🚀 Visão Geral

Este repositório contém as configurações e ferramentas de observabilidade para projetos locais, incluindo:

- **Métricas** — Coleta e visualização de métricas dos serviços
- **Logs** — Centralização e análise de logs
- **Traces** — Rastreamento distribuído entre serviços
- **Alertas** — Configuração de alertas e notificações

---

## 🛠️ Tecnologias Utilizadas

- [Prometheus](https://prometheus.io/) — Coleta de métricas
- [Grafana](https://grafana.com/) — Visualização de dashboards
- [Loki](https://grafana.com/oss/loki/) — Agregação de logs
- [Tempo](https://grafana.com/oss/tempo/) — Rastreamento distribuído
- [OpenTelemetry](https://opentelemetry.io/) — Instrumentação dos serviços

---

## 📁 Estrutura do Projeto

```
observability/
├── config/            # Configurações do collector, Prometheus e Tempo
├── grafana-storage/   # Dados persistentes do Grafana
├── tempo-data/        # Dados persistentes do Tempo
├── docker-compose.yml
├── README.md
└── SECURITY.md
```

---

## ⚙️ Como Usar

### Pré-requisitos

- Docker e Docker Compose instalados

### Executando localmente

```bash
# Clone o repositório
git clone https://github.com/Klaillton/observability.git

# Acesse o diretório
cd observability

# Suba os serviços
docker-compose up -d
```

---

## 🌿 Branches

| Branch | Descrição |
|--------|-----------|
| `main` | Branch principal — configurações estáveis |
| `develop` | Branch de desenvolvimento |
| `feature/*` | Branches de novas funcionalidades |
| `hotfix/*` | Branches de correções urgentes |

---

## 🤝 Contribuindo

1. Crie uma branch a partir de `develop`: `git checkout -b feature/minha-feature`
2. Faça suas alterações e commit: `git commit -m "feat: minha feature"`
3. Envie para o repositório: `git push origin feature/minha-feature`
4. Abra um **Pull Request** para a branch `develop`

---

## 📬 Contato

Dúvidas ou sugestões, abra uma [issue](https://github.com/Klaillton/observability/issues) no repositório.