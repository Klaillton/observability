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
