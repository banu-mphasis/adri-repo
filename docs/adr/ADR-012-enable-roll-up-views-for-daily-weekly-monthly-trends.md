# ADR-012: Enable Roll‑up Views for Daily/Weekly/Monthly Trends

- **Status**: Proposed
- **Date**: 2025-08-23

## Context

The current monitoring dashboard displays raw metric streams but lacks pre‑aggregated views for daily, weekly, and monthly trends. Stakeholders need quick insights into trend patterns without waiting for real‑time data to accumulate. The existing stack uses Prometheus for real‑time metrics and Grafana for visualization. However, Grafana queries raw data directly, which can be slow for large time ranges. A dedicated aggregation layer would provide efficient roll‑up views and reduce query load on Prometheus.

## Decision

Implement a dedicated aggregation service that consumes Prometheus metrics via the Pushgateway or via a custom exporter, aggregates them into daily, weekly, and monthly buckets, and stores the results in a time‑series database (e.g., InfluxDB or TimescaleDB). Expose new API endpoints that return aggregated data for the UI. Update Grafana dashboards to use these endpoints for trend panels. Ensure the aggregation service runs as a Kubernetes deployment with horizontal scaling and health checks. Add monitoring for aggregation lag and data consistency. The service will be idempotent and will handle re‑ingestion of data if needed.

## Consequences

1. Requires additional storage for aggregated metrics.
2. Increases complexity of data pipeline.
3. Potential performance impact on query latency.
4. Adds new API endpoints and UI components.
5. Requires monitoring and alerting for aggregation failures.

Benefits:
- Faster trend analysis for daily/weekly/monthly periods.
- Reduced load on raw data store.
- Improved user experience with ready‑made views.

Risks:
- Data consistency issues if aggregation lag.
- Extra maintenance overhead.
- Possible duplication of data if not cleaned up.


## Architecture Diagram

A high‑level architecture diagram showing:
1. Prometheus scraping metrics.
2. Aggregation Service consuming metrics.
3. Aggregated data stored in TimescaleDB.
4. API Gateway exposing aggregated data endpoints.
5. Frontend/UI consuming aggregated data for roll‑up views.