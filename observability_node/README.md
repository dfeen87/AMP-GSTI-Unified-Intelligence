# Global Observability Node

A standalone, read-only FastAPI microservice for monitoring and observability of the AMP-GSTI Unified Intelligence Platform.

## 📋 Overview

The Global Observability Node provides transparent access to:
- System resource metrics (CPU, memory, disk, load)
- Database and Redis connectivity status
- GSTI market intelligence state
- AMP candidate evaluation metrics (aggregated, anonymized)
- Hiring forecast indicators
- Audit log summaries (no PII)

## 🔒 Security

**Read-Only by Design:**
- All write operations rejected with HTTP 405
- No state modifications possible
- No secrets or credentials exposed
- Graceful degradation when services unavailable

See [SECURITY.md](SECURITY.md) for comprehensive security documentation.

## 🚀 Quick Start

```bash
# Install dependencies
pip install -r ../requirements.txt

# Run observability node (default: localhost:8081)
python -m observability_node.run

# Or with custom port
export OBS_NODE_PORT=8082
python -m observability_node.run
```

Access at: http://localhost:8081

## 📚 Documentation

- **[QUICKSTART.md](QUICKSTART.md)** - Deployment guide with examples
- **[SECURITY.md](SECURITY.md)** - Security model and measures
- **API Docs:** http://localhost:8081/docs (when running)

## 🧪 Testing

```bash
# Run integration tests
python observability_node/run_integration.py

# Expected output: 12/12 tests passed
```

## 📦 Deployment

### Docker

```bash
# Build
docker build -f Dockerfile.observability -t amp-gsti-obs:latest .

# Run
docker run -d -p 8081:8081 \
  -e DATABASE_URL=postgresql://user:pass@db:5432/amp_gsti \
  amp-gsti-obs:latest
```

### Docker Compose

```bash
docker-compose -f docker-compose.observability.yml up -d
```

### Kubernetes

```bash
kubectl apply -f k8s/observability-deployment.yaml
```

## 🔌 Endpoints

| Endpoint | Description |
|----------|-------------|
| `GET /` | API information |
| `GET /health` | Health check with uptime, connectivity |
| `GET /api/system_state` | System resources, DB/Redis metrics |
| `GET /api/gsti_state` | GSTI market intelligence |
| `GET /api/amp_state` | AMP candidate metrics |
| `GET /api/forecast_state` | Forecast and talent flow |
| `GET /api/audit_summary` | Audit logs (anonymized) |

## 📁 Files

```
observability_node/
├── __init__.py           # Module initialization
├── app.py                # FastAPI application
├── metrics.py            # System metrics collection
├── state_extractors.py   # Intelligence engine state
├── run.py                # Standalone runner
├── run_integration.py    # Integration tests
├── QUICKSTART.md         # Deployment guide
├── SECURITY.md           # Security documentation
└── README.md             # This file
```

## ⚙️ Configuration

Environment variables:
- `OBS_NODE_PORT` - Port to bind to (default: 8081)
- `OBS_NODE_HOST` - Host to bind to (default: 0.0.0.0)
- `DATABASE_URL` - PostgreSQL connection string (optional)
- `REDIS_URL` - Redis connection string (optional)
- `ENVIRONMENT` - Environment mode (development/production)

## 🎯 Use Cases

### Monitoring
- System resource tracking
- Service health monitoring
- Database connection monitoring

### Observability
- Intelligence engine state inspection
- Market regime tracking
- Candidate pool analytics

### Audit
- Activity log review
- Mutation tracking
- Access pattern analysis

## 🛡️ Production Considerations

1. **Deploy behind reverse proxy** (nginx, Traefik) for HTTPS
2. **Set resource limits** in container orchestration
3. **Monitor endpoint response times** for performance
4. **Rate limit** at ingress layer
5. **Enable access logging** for audit trail

## 📝 License

Non-Commercial License - Same as parent project

## 🤝 Contributing

This module follows the same contribution guidelines as the main AMP-GSTI project.

## 📧 Support

For issues or questions, refer to the main project README or open an issue on GitHub.
