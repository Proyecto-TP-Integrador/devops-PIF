
## TP18 — Gestión Centralizada de Vulnerabilidades con OWASP DefectDojo

Integración de **OWASP DefectDojo Community Edition (v3.4.101)** para consolidar, desduplicar y auditar los hallazgos de OWASP ZAP (DAST), Threagile (Threat Modeling), Semgrep (SAST), Trivy (SCA/Container/IaC) y Gitleaks (Secret Scanning).

### Arquitectura de Integración en 4 Fases

| Fase | Herramienta | Función | Mecanismo |
|------|-------------|---------|-----------|
| Fase 1 | Docker Buildx | Compilación inmutable | Exporta `app-image.tar` |
| Fase 2 | OWASP ZAP / Threagile / Semgrep / Trivy / Gitleaks | Auditoría multidominio | Genera reportes XML/JSON estructurados |
| Fase 3 | OWASP DefectDojo | Vulnerability Management | Ingestión vía API v2 (runner self-hosted) y deduplicación |
| Fase 4 | Helm & K8s | Despliegue a producción | Push a Docker Hub y release en Kubernetes |

### Mapeo de reportes a parsers de DefectDojo

| Reporte | Herramienta | scan_type |
|---------|-------------|-----------|
| `zap-report.xml` | OWASP ZAP (TP13C) | `ZAP Scan` |
| `threagile-risks.json` | Threagile (TP14) | `Threagile risks report` |
| `semgrep-results.json` | Semgrep (TP15) | `Semgrep JSON Report` |
| `trivy-container-results.json` / `trivy-sca-results.json` / `trivy-iac-results.json` | Trivy (TP16) | `Trivy Scan` |
| `gitleaks-report.json` | Gitleaks (TP17) | `Gitleaks Scan` |

### Comandos de Verificación Local

```bash
chmod +x scripts/verificar-defectdojo.sh
DEFECTDOJO_URL="http://localhost:8080" ./scripts/verificar-defectdojo.sh
```

El token se toma de `DEFECTDOJO_API_KEY` o de `~/.defectdojo-token` y nunca se versiona en el repositorio.
