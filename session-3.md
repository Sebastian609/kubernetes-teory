### Autenticacion en kubernetes

La autenticación en Kubernetes verifica la identidad de quien hace una petición al `kube-apiserver`. No define permisos: solo responde quién eres.

Los mecanismos más comunes son certificados cliente, tokens Bearer, `ServiceAccounts`, `OIDC` y webhooks de autenticación. En clústeres modernos, los usuarios humanos suelen autenticarse con un proveedor externo, mientras que los pods y aplicaciones usan `ServiceAccounts` para hablar con la API.

Flujo básico: el cliente envía una solicitud, el API server valida la credencial e identifica al usuario o servicio.

### Autorizacion en kubernetes

La autorización decide qué acciones puede hacer una identidad ya autenticada. En Kubernetes, esto se controla principalmente con **RBAC**.

Un pod puede autenticarse en la API usando un `ServiceAccount`. Luego, con `RBAC`, se le pueden dar permisos limitados, por ejemplo solo leer pods.

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
	name: lector-pods
	namespace: default
---
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
	name: leer-pods
	namespace: default
rules:
- apiGroups: [""]
	resources: ["pods"]
	verbs: ["get", "list", "watch"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
	name: vincular-lector-pods
	namespace: default
subjects:
- kind: ServiceAccount
	name: lector-pods
	namespace: default
roleRef:
	kind: Role
	name: leer-pods
	apiGroup: rbac.authorization.k8s.io
```

### Control de admision

El control de admisión actúa después de la autenticación y la autorización. Su función es filtrar o modificar solicitudes antes de que el objeto quede guardado en el clúster.

Aquí entran mecanismos como `ResourceQuota`, `LimitRanger`, `PodSecurity` y los webhooks de admisión validadores o mutadores. Por ejemplo, pueden impedir que un pod se cree si no cumple una política de seguridad o si excede los recursos permitidos.

Resumen del orden:

1. Autenticación: valida la identidad.
2. Autorización: verifica permisos.
3. Control de admisión: aplica reglas extra sobre la solicitud.


openssl genrsa -out svillars.key 2048