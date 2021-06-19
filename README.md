# Configuración servidores - despliegue aplicaciones
## Bootcamp WEB X  
 
Entrega de la práctica perteneciente al módulo de configuración de servidores y despliegue de aplicaciones.

[Volver a Nodepop](https://github.com/JosepCristobal/Nodepop_WebX_Avanzado_PT/blob/master/README.md#conclusiones-finales)

## Objetivo
El objetivo es desplegar en un servidor, que sea de acceso público, la práctica realizada en el módulo de node.js y también la práctica realizada en el módulo de React.
Para el hosting se podrá elegir el que mejor le parezca al alumno (Azure, AWS, Google Cloud, etc.).

## Arquitectura y funcionalidad
 - Utilizar node como servidor de aplicaciones utilizando Pm2 o Supervisor como gestor de procesos node para que siempre esté en ejecución. La aplicación node deberá reiniciarse automáticamente al arrancar el servidor.   
 - Utilizar nginx como proxy inverso que se encrgue de recibir las peticiones http y derivárselas a node.    
 - Los archivos estáticos de la aplicación (imágenes, css, etc.) deberán ser servidos por nginx (no por node). Para poder diferenciar quién sirve estos estáticos, se deberá añadir una cabecera HTTP cuando se sirvan estáticos cuyo valor sea: X-Owner (la X- indica que es una cabecera personalizada) y el valor de la cabecera deberá ser el nombre de la cuenta de usuario en github o bitbucket del alumno. Si la solución de la práctica por parte del alumno no tuviera archivos estáticos, deberá proporcionar el acceso a un archivo estático que se sirva a través de nginx (por ejemplo a través de la URL <dominio>/public/logo.jpg). En este caso, el alumno deberá indicar la URL del archivo estático en el archivo README.md del repositorio.
 - Se accederá al servidor web indicando la dirección IP del servidor en lugar del nombre de dominio, se deberá cargar la práctica realizada en el módulo de React.

En este último apartado, quiero mencionar que tal como se pide que introduciendo la ip pública redirigir a la página de React, al incorporar https en todas la páginas, he decidido hacer una redireccion de la ip publica **http://18.169.74.133 a https://pepetrans.com**. Esta redirección la he configurado en el fichero de configuración "default" en /etc/nginx/sites-availabes.

## Accesos
Para poder probar alguna de las funcionalidades de nuestra aplicación dejo una peuqeña descripción:
 - https://pepetrans.com
 	- Acceso a la página principal de React. Al no estar validados, nos saldrá la página de Login. Esta no funciona ya que no conecta con el backend que utilizamos en la práctica original. 
 - https://www.pepetrans.com
 	- Igual que la anterior.
 - https://node.pepetrans.com
 	- Nos da acceso a la página html que creamos en la práctica de node avanzado. No necesita autenticación. En esta página podemos verificar que todos los ficheros estáticos llevan un header personalizado, tal y como se pide en la práctica.
 - https://node.pepetrans.com/apiv1/authJWT
 	- En este punto podemos adquirir un token con nuestras credenciales para poder acceder a todas la funcionalidades del API, Alta, baja y visualización de anuncios.
 - https://node.pepetrans.com/apiv1/anuncios/?

Todas la funcionalidades se han probado y están operativas.

## Paso a Paso
Guia de despliegue para el yo del mañana.
[Step by Step](https://github.com/JosepCristobal/srv_confServWebX_PT/blob/master/README_Step_By_Step.md)

## Conclusiones finales

Ver una aplicación desarrollada por ti, publicada con un dominio própio y respondiendo a tus peticiones desde un entorno público, esto es una pasada.
Alberto contigo he aprendido muchísimas cosas del mundo web que desconocía, me has abierto muchas puertas y me has marcado un camino que hubiera sido imposible sin tu ayuda.

Muchas gracias por compartir tu conocimiento.





[Volver a Nodepop](https://github.com/JosepCristobal/Nodepop_WebX_Avanzado_PT/blob/master/README.md#conclusiones-finales)
