## Levantar un servicio en Kubernetes

### 1. Definir el Pod o Deployment
El archivo [deployment.yml](lab/deployment.yml) crea un `Deployment` llamado `mideployment` con 3 réplicas de `nginx`.

La idea es que Kubernetes mantenga siempre tres pods vivos y, si uno falla, vuelva a crearlo automáticamente.

Script de `Deployment`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
	name: mideployment
	labels:
		app: nginx
spec:
	replicas: 3
	selector:
		matchLabels:
			app: nginx
	template:
		metadata:
			labels:
				app: nginx
		spec:
			containers:
			- name: nginx
				image: nginx
				ports:
				- containerPort: 80
```

### 2. Usar etiquetas para conectar recursos
El `Deployment` etiqueta sus pods con `app: nginx`. Esa etiqueta es clave porque después el `Service` la usa para encontrar los pods correctos.

### 3. Crear el Service
El archivo [service.yml](lab/service.yml) crea un `Service` de tipo `NodePort` llamado `miservice`.

Este servicio expone el puerto 80 del clúster y envía el tráfico a los pods que coincidan con el selector `app: nginx`.

Script de `Service`:

```yaml
apiVersion: v1
kind: Service
metadata:
	name: miservice
	labels:
		run: miservice
spec:
	type: NodePort
	ports:
	- port: 80
		protocol: TCP
	selector:
		app: nginx
```

### 4. Aplicar los manifiestos
Orden recomendado:

1. Crear el Deployment.
2. Crear el Service.
3. Verificar que los pods estén listos.
4. Probar acceso al servicio.

Comandos típicos:

```bash
kubectl apply -f lab/deployment.yml
kubectl apply -f lab/service.yml
kubectl get pods
kubectl get svc
```

### 5. Entender el flujo de red
El cliente no habla directamente con un pod. Primero entra por el `Service`, y Kubernetes distribuye el tráfico entre las réplicas del `Deployment`.

### 6. Caso de prueba con el Pod aislado
El archivo [mipod.yml](lab/mipod.yml) crea un pod independiente con la etiqueta `app: dev`.

Ese pod no quedará detrás del `Service` actual, porque el `Service` busca `app: nginx`. Si quisieras exponer ese pod, tendrías que cambiar el selector del servicio o ajustar la etiqueta del pod.

### Resumen corto

1. Crear el `Deployment` para correr la app.
2. Asegurar que los pods tengan etiquetas correctas.
3. Crear el `Service` para exponerlos.
4. Usar `NodePort` para acceder desde fuera del clúster.
5. Verificar que `selector` y `labels` coincidan.
