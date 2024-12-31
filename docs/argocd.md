# Install Argo CD

ArgoCD needs to be installed on your Kubernetes cluster to manage the GitOps workflow. Follow these steps to install it:

## Steps 1.1: Install ArgoCD on Ubuntu

Run the following commands to install ArgoCD CLI:
```bash
# Retrieve the version of the current release
ARGOCD_VERSION=$(curl --silent "https://api.github.com/repos/argoproj/argo-cd/releases/latest" | grep '"tag_name"' | sed -E 's/.*"([^"]+)".*/\1/')

# Retrieve the binary from GitHub to a temporary location
curl -sSL -o /tmp/argocd-${ARGOCD_VERSION} https://github.com/argoproj/argo-cd/releases/download/${ARGOCD_VERSION}/argocd-linux-amd64

# Make the binary executable and move it to a location within $PATH /usr/local/bin
chmod +x /tmp/argocd-${VERSION}
sudo mv /tmp/argocd-${VERSION} /usr/local/bin/argocd 

# Verify installation
argocd version --client
```
Run the following command to install ArgoCD:

```bash
# ArgoCD Installation
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

## Steps 1.2: Access ArgoCD CLI

Expose the ArgoCD server
```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
Get the username and password
```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath='{.data.password}' | base64 -d
```
Open the browser and go to `http://localhost:8080`. Use `admin` as the username and use the password that you got from the last command