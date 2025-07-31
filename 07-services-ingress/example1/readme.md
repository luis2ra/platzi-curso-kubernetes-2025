# 07-services-ingress

## 🧭 Objetivo

### Este ejercicio tiene como propósito demostrar el flujo completo para exponer una aplicación en Kubernetes, pasando por dos etapas fundamentales:

1. **Despliegue y exposición de una app "Hello World"**  
   Se despliega una aplicación simple de forma imperativa y se expone al exterior mediante un servicio de tipo `NodePort`.

2. **Configuración de un Ingress Controller y recurso Ingress**  
   Se habilita el Ingress Controller (NGINX en Minikube) y se define un recurso `Ingress` que permite enrutar peticiones HTTP mediante nombres de dominio personalizados (host-based routing), facilitando así una entrada más flexible y escalable al clúster.


## Despliega una app "hello, world":

```bash
kubectl create deployment web --image=gcr.io/google-samples/hello-app:1.0
```

## Verificar el Deployment creado:

```bash
k get deploy
```

## Verificar el pod creado:
```bash
$ k get pods
NAME                   READY   STATUS    RESTARTS   AGE
web-75995f7dbf-sptmz   1/1     Running   0          3m32s
```

## Exposición del Deployment:

```bash
kubectl expose deployment web --type=NodePort --port=8080
```

## Verificamos el servicio creado:
```bash
k get svc
NAME         TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
kubernetes   ClusterIP   10.96.0.1      <none>        443/TCP          6d22h
web          NodePort    10.103.0.154   <none>        8080:32706/TCP   3s
```

## Accede al servicio usando la IP proporcionada:

```bash
minikube service web --url
http://192.168.49.2:32706
```

## Habilita el Ingress controller:

```bash
minikube addons enable ingress
```

## Verifica que el NGINX Ingress controller esté funcionando:

```bash
kubectl get pods -n ingress-nginx
```

## Crea un Ingress:

```bash
kubectl apply -f https://k8s.io/examples/service/networking/example-ingress.yaml
```

## Verifica que el Ingress esté correctamente configurado (sin address):

```bash
kubectl get ingress
```

## Visualizar el contenido del Ingress directamente desde kubectl:

```bash
kubectl describe ingress example-ingress
```

## Para que funcionen los dominios locales, añade a tu /etc/hosts:
```bash
# /etc/hosts

127.0.0.1   hello-world.example
```
 
## Iniciar el tunnel (en una terminal separada)
```bash
minikube tunnel
```

---

### 🧹 Limpieza de recursos

Una vez finalizada la práctica, puedes eliminar los recursos creados para liberar espacio y evitar conflictos futuros:

```bash
# Elimina el recurso Ingress
kubectl delete ingress example-ingress

# Elimina el servicio expuesto
kubectl delete service web

# Elimina el deployment de la app
kubectl delete deployment web

# (Opcional) Deshabilita el Ingress Controller en Minikube
minikube addons disable ingress