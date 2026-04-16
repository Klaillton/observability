# 📊 Observability EPO

Repositório de observabilidade da EPO, centralizado para monitoramento, rastreamento e logging dos serviços.

---

## 🚀 Visão Geral

Este repositório contém as configurações e ferramentas de observabilidade utilizadas nos serviços da EPO, incluindo:

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
observability-epo/
├── dashboards/        # Dashboards do Grafana
├── alerts/            # Regras de alertas
├── configs/           # Configurações gerais
├── docs/              # Documentação
├── README.md
└── SECURITY.md
```

---

## ⚙️ Como Usar

### Pré-requisitos

- Docker e Docker Compose instalados
- Acesso ao ambiente da EPO

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
| `main` | Branch principal — código estável e em produção |
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

Dúvidas ou sugestões, entre em contato com o time da EPO.