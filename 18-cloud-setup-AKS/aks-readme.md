# 18-cloud-setup-AKS

## Azure Kubernetes Service (AKS)

# Recuerda crear tu cuenta segun lo aprendido en el curso de Azure

https://platzi.com/home/clases/11884-az-900/74238-creando-tu-cuenta-de-azure/

# Prerequisitos y Login en tu cuenta de Azure

```bash
az --version
az aks install-cli
az login

```

# (Optional) Configuración Provider Register

```bash
az provider list --output table
az provider register --namespace microsoft.insights
az provider register --namespace Microsoft.ContainerService
```

# Crear un cluster en AKS

```bash
az group create --name k8scourse-aks-demo --location eastus

az aks create --resource-group k8scourse-aks-demo --name k8sCourseAKSDemo --node-count 3 --enable-addons monitoring --generate-ssh-keys --node-vm-size Standard_D2s_v3
```

# Extraer las credenciales del cluster

```bash
az aks get-credentials --resource-group k8scourse-aks-demo --name k8sCourseAKSDemo
```

# Validar la conexión al cluster

```bash
kubectl get nodes
```

# Crear un namespace

```bash
kubectl create namespace platzi
```

# Clean up Cluster

```bash
az aks delete --resource-group k8scourse-aks-demo --name k8sCourseDemoAKS --yes --no-wait
```
