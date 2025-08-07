# 🚀 Node.js AKS Deployment via GitHub Actions
This project demonstrates a complete CI/CD pipeline for a Node.js application deployed to **Azure Kubernetes Service (AKS)** using:
- **Docker**
- **Azure Container Registry (ACR)**
- **Helm**
- **GitHub Actions**
---

##### 📦 Project Structure
```
.
├── .github/
│   └── workflows/
│       └── deploy-aks.yml   # GitHub Actions workflow
├── sample-nodeapp/          # Node.js app (Dockerized)
├── nodeapp/                 # Helm chart for Kubernetes deployment
└── README.md
````
---
#### ✅ 1. Build Stage
Triggered on each push to the `main` branch:
- Logs into Azure Container Registry (ACR)
- Builds Docker image for `sample-nodeapp`
- Tags it with the Git SHA
- Pushes the image to ACR
#### ✅ 2. Deploy Stage
- Loads the Kubernetes config from GitHub Secrets
- Installs Helm
- Deploys the image using the `nodeapp` Helm chart
---
## 🔐 Required GitHub Secrets

| Secret Name          | Description                                |
|----------------------|--------------------------------------------|
| `ACR_REGISTRY_URL`   | Azure Container Registry login server (e.g. `myregistry.azurecr.io`) |
| `ACR_USER`           | ACR service principal or admin username     |
| `ACR_API_KEY`        | Password or service principal secret        |
| `KUBECONFIG`         | Kubeconfig contents (base64-decoded YAML)  |
> Keep these values secure — do not commit them to the repo.
---
## 📥 Setting Up
1. **Create and configure an ACR**  
2. **Push your kubeconfig** into GitHub Secrets (`KUBECONFIG`)  
3. **Create the required ACR secrets** (as above)
4. Ensure your `nodeapp/` Helm chart and `sample-nodeapp/` Docker app are valid
---
## 🧪 Manual Deployment (Optional)
If you want to test the Helm deployment locally:
```bash
helm upgrade --install nodeapp ./nodeapp \
  --set image.repository=myregistry.azurecr.io/demo-nodejs \
  --set image.tag=<your-tag>
````
---
## 📌 Notes
* Helm chart assumes `imagePullSecrets.create=false`. You must create the Kubernetes secret manually or use `kubectl create secret docker-registry ...` if your cluster is private.
* The Helm chart is located in `./nodeapp`.
* The application Dockerfile is in `./sample-nodeapp`.

---

## 🧰 Useful Commands

```bash
# Build and push manually (if needed)
docker build -t myregistry.azurecr.io/demo-nodejs:<tag> ./sample-nodeapp
docker push myregistry.azurecr.io/demo-nodejs:<tag>

# Test Helm chart
helm template nodeapp ./nodeapp
or 
helm upgrade --install nodeapp ./nodeapp \       
    --set imagePullSecrets.create=false \
    --set image.repository=avinash9033/demoapp \
    --set image.tag=latest
```
---
Thanks

