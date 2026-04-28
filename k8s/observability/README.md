# Observability no Kubernetes

Pacote Kustomize para subir a stack de observabilidade no namespace `observability`.

## Visao da composicao

Este diretorio aplica um padrao comum de plataforma:

- `configmaps.yaml`: configuracao declarativa dos componentes
- `storage.yaml`: persistencia para dados de estado
- `apps.yaml`: deployments e services
- `ingress.yaml`: entrada HTTP amigavel para uso local
- `backup.yaml` e `restore.yaml`: operacao basica de resiliencia para Grafana

Objetivo da composicao: separar claramente "configuracao", "execucao" e "dados".

## O que esta incluso

- Loki
- Tempo
- Prometheus
- OpenTelemetry Collector
- Grafana
- ConfigMaps equivalentes aos arquivos de `config/`
- PVCs para Grafana, Loki e Tempo

## O que nao esta incluso

- Brewer
- Ingress
- Persistencia para Prometheus

O alvo `brewer-app` foi removido do Prometheus neste pacote para evitar falhas de scrape enquanto a aplicacao ainda nao estiver migrada para Kubernetes.

Isso destaca uma diferenca importante entre ambientes:

- no Docker Compose, o alvo pode existir no mesmo host
- no Kubernetes, o alvo precisa existir como Service/Pod no cluster

## Como aplicar

```bash
kubectl apply -k k8s/observability
```

### Ordem mental do bootstrap

1. Namespace
2. ConfigMaps/Secrets
3. PVCs
4. Deployments/Services
5. Ingress

O Kustomize resolve isso no mesmo apply, mas entender a ordem ajuda no debug.

## Como acompanhar

```bash
kubectl get all -n observability
kubectl get pvc -n observability
kubectl logs -n observability deployment/otel-collector
```

## Como acessar

Use port-forward enquanto o Ingress da stack nao estiver definido:

```bash
kubectl port-forward -n observability svc/grafana 3000:3000
kubectl port-forward -n observability svc/prometheus 9090:9090
kubectl port-forward -n observability svc/loki 3100:3100
kubectl port-forward -n observability svc/tempo 3200:3200
```

## Credencial inicial do Grafana

O valor-base de credencial esta em `secrets/grafana-admin.example.env`.

No deploy, o Kustomize gera o Secret `grafana-admin` a partir de `secrets/grafana-admin.env`, usando a chave `GF_SECURITY_ADMIN_PASSWORD`.

Troque esse valor antes de aplicar em ambiente compartilhado.

Sugestao operacional: apos subir a stack, altere a senha admin e simule um
restore para praticar rotinas operacionais basicas.

## Roteiro para novas aplicacoes (onboarding)

Quando uma nova aplicacao entrar no cluster, siga este roteiro para ela usar a
stack de observabilidade ja existente.

Template pronto para copiar e adaptar:

- `k8s/observability/examples/app-deployment-observability-template.yaml`

### 1) Instrumentar a aplicacao com OpenTelemetry

- Habilite envio OTLP para o Collector:
	- endpoint gRPC: `otel-collector.observability.svc.cluster.local:4317`
	- endpoint HTTP: `http://otel-collector.observability.svc.cluster.local:4318`
- Defina identificacao minima do servico:
	- `service.name` (obrigatorio)
	- `service.namespace` (recomendado)
	- `service.version` (recomendado)

Exemplo de variaveis comuns (ajuste para a linguagem/framework):

```yaml
env:
	- name: OTEL_SERVICE_NAME
		value: minha-app
	- name: OTEL_EXPORTER_OTLP_ENDPOINT
		value: http://otel-collector.observability.svc.cluster.local:4318
	- name: OTEL_EXPORTER_OTLP_PROTOCOL
		value: http/protobuf
	- name: OTEL_RESOURCE_ATTRIBUTES
		value: service.namespace=apps,service.version=1.0.0,deployment.environment=dev
```

### 2) Garantir logs com correlacao de trace

- Inclua `trace_id` e `span_id` no log da aplicacao (via MDC/contexto do SDK).
- Isso permite correlacionar logs (Loki) com traces (Tempo) no dashboard de
	correlacao.

### 3) Expor metricas padrao da app

- Priorize metricas HTTP, DB e filas com labels semanticas OTel.
- O dashboard `Application Metrics` ja foi montado para esse conjunto comum e
	deve comecar a mostrar dados sem criar dashboard novo por app.

### 4) Checklist rapido de validacao

```bash
kubectl -n observability exec deploy/prometheus -- \
	wget -qO- 'http://localhost:9090/api/v1/targets'

kubectl -n observability logs deploy/otel-collector --tail=100
```

No Grafana, valide nesta ordem:

- `OTel Collector Health`: collector recebendo e exportando
- `Application Metrics`: trafego HTTP, latencia e erros da nova app
- `Traces and Logs Correlation`: traces e logs com `trace_id`

### 5) Quando criar dashboard dedicado por app

Crie dashboard dedicado somente quando houver requisito especifico de negocio
(SLOs, KPIs, funis, eventos de dominio). Para operacao tecnica inicial, os
dashboards atuais ja cobrem o necessario.
