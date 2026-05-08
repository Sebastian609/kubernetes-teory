## Elementos de Kubernetes (Conceptos Core)

Aquí tienes una referencia compacta y enriquecida de los objetos fundamentales en K8s:

### 1. POD (Unidad Básica)
Es la unidad más pequeña desplegable. 
* **Naturaleza:** Efímeros y desechables. Si mueren, no reviven solos.
* **Componentes:** Aloja uno o más contenedores estrechamente acoplados.
* **Recursos:** Comparten red (misma IP, puerto `localhost`) y almacenamiento (volúmenes).
* **Configuración Clave (YAML):** Configurado bajo `spec.containers`.

```yaml
# Ejemplo minimalista de Pod
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels: { app: web }
spec:
  containers:
  - name: nginx
    image: nginx:alpine
```

### 2. LABELS & SELECTORS (Organización)
* **Labels (Etiquetas):** Pares `key: value` (`app: backend`, `env: prod`) adjuntos a objetos. No afectan su comportamiento, solo agrupan lógicamente.
* **Selectors:** Mecanismo usado por Services o Deployments para "pescar" o enlazar Pods específicos mediante sus Labels.

### 3. REPLICA SET (Control de Disponibilidad)
* **Objetivo:** Garantizar que un número exacto de réplicas de un Pod estén siempre corriendo (Mantiene el "estado deseado").
* **Mecanismo:** Si un nodo cae, levanta los Pods faltantes en los nodos sanos usando Label Selectors.
* *Uso Práctico:* Casi nunca se crean a mano; los controla un `Deployment`.

### 4. DEPLOYMENT (Despliegue y Ciclo de Vida)
* **Función:** Representación ideal de un microservicio. Controla a los ReplicaSets de fondo.
* **Ventajas Core:** 
  * Permite **Rolling Updates** (Actualizaciones sin caída del servicio).
  * Permite **Rollbacks** (Volver a la versión anterior si algo falla).
  * Escalado rápido de aplicaciones.

### 5. SERVICE (Redes y Exposición)
Aporta una **IP fija** (ClusterIP) y DNS estable a un grupo de Pods dinámicos, actuando como balanceador de carga.

* **Tipos de exposición:**
  1. `ClusterIP`: Visible solo internamente (Por defecto).
  2. `NodePort`: Abre un puerto estático en todos los nodos del clúster IP.
  3. `LoadBalancer`: Pide balanceador de carga público al proveedor Cloud (AWS, GCP).

### 6. NAMESPACES (Aislamiento Multi-Tenant)
* **Función:** Divide el clúster físico en múltiples clústeres lógicos (`dev`, `staging`, `prod`).
* **Ventajas:** Evita colisiones de nombres, aísla equipos y permite establecer cuotas/límites restrictivos de CPU/RAM.
