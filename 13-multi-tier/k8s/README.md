# Despliegue de Aplicación Multi-Tier en Kubernetes

Esta guía explica cómo desplegar la aplicación frontend + backend en Kubernetes utilizando Minikube.

## 📋 Prerequisitos

- Kubernetes (Minikube)
- kubectl configurado
- Docker
- **Imágenes Docker construidas**: `frontend:latest` y `backend:latest`
  - *Ver `../curso-de-docker-fundamentos/README.md` para construir las imágenes*

## ⚙️ Configuración del Ambiente

### 1. Configurar Minikube para usar imágenes locales

```bash
# Iniciar Minikube con driver Docker
minikube start --driver=docker

# Configurar el entorno Docker para usar el registry de Minikube
eval $(minikube docker-env)

# Verificar que las imágenes están disponibles internamente en k8s
docker images | grep -E "(frontend|backend)"
```

### 2. Habilitar addons necesarios

```bash
# Habilitar metrics-server para monitoreo
minikube addons enable metrics-server
```

## 🚀 Despliegue en Kubernetes

### 1. Desplegar Backend

```bash
# Aplicar deployment del backend
kubectl apply -f backend/deployment.yaml

# Aplicar service del backend
kubectl apply -f backend/service.yaml
```

### 2. Desplegar Frontend

```bash
# Aplicar deployment del frontend
kubectl apply -f frontend/deployment.yaml

# Aplicar service del frontend
kubectl apply -f frontend/service.yaml
```

### 3. Aplicar todos los recursos de una vez (alternativa)

```bash
# Aplicar todos los archivos YAML
kubectl apply -f backend/
kubectl apply -f frontend/
```

## ✅ Verificación del Despliegue

### 1. Verificar pods
```bash
kubectl get pods
```
**Resultado esperado:** Todos los pods en estado `Running`

### 2. Verificar servicios
```bash
kubectl get services
```

### 3. Verificar deployments
```bash
kubectl get deployments
```

### 4. Acceder a la aplicación

```bash
# Obtener URL del frontend
minikube service frontend-service --url

# Obtener URL del backend
minikube service backend-service --url
```

### 5. Verificar logs (si hay problemas)
```bash
# Ver logs del frontend
kubectl logs -l app=frontend

# Ver logs del backend
kubectl logs -l app=backend
```

## 🌐 Acceso a la Aplicación

- **Frontend**: Usar la URL obtenida con `minikube service frontend-service --url`
- **Backend API**: Usar la URL obtenida con `minikube service backend-service --url`

## 🧹 Limpieza del Ambiente Post-Prueba

### 1. Eliminar recursos de Kubernetes

```bash
# Eliminar todos los recursos creados
kubectl delete -f backend/
kubectl delete -f frontend/

# O eliminar recursos individuales
kubectl delete deployment backend-deployment frontend-deployment
kubectl delete service backend-service frontend-service
```

### 2. Verificar limpieza
```bash
# Verificar que no queden pods
kubectl get pods

# Verificar que no queden servicios
kubectl get services
```

### 3. Limpiar imágenes Docker (opcional)
```bash
# Listar imágenes
docker images

# Eliminar imágenes específicas
docker rmi frontend:latest backend:latest

# O limpiar todas las imágenes no utilizadas
docker image prune -a
```

### 4. Detener Minikube (opcional)
```bash
# Detener Minikube
minikube stop

# Eliminar completamente el cluster (cuidado!)
minikube delete
```

## 🐛 Solución de Problemas

### Pod en estado ImagePullBackOff
- Verificar que las imágenes están disponibles: `docker images | grep -E "(frontend|backend)"`
- Asegurar que `imagePullPolicy: Never` está configurado en los deployments
- Verificar que `eval $(minikube docker-env)` se ejecutó correctamente

### Servicios no accesibles
- Verificar que Minikube está ejecutándose: `minikube status`
- Usar `minikube service <service-name> --url` para obtener la URL correcta
- Verificar que los puertos en el Service coinciden con los del Deployment

### Frontend no se conecta al Backend
- Verificar que ambos servicios están ejecutándose
- Revisar logs de los pods para errores de conectividad: `kubectl logs -l app=frontend`
- Verificar que el frontend puede resolver el backend service

## 📁 Estructura de Archivos K8s

```
k8s/
├── README.md                   # Esta guía
├── backend/
│   ├── deployment.yaml         # Deployment del backend
│   └── service.yaml            # Service LoadBalancer del backend
└── frontend/
    ├── deployment.yaml         # Deployment del frontend
    └── service.yaml            # Service LoadBalancer del frontend
```
