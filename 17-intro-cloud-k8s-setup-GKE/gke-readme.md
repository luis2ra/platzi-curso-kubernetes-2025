# 17-intro-cloud-k8s-setup-GKE


## Google Kubernetes Engine (GKE)

# Prerequisitos y Login en tu cuenta de Google Cloud

```bash
./google-cloud-sdk/install.sh
gcloud auth login
```

# Activar API de Kubernetes Engine y Compute Engine

```bash
gcloud services enable container.googleapis.com
gcloud services enable compute.googleapis.com
```

# Crear un cluster en GKE

```bash
gcloud container clusters create k8s-course-demo --zone us-central1-a --num-nodes 2
```

# Install plugin kubectl

```bash
gcloud components install gke-gcloud-auth-plugin
```

# (Optional) Configurar contexto de k8s en k8bectl

```bash
gcloud container clústers get-credentials k8s-course-demo --zone us-central1-a
```

# Clean up Cluster

```bash
gcloud container clusters delete k8s-course-demo --zone us-central1-a
```