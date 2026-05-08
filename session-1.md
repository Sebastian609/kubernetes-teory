# ☸️ K8s Quick Reference (Cheat Sheet)

Aquí tienes una referencia rápida y técnica para trabajar con Kubernetes a nivel local.

---

## 🚀 Minikube (Infraestructura)
Minikube despliega un clúster local de un solo nodo, ideal para desarrollo y pruebas.

| Comando | Acción |
| :--- | :--- |
| `minikube start` | Levantar / Iniciar el clúster |
| `minikube status` | Ver el estado del clúster |
| `minikube dashboard` | Abrir la GUI Web (Gestión visual) |
| `minikube stop` | Detener el clúster sin perder datos |
| `minikube delete` | Eliminar el clúster y toda su configuración |

> **Dashboard:** Permite ver logs, escalar pods y editar YAMLs de forma visual. Usa el flag `--url` para obtener el enlace directo en entornos remotos o sin interfaz gráfica nativa.

---

## 🎮 kubectl (Plano de Control)
Es la herramienta CLI principal (cliente) para enviar órdenes a la API de Kubernetes.

* **Configuración:** Se ubica por defecto en `~/.kube/config` (gestionada mediante *contextos* para cambiar entre clústeres).
* **Pro-tip (Alias):** Añade `alias k='kubectl'` a tu archivo `~/.bashrc` o `~/.zshrc` para ahorrar tiempo.

### 📂 Comandos Clave (CLI)

#### 1. Inspección
```bash
k get nodes           # Listar los nodos del clúster
k get pods            # Listar pods en el namespace actual (-A para todos)
k get svc             # Listar servicios
k describe pod [name] # Ver detalles técnicos y registro de eventos (errores)
```

#### 2. Gestión Operativa
```bash
k apply -f app.yaml   # Crear o actualizar un recurso basado en un archivo YAML
k delete -f app.yaml  # Eliminar los recursos definidos en el YAML
k delete pod [name]   # Eliminar un pod específico manualmente
```

#### 3. Debugging y Troubleshooting
```bash
k logs [pod-name]             # Ver la salida estándar (consola) del contenedor
k logs -f [pod-name]          # Seguir (follow) los logs en tiempo real (tail)
k exec -it [pod-name] -- sh   # Abrir una sesión interactiva (shell) dentro del contenedor
```

---

## 💡 Conceptos Fundamentales
* **Minikube:** Representa el *host* o motor donde viven los recursos.
* **kubectl:** Es el *mando* o volante para interactuar con la API.
* **Driver:** Al iniciar Minikube, se recomienda usar Docker como driver (`--driver=docker`) para un aislamiento ligero y rápido.
* **Ref**: kubernetes-teory
