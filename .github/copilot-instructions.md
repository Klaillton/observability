# GitHub Copilot Instructions

This repository contains an observability stack using OpenTelemetry, Prometheus, Grafana, and Tempo.

## Stack Overview

- **OpenTelemetry Collector**: Receives, processes, and exports telemetry data (traces, metrics, logs)
- **Prometheus**: Metrics collection and storage
- **Grafana Tempo**: Distributed tracing backend
- **Grafana**: Visualization and dashboards

## Guidelines

- All services are orchestrated via `docker-compose.yml`
- Configuration files live under `config/`
- Environment variables are documented in `.env.example`
- Keep Docker image versions pinned when possible (avoid `latest` in production)
- Follow OpenTelemetry conventions for collector pipeline configuration
