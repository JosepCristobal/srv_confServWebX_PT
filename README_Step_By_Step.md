## Configuración de servidores y despliegue de aplicaciones
[Volver al README](https://github.com/JosepCristobal/srv_confServWebX_PT#paso-a-paso)

## Step by step

Con este documento intento detallar todos los pasos que se han seguido en la configuración del servidor y el despliegue de las aplicaciones.


1. Utilizamos AWS para desplegar nuestro servidor de la práctica.
2. Escogemos Ubuntu 20.04 LTS Tipo de instáncia T2.micro. Sólo marcamos "protegerse contra la terminación accidental". El resto lo dejamos por defecto.
3. Pasamos al apartado de almacenamiento y lo dejamos como nos viene por defecto, 8 GB, 100/3000 Iops. Dejamos marcado eliminar al terminar.
4. Siguiente paso, añadimos una etiqueta name - SRV_PTConfServWebX
5. Creamos el grupo de seguridad al que nos adherimos. Aprovechamos el que se creó en el curso.
6. Revisamos y lanzamos.
7. Nos solicita si generamos clave o aprovechamos de existentes. Nosotros creamos claves nuevas y las descargamos.
8. Verificamos que las tenemos en nuestro ordenador y lanzamos la instancia.
9. Ya tenemos nuestro servidor arrancado.
10. En nuestro ordenador, nos situamos en donde hemos guardado el certificado y cambiamos los permisos para poder utilizarlo en nuestra conexión
11. chmod 0600 nombreCert.pem
12. A continuación nos creamos una IP pública fija para nuestro servidor.
13. En AWS en red y seguridad nos vamos a direcciones ip elásticas y asignamos una ip pública fija a nuestro servidor-
14. Ahora ya podemos entrar via ssh a nuestro servidor con el comando
15. ssh -i certificado.pem ubuntu@IP_Servidor.
16. Una vez dentro, hacemos un update y un upgrade para actualizar nuestro servidor a la última en parches de seguridad.
17. sudo apt update
18. sudo apt upgrade
19. Hacemos una asignación de DNS. Ya dsponíamos de un dominio creado en clase, lo reasignamos a la IP del nuevo servidor.
20. Instalamos nginx.
21. Instalamos herramientas recomendadas en el curso.
	- Build-essential
	- libssl -dev zlib1g-dev
	- libbz2 -dev
	- libreadline -dev python3
	- fail2band
	
22. Entramos en nginx.conf para deshabilitar el mensaje de bienvenida que nos muestra la versión de nginx y el S.O.
23. Server-tokens off Descomentamos la linea
24. En AWS abrimos puerto 80 en el firewall y probamos que podemos entrar a la página por defecto de nginx.
25. Vamos a subir nuestro proyecto de React
26. Creamos un nuevo usuario llamado web: sudo adduser web pass 4321
27. Ahora lo bloqueamos: sudo passwd -l web
28. Dejamos el servidor y nos vamos a nuestro ordenador donde tenemos el proyecto de Reac
29. PUBLIC_URL=/ npm run build
30. Subimos la carpeta build a nuestro servidor de AWS
31. scp -r -i certificado.pem build ubuntu@ipServidor:~/
32. Ya lo tenemos en nuestro servidor.
33. Ahora lo movemos a la carpeta nodepop del usuario web y lo hacemos propietario.
34. sudo mv build/ /home/web/nodepop
35. sudo chown -R web:web /home/web/nodepop/
36. Creamos fichero de configuración en /etc/nginx/sites_available sudo nano nodepopWeb
37. Ahora deberemos crear un acceso directo en sites-enabled
38. sudo ln -s ruta_origen ruta_destino
39. Ahora vamos a desplegar una aplicación de node en nuestro servidor.
40. Creamos usuario app: sudo adduser apps
41. Lo bloqueamos sudo passwd -l apps
42. Ahora nos cambiamos de usuario
43. sudo -u apps -i
44. Instalamos nvm para poder controlar la versión de no que más nos convenga para cada aplicación.
45. Navegamos a la web de nvm y copiamos url qe empieza por curl-o- y ejecutamos.
46. Salimos de la sesión de usuario apps y volvemos a entrar para que se cargue la nueva configuración instalada.
47. nvm --version nos da la versión 0.38.0
48. Ahora instalamos node en su última versión LTS
49. nvm install --lts
50. node --version 
51. Ahora hacemos el clone de nuestra app de nodepop que hicimos como práctica en node avanzado.
52. git clone https:// repositorio
53. nos vamos al directorio donde está el package.json 
54. Seguimos como usuario apps y ejecutamos npm install para actualizar todos los paquetes.
55. Ahora, en el directorio raiz de nuestra aplicación creamos un fichero .env para indicar dos variables de entorno que debemos configurar.
56. Ahora nos situamos como usuario ubuntu
57. Instalamos MongoDB
58. Descargamos con wget.
59. Vamos a Mongo comunity y copiamos los enlaces de descarga con la configuración
60. version 4.4.6 ubuntu 20.06 server
61. version 4.4.6 ubuntu 20.06 shell(deb)
62. Para instalarlo
63. sudo dpkg -i mongodb-org-server .deb
64. sudo dpkg -i mongodb-org-shell   .deb
65. Una vez instalado, arrancamos mongoDB manualmente para verificar que funciona correctamente.
66. sudo systemctl start mongod
67. Ahora pasaremos un comando para que arranque siempre con el S.O.
68. sudo systemctl enable mongod
69. Nos creará un link al sistema de arranque del SO
70. Ahora nos situamos en la raiz de nuestro proyecto de node y ejecutamos nuestro script de inicialización
71. npm run installDB
72. Ha funcionado!!!
73. Hacemos pruebas y conseguimos visualizar datos en navegador web y en Postman.
74. Para conseguir llegar a nuestra aplicación de node hemos abierto el puerto 3000 en el firewall de nuestro servidor de AWS.
75. Siguientes pasos: 
	- Vamos a securizar mongo para que utilice usuario y contraseña para acceder a la BBDD.
	- Acceso al API a través de Nginx.
	- Activar acceso https para todos los accesos.
	- Arrancar servicios de node de forma automática con supervisor.
76. Vamos a por MongoDB.
77. Pasamos a usuario ubuntu
78. Creamos usuario admin en MongoDB con el rol de userAdminAnyDatabase y readWriteAnyDatabase.
79. Entramos en la shell de Mongo
80. use admin y creamos el usuario. Ver web de mongo
81. Modificamos el fichero /etc/mongod.conf
82. Descomentamos securyty , pulsamos intro, añadimos dos espacios y escribimos authorization: enabled
83. Reiniciamos el servicio de MongoDB: sudo systemctl restart mongod
84. Conectamos a mongo: mongo --authenticationDatabase admin -u admin -p
85. Una vez conectados como administrador, creamos un usuario para nodepop.
86. Nos situamos en cursonode: use cursonodes y creamos el usuario según el script de ejemplo del curso o de la documentación de mongo en la web.
87. Ahora para conectarnos a la BBDD deberemos hacerlo con la siguiente cadena de conexión
88. mongodb://cursonode:miPassword@127.0.0.1:27017/cursonode
89. Lo probamos y funciona. Ahora debemos cambiar la cadena de conexión en nuestro fichero .env
90. El siguiente paso será el de arrancar node con Supervisor.
91. Instalamos supervisor con sudo apt install supervisor.
92. Estamos con el usuario ubuntu.
93. Ahora debemos crear un fichero de configuración y lo hacemos en /etc/supervisor/conf.d/cursonode.conf según configuración de los apuntes de clase. También podemos consultar en la web de supervisor.
94. Verificamos que funcione y arranque el servicio correctamente.
95. sudo systemctl status supervisor
96. Reiniciamos las instancias para ver si arrancan todos los servicios que hemos ido instalando y todo arrancado de forma correcta.
97. Ahora creamos otro fichero de configuración para supervisor con el proposito de que nos arranque un microservicio que tenemos en nuestro proyecto de node que nos crea thumnails de la fotos de los anuncios. micronode.conf
98. Reiniciamos supervisor y todos los servicios arrancan de forma correcta,
99. A continuación configuraremos nginx para que actúe de proxy inverso y podamos acceder a los servicios de node por el puerto 80 o 443.
100. Creamos fichero de configuración para acceder a nuestro servicio de node, lo llamaremos nodepopapi en sites-available
101. Ahora deberemos crear un acceso directo en sites-enabled con el comando  sudo ln -s /etc/nginx/sites-available/nodepopapi /etc/nginx/sites-anabled/nodepopapi.
102. Verificamos que hemos hecho bien la configuración y hacemos un reload de nginx
103. sudo nginx -t
104. sudo systemctl reload nginx
105. Comprobamos y funciona correctamente.
106. Editamos los ficheros de configuración de nginx para añadirles que todos los ficheros estáticos de nuestra apps de node nos los sirva nginx y les añada una cabecera personalizada.
107. Verificamos y funciona correctamente.
108. Ahora que tenemos funcionando nginx como proxy inverso, vamos a deshabilitar el puerto 3000 en nuestro firewall de AWS y verificamos que nuestra aplicación en node sigue funcionando por el puerto 80
109. Por último, vamos a implementar el https en todos los accesos a nuestra web.
110. Utilizaremos certificados gratuitos de letsencrypt y los gestionaremos de forma muy sencilla con certbot.
111. Instalamos certbot en nuestro servidor con el usuario ubuntu
112. sudo apt install certbot
113. sudo apt install python3-certbot-nginx
114. Una vez instalado, deberemos configurar certbot y decirle que trabaje con nginx
115. sudo certbot --nginx a partir de este punto deberemos contestar una serie de preguntas que condicionarán el funcionamiento de nginx y los certificados.

	- Nos pide el e-mail y lo ponemos. Es para casos de emergéncia.
	- Pide aceptar licencia. Pulsamos la tecla A
	- Pide e-mail para ceder datos y contestamos que N
	- Nos muestra todos los dominios configurados en nuestro nginx y pulsamos intro para aceptarlos todos.
	- Nos genera los certificados y nos da el succesfull.
	- Finalmente nos pregunta si todas las peticiones http las redirige hacia https y contestamos 2 (que es que si)
	- Congratulations!! Ya tenemos nuetro https funcionando con certificados totalmente válidos.
	
116. Por último, vamos a nuestro servidor en AWS y habilitamos el puerto 443 de entrada en el firewall.
117. Verificamos que todo esté funcionando correctamente y así es.
118. Damos la práctica por finalizada y funcionando.

[Volver al README](https://github.com/JosepCristobal/srv_confServWebX_PT#paso-a-paso)
