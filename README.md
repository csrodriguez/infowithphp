# Informacion con PHP

Se usa para tener informacion detallada del servidor y asi saber por ejemplo a que pod esta siendo enviado en OpenShift o Kubernetes.

## Con OpenShift y usando S2I

Primero creamos un proyecto

```bash
oc new-project nombre-proyecto
```

A continuacion creamos una nueva aplicacion con ImageStreams y por medio de ~ le indicamos el repositorio que queremos utilizar. Para saber que imagen disponemos de php podemos utilizar el siguiente comando: `oc get is -n openshift | grep php`

```bash
oc new-app php:7.3~https://github.com/csrodriguez/infowithphp.git
```

Ahora creamos la ruta exponiendo el servicio. Si tenemos dudas sobre el nombre del servicio podemos utilizar : `oc get svc`

```
oc expose svc/infowithphp
```

Y para ver la direccion usamos: `oc get routes.route.openshift.io`

Ahora, por ejemplo, en una terminal con bash para ver en que pods le esta siengo direccionado, usamos lo siguiente

```bash
curl direccion-que-nos-da-route -s | grep 'HOSTNAME '
```

Esto tambien puede ser muy util para ver como nos esta siendo redireccionado cuando tenemos varias replicas y utilizando un ciclo for que itere 10 veces por ejemplo nos va a estar mostrando los distintos pods:

```bash
for i in $(seq 10); do curl direccion-que-nos-da-route -s | grep 'HOSTNAME '; done
```
