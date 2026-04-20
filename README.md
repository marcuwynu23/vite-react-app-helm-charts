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

## Installation (via Artifact Hub)

The easiest way to install the chart is directly from the remote repository.

1. Add the Repository

```bash
helm repo add marcu-repo https://marcuwynu23.github.io/vite-react-app-helm-charts/
helm repo update
```

2. Install the Chart

```bash
helm install vite-react-app marcu-repo/vite-react-app-helm-charts
```

## Upgrade & Scaling

### To Upgrade to a newer version:

```bash
helm repo update
helm upgrade vite-react-app marcu-repo/vite-react-app-helm-charts
```

### To Scale Replicas (e.g., scale to 4):

You can override the default values directly from the command line:

```bash
helm upgrade vite-react-app marcu-repo/vite-react-app-helm-charts --set replicaCount=4
```

### Configuration Overrides

If you want to use a custom my-values.yaml file with the remote chart:

1. Create a my-values.yaml locally.

2. Run the install/upgrade with the -f flag:

```bash
helm upgrade --install vite-react-app marcu-repo/vite-react-app-helm-charts -f my-values.yaml
```

---

## 📌 Notes

- Ensure your container image is publicly accessible or configure `imagePullSecrets`
- Ingress requires an ingress controller (e.g., NGINX)
- NodePort is suitable for local development only

---

## 📄 License

MIT License
