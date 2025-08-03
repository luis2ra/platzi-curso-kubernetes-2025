# Aplicación Multi-Tier con Docker

Esta es una aplicación de demostración con arquitectura multi-tier compuesta por un frontend (Nginx) y un backend (Flask) que se despliega utilizando Docker Compose.

## 📋 Descripción

La aplicación consiste en:
- **Frontend**: Página web estática tipo "linktree" servida por Nginx
- **Backend**: API REST en Flask que proporciona información de redes sociales
- **Comunicación**: El frontend consume datos del backend vía AJAX

## 🏗️ Arquitectura

```
┌─────────────────┐     HTTP     ┌─────────────────┐
│    Frontend     │─────────────▶│     Backend     │
│  (Nginx:8080)   │              │   (Flask:5001)  │
│   Linktree UI   │              │   API REST      │
└─────────────────┘              └─────────────────┘
```

## 🚀 Despliegue Local

### Pre-requisitos
- Docker
- Docker Compose

### Pasos para desplegar

1. **Navegar al directorio de la aplicación:**
   ```bash
   cd 13-multi-tier/curso-de-docker-fundamentos
   ```

2. **Construir y ejecutar los servicios:**
   ```bash
   docker-compose up --build
   ```

3. **Ejecutar en segundo plano (opcional):**
   ```bash
   docker-compose up -d --build
   ```

## 🔗 Acceso a la Aplicación

Una vez desplegada, puedes acceder a:

- **Frontend**: http://localhost:8080
- **Backend API**: http://localhost:5001/getMyInfo

## ✅ Verificaciones Mínimas

### 1. Verificar que los contenedores están ejecutándose
```bash
docker-compose ps
```
**Resultado esperado:** Ambos servicios deben mostrar estado `Up`

### 2. Verificar el backend
```bash
curl http://localhost:5001/getMyInfo
```
**Resultado esperado:** JSON con información de redes sociales
```json
{
  "author": "CNCF",
  "blog": "https://kubernetes.io/",
  "lastname": "Orquestador",
  "name": "Kubernetes",
  "socialMedia": {
    "facebookUser": "kubernetesio",
    "githubUser": "kubernetes",
    "instagramUser": "cloudnativecomputingfoundation",
    "linkedin": "company/cloud-native-computing-foundation/",
    "xUser": "K8sArchitect"
  }
}
```

### 3. Verificar el frontend
```bash
curl -I http://localhost:8080
```
**Resultado esperado:** Código de respuesta `200 OK`

### 4. Verificar la página web completa
- **Acceder a la página**: http://localhost:8080
- **⚠️ Importante**: Debe mostrar la página "Kubernetes Linktree", NO la página por defecto de nginx
- **Contenido esperado**:
  - Título: "Kubernetes Linktree"
  - Imagen de perfil de Kubernetes
  - Secciones organizadas:
    - **MIS REDES SOCIALES**: Facebook, Instagram, X, Github
    - **MI EXPERIENCIA**: LinkedIn
    - **MI BLOG**: Kubernetes Students
- **Funcionalidad esperada**:
  - Los enlaces deben dirigir a las páginas oficiales de Kubernetes/CNCF
  - El diseño debe mostrar un fondo con la imagen del curso de Kubernetes
  - Los botones deben tener efectos hover (cambio de color al pasar el mouse)

### 5. Verificar la comunicación frontend-backend
- Abrir http://localhost:8080 en el navegador
- Los enlaces de redes sociales deben funcionar correctamente
- **No debe aparecer** ningún error de CORS en la consola del navegador
- **Verificar enlaces específicos**:
  - Facebook → https://www.facebook.com/kubernetesio
  - Instagram → https://www.instagram.com/cloudnativecomputingfoundation
  - X → https://www.x.com/K8sArchitect
  - Github → https://www.github.com/kubernetes
  - LinkedIn → https://www.linkedin.com/company/cloud-native-computing-foundation/
  - Blog → https://kubernetes.io/

### 6. Verificar logs (si hay problemas)
```bash
# Ver logs de todos los servicios
docker-compose logs

# Ver logs específicos
docker-compose logs frontend
docker-compose logs backend
```

## 🎨 Descripción de la Página Web

La aplicación presenta una página estilo "linktree" con las siguientes características:

### Diseño Visual
- **Fondo**: Imagen del curso de Kubernetes de Platzi con overlay oscuro
- **Colores**: Esquema azul y verde con efectos de transparencia
- **Tipografía**: Fuentes modernas con sombras y efectos visuales
- **Responsive**: Adaptable a diferentes tamaños de pantalla

### Contenido
- **Header**: Imagen de perfil de Kubernetes (avatar de GitHub)
- **Título**: "Kubernetes Linktree" con estilo destacado
- **Tres secciones de enlaces**:
  1. **Redes Sociales**: Facebook, Instagram, X, Github
  2. **Experiencia Profesional**: LinkedIn
  3. **Blog/Sitio Web**: Enlace oficial de Kubernetes
- **Footer**: Créditos del curso con iconos decorativos

### Interactividad
- **Efectos hover**: Los botones cambian de color al pasar el mouse
- **Enlaces dinámicos**: Los URLs se cargan desde el backend via AJAX
- **Responsive design**: Se adapta a móviles y escritorio

## 🛑 Detener la Aplicación

```bash
# Detener servicios
docker-compose down

# Detener y eliminar volúmenes (limpieza completa)
docker-compose down -v
```

## 🐛 Solución de Problemas

### Error de CORS
- Verificar que el backend esté ejecutándose en el puerto 5001
- Verificar que `flask_cors` esté instalado en el backend

### Frontend no carga datos
- Verificar que ambos servicios estén ejecutándose
- Revisar la consola del navegador para errores JavaScript
- Verificar que el endpoint `/getMyInfo` responda correctamente

### Puertos ocupados
- Cambiar los puertos en `docker-compose.yml` si 8080 o 5001 están ocupados
- Verificar puertos disponibles: `netstat -tulpn | grep :8080`

### Frontend muestra página por defecto de nginx
- **Problema**: Se ve "Welcome to nginx!" en lugar de la página personalizada
- **Causa**: nginx no encuentra el archivo `index.html`
- **Solución**: El Dockerfile ya incluye un comando para renombrar `linktree.html` a `index.html`
- **Verificar**: Reconstruir la imagen con `docker-compose up --build`

## 📁 Estructura del Proyecto

```
curso-de-docker-fundamentos/
├── docker-compose.yml          # Configuración de servicios
├── backend/
│   ├── Dockerfile             # Imagen del backend
│   ├── app.py                 # Aplicación Flask
│   └── requirements.txt       # Dependencias Python
└── frontend/
    ├── Dockerfile            # Imagen del frontend
    └── sitio/
        ├── linktree.html     # Página principal
        └── requests.js       # Lógica de comunicación con backend
```

## 🎯 Siguiente Paso: Kubernetes

Este proyecto está diseñado para ser migrado a Kubernetes. Los archivos de configuración K8s se encuentran en la carpeta `k8s/`.
