# Other example of use

## Ingress vs LoadBalancer Services
Primero, es importante entender que:

- Ingress no es un servicio, es un recurso de enrutamiento HTTP/HTTPS
- Necesita un Ingress Controller para funcionar (como NGINX, Traefik, etc.)
- El Ingress Controller sí es un servicio (típicamente de tipo LoadBalancer)


```bash
# Primero habilitas el addon de ingress
minikube addons enable ingress
```
El comando previo instala el NGINX Ingress Controller como un servicio de tipo LoadBalancer.


```bash
# 2. Aplicar el ejemplo
kubectl apply -f 07-services-ingress/example2/ingress-example.yaml

# 3a. Verificar la IP del Ingress Controller
k get svc -n ingress-nginx
# NAME                                 TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
# ingress-nginx-controller             NodePort    10.106.107.16   <none>        80:31465/TCP,443:30461/TCP   23m
# ingress-nginx-controller-admission   ClusterIP   10.99.212.214   <none>        443/TCP                      23m

# 3b. Para que funcionen los dominios locales, añade a tu /etc/hosts:
127.0.0.1   myapp.local
127.0.0.1   api.myapp.local

# 3c. Iniciar el tunnel (en una terminal separada)
minikube tunnel

# 4. Verificar la IP del Ingress Controller
kubectl get services -n ingress-nginx
# ingress-nginx-controller LoadBalancer 10.96.144.2 80:80/TCP,443:443/TCP

# 5. Obtener la IP asignada al Ingress
kubectl get ingress web-app-ingress
# NAME              CLASS   HOSTS                           ADDRESS    PORTS
# web-app-ingress   nginx   myapp.local,api.myapp.local   10.96.144.2   80

# 6. Acceso a la Aplicación
curl http://myapp.local
curl http://api.myapp.local/api

```

## Limpieza del Ambiente de Pruebas
### 1. Detener Minikube Tunnel

```bash
# En la terminal donde está corriendo el tunnel, presiona:
Ctrl + C
```

### 2. Elimina los recursos definidos en los ejemplo

```bash
$ kubectl delete -f 07-services-ingress/example2/ingress-example.yaml
deployment.apps "web-app" deleted
service "web-app-service" deleted
ingress.networking.k8s.io "web-app-ingress" deleted
```

### 3. Verificación de Limpieza (usando el alias "k")

```bash
# Verificar que no hay recursos corriendo
k get all

# Verificar estado de minikube
minikube status

# Verificar addons habilitados
minikube addons list
```

---

## 📊 Diferencias Clave: Service LoadBalancer vs Ingress

| **Aspecto**       | **Service LoadBalancer**                               | **Ingress**                                            |
|-------------------|---------------------------------------------------------|---------------------------------------------------------|
| **Protocolo**     | TCP/UDP                                                 | HTTP/HTTPS únicamente                                   |
| **Routing**       | Por puerto                                              | Por host/path                                           |
| **SSL/TLS**       | Requiere configuración manual                           | Terminación SSL automática                              |
| **Múltiples servicios** | Una IP por servicio                                | Una IP para múltiples servicios                         |
| **Costo**         | Mayor (una IP externa por servicio)                     | Menor (una IP para múltiples servicios)                 |
| **Configuración** | Más simple                                              | Más compleja pero más flexible                          |
| **Load Balancing**| Básico (round-robin)                                    | Avanzado (weighted, sticky sessions, etc.)              |
| **Monitoreo**     | Limitado                                                | Métricas HTTP detalladas                                |
