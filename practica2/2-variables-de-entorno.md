# Variables de Entorno
### ¿Qué son las variables de entorno

Las variables de entorno son parámetros que almacenan información del sistema operativo y del entorno del usuario. Son utilizadas por los programas para obtener información necesaria para su funcionamiento, como rutas de archivos, configuraciones de usuario y detalles de conexión a bases de datos. En el contexto de Docker, las variables de entorno permiten configurar contenedores de manera flexible y portable, asegurando que puedan funcionar en diferentes entornos de desarrollo, prueba y producción con las mismas configuraciones básicas.

### Para crear un contenedor con variables de entorno

``` bash
docker run -d --name <nombre contenedor> -e <nombre variable1>=<valor1> -e <nombre variable2>=<valor2>
```

### Crear un contenedor a partir de la imagen de nginx:alpine con las siguientes variables de entorno: username y role. Para la variable de entorno rol asignar el valor admin.

```bash
docker run -d --name variables -e username=santiago -e role=admin nginx:alpine
```

![Variables de entorno](imagenes/2_captura1.png)

### Comprobación de la creación de variables de entorno

```bash
docker exec variables sh -c 'echo $username'
docker exec variables sh -c 'echo $role'
```
![Variables de entorno](imagenes/2_captura2.png)

### Crear un contenedor con mysql:8 , mapear todos los puertos
```bash
docker run -d --name mysql_container -p 3006:3006 -p 30006:30006 mysql:8
```
![Contenedor con mysql:8](imagenes/2_captura3.png)

### ¿El contenedor se está ejecutando?
No se está ejecutando.

![Ejecución del contenedor mysql:8](imagenes/2_captura4.png)

### Identificar el problema
Se necesita crear las variables de entorno

![Problema con el contenedor mysql:8](imagenes/2_captura5.png)

### Eliminar el contenedor creado con mysql:8 

![Eliminar contenedor mysql:8](imagenes/2_captura6.png)

### Para crear un contenedor con variables de entorno especificadas
- Portabilidad: Las aplicaciones se vuelven más portátiles y pueden ser desplegadas en diferentes entornos (desarrollo, pruebas, producción) simplemente cambiando el archivo de variables de entorno.
- Centralización: Todas las configuraciones importantes se centralizan en un solo lugar, lo que facilita la gestión y auditoría de las configuraciones.
- Consistencia: Asegura que todos los miembros del equipo de desarrollo o los entornos de despliegue utilicen las mismas configuraciones.
- Evitar Exposición en el Código: Mantener variables sensibles como contraseñas, claves API, y tokens fuera del código fuente reduce el riesgo de exposición accidental a través del control de versiones.
- Control de Acceso: Los archivos de variables de entorno pueden ser gestionados con permisos específicos, limitando quién puede ver o modificar la configuración sensible.

Previo a esto es necesario crear el archivo y colocar las variables en un archivo, **.env** se ha convertido en una convención estándar, pero también es posible usar cualquier extensión como **.txt**.
```bash
docker run -d --name <nombre contenedor> --env-file=<nombreArchivo>.<extensión> <nombre imagen>
```
**Considerar**
Es necesario especificar la ruta absoluta del archivo si este se encuentra en una ubicación diferente a la que estás ejecutando el comando docker run.

### Crear un contenedor con mysql:8 , mapear todos los puertos y configurar las variables de entorno mediante un archivo

Crar el archivo variables.env

![Variables para el contenedor mysql:8](imagenes/2_captura7.png)

```bash
docker run -d --name mysql_container --env-file=variables.env -p 3006:3006 -p 30006:30006 mysql:8
```
![Ejecución del contenedor mysql:8](imagenes/2_captura8.png)

# CAPTURA CON LA COMPROBACIÓN DE LA CREACIÓN DE LAS VARIABLES DE ENTORNO DEL CONTENEDOR ANTERIOR 

```bash
docker exec mysql_container sh -c 'echo $MYSQL_USER'
docker exec mysql_container sh -c 'echo $MYSQL_PASSWORD' 
```

![Variables de entorno del contenedor mysql:8](imagenes/2_captura9.png)

### ¿Qué bases de datos existen en el contenedor creado?

```bash
docker exec -it mysql_container mysql -u santiago -p
```

![Bases de datos en el contenedor mysql:8](imagenes/2_captura10.png)