# 📊 Observability

Repositório centralizado de observabilidade para configuração, gerenciamento e monitorização de projetos locais.

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

### Entendendo o fluxo de observabilidade deste repositorio

1. A aplicacao instrumentada envia telemetria (traces, metrics, logs) para o OpenTelemetry Collector.
2. O Collector roteia cada tipo de sinal para o backend certo:
	- traces -> Tempo
	- metrics -> endpoint Prometheus do proprio Collector
	- logs -> Loki
3. O Prometheus coleta metrics periodicamente por scrape.
4. O Grafana consulta Prometheus, Loki e Tempo para dashboards e correlacao entre sinais.

Em termos de desenho de arquitetura, pense em tres camadas:

- Coleta e roteamento: OpenTelemetry Collector
- Armazenamento especializado: Prometheus (metrics), Loki (logs), Tempo (traces)
- Visualizacao e exploracao: Grafana

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

### Portas e para que servem

- `3000`: Grafana (UI)
- `3100`: Loki (API de logs)
- `3200`: Tempo (API de traces)
- `4317`: OTLP gRPC (entrada principal de telemetria)
- `4318`: OTLP HTTP (entrada alternativa de telemetria)
- `8889`: endpoint de metrics exportadas pelo Collector para scrape do Prometheus
- `9090`: Prometheus (UI e API)

### Roteiro de validacao tecnica

Fluxo recomendado para validar a stack ponta a ponta:

1. Suba a stack.
2. Gere carga simples em uma aplicacao instrumentada.
3. Confira traces no Tempo via Grafana.
4. Localize logs da mesma requisicao no Loki.
5. Compare latencia do trace com metricas de request duration no Prometheus.

Esse ciclo ajuda a entender observabilidade de forma integrada, e nao como ferramentas isoladas.

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