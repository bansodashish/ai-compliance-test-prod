# Sample App - Production Environment

This repository contains Kubernetes manifests for the sample application in the **production** environment.

## 📁 Repository Structure
```
test_prod/
├── deployment.yaml     # Main application deployment
├── README.md          # This file
└── .gitignore         # Git ignore file
```

## 🏗️ Deployment Details

### **Environment**: Production
### **Namespace**: `production`
### **Application**: `sample-app-production`

### **Configuration:**
- **Replicas**: 3 (for high availability)
- **Image**: nginx:1.21-alpine
- **Resources**: 
  - CPU: 200m request, 500m limit
  - Memory: 256Mi request, 512Mi limit
- **Environment Variables**:
  - `ENVIRONMENT=production`
  - `LOG_LEVEL=info`

### **Services:**
- **ClusterIP Service**: `sample-app-service` on port 80
- **Ingress**: `sample-app.example.com` (with TLS)

### **Security Features:**
- **TLS Certificate**: Managed by cert-manager
- **Production Health Checks**: Enhanced liveness/readiness probes
- **Resource Limits**: Production-grade resource allocation

## 🚀 Deployment Commands

```bash
# Create namespace
kubectl create namespace production

# Apply manifests
kubectl apply -f deployment.yaml

# Check deployment status
kubectl get pods -n production
kubectl get svc -n production
kubectl get ingress -n production
```

## 🔍 Monitoring & Debugging

```bash
# View logs
kubectl logs -n production deployment/sample-app-production

# Describe pod
kubectl describe pod -n production -l app=sample-app

# Check resource usage
kubectl top pods -n production
```

## ⚠️ Security Notice

**This deployment currently lacks security contexts and will trigger compliance violations:**

- ❌ Missing `runAsNonRoot: true`
- ❌ Missing `runAsGroup` configuration
- ❌ Missing `seccompProfile` settings
- ❌ Missing `allowPrivilegeEscalation: false`
- ❌ Missing `readOnlyRootFilesystem: true`

**This is intentional for testing the AI compliance workflow!**

## 🤖 AI Compliance Workflow

This repository is designed to be used with the AI-enabled compliance workflow:

1. **Trigger Compliance Check**: The AI workflow will scan this deployment
2. **Detect Violations**: Security context violations will be identified
3. **Auto-Remediation**: AI will generate fixes based on `source_of_truth.yaml`
4. **Create PR**: Automated pull request with security improvements

## 🛡️ Production Security

Once the AI compliance workflow applies fixes, this deployment will include:

✅ **Pod Security Context**:
```yaml
securityContext:
  runAsNonRoot: true
  runAsUser: 1000
  fsGroup: 3000
  runAsGroup: 3000
  seccompProfile:
    type: "RuntimeDefault"
```

✅ **Container Security Context**:
```yaml
securityContext:
  allowPrivilegeEscalation: false
  readOnlyRootFilesystem: true
  capabilities:
    drop:
    - ALL
```

## 🔄 Continuous Integration

This repository integrates with:
- **GitHub Actions** for CI/CD
- **n8n Workflows** for compliance automation
- **ArgoCD** for GitOps deployment
- **Monitoring Stack** for production observability