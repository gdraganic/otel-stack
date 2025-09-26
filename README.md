# OpenTelemetry Development Stack

A complete observability stack for development purposes using OpenTelemetry, Grafana, Prometheus, Loki, and Tempo. This stack provides comprehensive monitoring, logging, and tracing capabilities for your applications.

## 🏗️ Architecture

This stack consists of the following components:

- **OpenTelemetry Collector** - Receives telemetry data from applications and routes it to appropriate backends
- **Prometheus** - Metrics collection and storage
- **Loki** - Log aggregation and storage
- **Tempo** - Distributed tracing backend
- **Grafana** - Visualization and dashboards for all telemetry data

## 🚀 Quick Start

1. **Start the stack:**
   ```bash
   docker-compose up -d
   ```

2. **Access Grafana:**
   - URL: http://localhost:8030
   - Username: `admin`
   - Password: `admin`

3. **Configure your application** to send telemetry data to the OpenTelemetry Collector:
   - **OTLP gRPC**: `localhost:4317`
   - **OTLP HTTP**: `localhost:4318`

## 📊 Services & Ports

| Service | Port | Description |
|---------|------|-------------|
| Grafana | 8030 | Web UI for dashboards and visualization |
| OpenTelemetry Collector | 4317, 4318 | OTLP gRPC and HTTP receivers |
| Prometheus | 9090 | Metrics storage (internal) |
| Loki | 3100 | Log storage (internal) |
| Tempo | 3200, 4317, 4318 | Trace storage and OTLP receivers (internal) |

## 🔧 Configuration

### OpenTelemetry Collector
The collector is configured to:
- Receive telemetry data via OTLP (gRPC and HTTP)
- Route traces to Tempo
- Route logs to Loki
- Route metrics to Prometheus
- Export metrics for Prometheus scraping

### Grafana Data Sources
Pre-configured data sources:
- **Prometheus** (default) - For metrics visualization
- **Loki** - For log exploration
- **Tempo** - For trace analysis
- **Cross-correlation** - Traces can be linked to logs and metrics

### Tempo Configuration
- Local storage backend for development
- 1-hour block retention
- Metrics generation enabled
- Remote write to Prometheus for trace metrics

### Loki Configuration
- File system storage
- TSDB index for better performance
- 24-hour index period

## 📈 Usage Examples

### Sending Traces
```go
// Example Go application
import (
    "go.opentelemetry.io/otel"
    "go.opentelemetry.io/otel/exporters/otlp/otlptrace/otlptracegrpc"
)

// Configure OTLP exporter
exporter, err := otlptracegrpc.New(ctx, 
    otlptracegrpc.WithEndpoint("localhost:4317"),
    otlptracegrpc.WithInsecure(),
)
```

### Sending Metrics
```go
// Example Go application
import (
    "go.opentelemetry.io/otel/exporters/prometheus"
)

// Configure Prometheus exporter
exporter, err := prometheus.New()
```

### Sending Logs
```go
// Example Go application
import (
    "go.opentelemetry.io/otel/exporters/otlp/otlplog/otlploggrpc"
)

// Configure OTLP log exporter
exporter, err := otlploggrpc.New(ctx,
    otlploggrpc.WithEndpoint("localhost:4317"),
    otlploggrpc.WithInsecure(),
)
```

## 🔍 Exploring Data

### Grafana Dashboards
1. **Metrics**: Create dashboards using Prometheus queries
2. **Logs**: Use Loki data source for log exploration
3. **Traces**: Use Tempo data source for distributed tracing
4. **Correlation**: Click on trace spans to view related logs and metrics

### Prometheus Queries
Access Prometheus directly at `http://localhost:9090` for advanced metric queries.

## 🛠️ Development Tips

### Adding Custom Dashboards
1. Access Grafana at http://localhost:8030
2. Create new dashboards or import existing ones
3. Use the pre-configured data sources

### Extending the Collector
Modify `otel/otel-collector-config.yaml` to:
- Add new receivers (e.g., Jaeger, Zipkin)
- Configure additional processors
- Add new exporters

### Scaling for Production
This configuration is optimized for development. For production:
- Use persistent storage volumes
- Configure proper retention policies
- Set up authentication and TLS
- Use external storage backends (S3, GCS, etc.)

## 🧹 Cleanup

Stop and remove all containers:
```bash
docker-compose down -v
```

## 📚 Additional Resources

- [OpenTelemetry Documentation](https://opentelemetry.io/docs/)
- [Grafana Documentation](https://grafana.com/docs/)
- [Prometheus Documentation](https://prometheus.io/docs/)
- [Loki Documentation](https://grafana.com/docs/loki/)
- [Tempo Documentation](https://grafana.com/docs/tempo/)

## 🔧 Troubleshooting

### Common Issues

1. **Port conflicts**: Ensure ports 8030, 4317, 4318 are available
2. **Data not appearing**: Check that your application is sending data to the correct endpoints
3. **Grafana login issues**: Default credentials are admin/admin

### Logs
View container logs:
```bash
docker-compose logs -f [service-name]
```

### Reset Everything
```bash
docker-compose down -v
docker system prune -f
docker-compose up -d
```
