# 08-configs-secrets

## Apply ConfigMap
```bash
# Creates or updates the ConfigMap from the auth-config.yaml file
kubectl apply -f 08-configs-secrets/auth-config.yaml
```

## View ConfigMap
```bash
# Lists the ConfigMap named 'auth-config' in the current namespace
kubectl get configmap auth-config
```

## Detail ConfigMap
```bash
# Detail the ConfigMap named 'auth-config'
kubectl describe configmap auth-config
```

## Create secret imperatively
```bash
# Creates a Secret named 'auth-secret' with the specified key-value pairs
kubectl create secret generic auth-secret \
  --from-literal=client_id=myclientid \
  --from-literal=client_secret=secret

# Alternative: If you have a .env file with KEY=VALUE format
kubectl create secret generic auth-secret --from-env-file=.env

# Lists the Secret in the current namespace
kubectl get secret

# Detail
kubectl describe secret auth-secret

# Edit
kubectl edit secret auth-secret

```

## Apply Secret declaratively
```bash
# Creates or updates the Secret from the auth-secret.yaml file
kubectl apply -f 08-configs-secrets/auth-secret.yaml
```

## View Secret
```bash
# Lists the Secret named 'auth-secret' in the current namespace
kubectl get secret auth-secret
```

## Other links
- https://www.base64decode.org/

- https://external-secrets.io/latest/
