# Release Notes - 2026-04-28

## Escopo
Evolucao da stack de observabilidade no Kubernetes com foco em onboarding de
novas aplicacoes e cobertura base de infraestrutura.

## Entregas
- Adicionado `node-exporter` (DaemonSet + Service) para metricas de host.
- Prometheus atualizado para scrape de:
  - `otel-collector:8888` (metricas internas do collector)
  - `node-exporter:9100` (metricas do servidor)
- OTel Collector atualizado para expor metricas internas na porta `8888`.
- Novos dashboards:
  - `Server Resources`
  - `OTel Collector Health`
  - `Application Metrics`
- Dashboards existentes com descricoes nos paineis para uso operacional.
- Guia de onboarding de novas apps no Kubernetes.
- Templates de deployment/service para onboarding:
  - generico
  - Java/Spring
  - Node.js
- Checklist de go-live de observabilidade por aplicacao.

## Impacto esperado
- Menor tempo para integrar nova aplicacao ao stack.
- Melhor visibilidade de saude da plataforma e pipeline OTel.
- Base pronta para escalar observabilidade sem criar dashboards por app no
  primeiro momento.

## Validacao executada
- Targets do Prometheus verificados como `up`.
- Dashboards provisionados e visiveis no Grafana.
- Rollout de componentes relevantes concluido (Grafana/Prometheus/OTel).

## Risco e mitigacao
- Risco: aplicacao sem instrumentacao nao gera dados nos dashboards de app.
- Mitigacao: seguir roteiro em `k8s/observability/README.md` e usar templates
  em `k8s/observability/examples/`.
