# vite-react-app-helm-charts

A Helm chart for deploying a **Vite-based web application** to Kubernetes.

This chart uses a container image hosted on GitHub Container Registry:

```
ghcr.io/marcuwynu23/vite-app-sample:latest
```

---

## 📦 Prerequisites

- Kubernetes cluster (e.g., Minikube, Docker Desktop, or cloud cluster)
- Helm 3.x installed

---

## 🚀 Installation

Clone the repository and install the chart:

```bash
git clone https://github.com/your-repo/vite-app-helm-charts.git
cd vite-app-helm-charts/vite-app
helm install vite-app .
```

---

## 🔄 Upgrade / Reinstall

```bash
helm upgrade vite-app .
```

Clean reinstall:

```bash
helm uninstall vite-app
helm install vite-app .
```

---

## 🌐 Accessing the Application

By default, the service is exposed using **NodePort**:

```yaml
service:
  type: NodePort
  port: 80
```

### If using Minikube:

```bash
minikube service vite-app
```

### If using Docker Desktop / kind:

```bash
kubectl get svc
```

Then open:

```
http://localhost:<nodePort>
```

---

## ⚙️ Configuration

You can customize the deployment via `values.yaml`.

### 🔑 Key Settings

#### Image

```yaml
image:
  repository: ghcr.io/marcuwynu23/vite-app-sample
  tag: "latest"
  pullPolicy: IfNotPresent
```

#### Service

```yaml
service:
  type: NodePort
  port: 80
```

#### Ingress (disabled by default)

```yaml
ingress:
  enabled: false
```

Enable ingress for production use:

```yaml
ingress:
  enabled: true
  className: nginx
  hosts:
    - host: your-domain.com
      paths:
        - path: /
          pathType: Prefix
```

---

## 📊 Scaling

Enable autoscaling:

```yaml
autoscaling:
  enabled: true
  minReplicas: 1
  maxReplicas: 5
  targetCPUUtilizationPercentage: 80
```

---

## ❤️ Health Checks

Liveness and readiness probes are enabled:

```yaml
livenessProbe:
  httpGet:
    path: /
    port: http

readinessProbe:
  httpGet:
    path: /
    port: http
```

---

## 📁 Project Structure

```
vite-app/
├── Chart.yaml
├── values.yaml
├── templates/
│   ├── deployment.yaml
│   ├── service.yaml
│   └── ingress.yaml
```

---

## 🛠 Troubleshooting

### Check Pods

```bash
kubectl get pods
```

### View Logs

```bash
kubectl logs <pod-name>
```

### Describe Resource

```bash
kubectl describe pod <pod-name>
```

### Render Templates (debug)

```bash
helm template .
```

---

## 📌 Notes

- Ensure your container image is publicly accessible or configure `imagePullSecrets`
- Ingress requires an ingress controller (e.g., NGINX)
- NodePort is suitable for local development only

---

## 📄 License

MIT License
