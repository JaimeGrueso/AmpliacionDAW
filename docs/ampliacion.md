# Configuración de un Servidor Nginx con Hosts Virtuales y Directorios de Usuario

# Conexion SSH
Para esta páctica trabajaremos desde nuestra propia máquina anfitriona y nos conectaremos 
a nuestra máquina virtual de Debian mediante SSH. Para ello, abrimos una terminal y ejecutamos:

![Conexion SSH](./assets/images/screenshot.3.jpg)

# Creacion de Usuarios
Para crear los usuarios en Debian, utilizamos el comando `useradd` seguido del nombre del usuario. Como se creará un directorio, se ejecutará el comando con la opción -m y -s para asignarle un directorio a cada usuario y un shell.

![Creacion de Usuarios 1](./assets/images/screenshot.25.jpg)


# Creación de las Carpetas public_html
Para la creación de las carpetas `public_html`, nos cambiaremos de usuario para poder crear dichas carpetas. Para ello, utilizamos el comando `su` seguido del nombre del usuario y `mkdir` para crear la carpeta.

![Creacion de Carpetas public_html 1](./assets/images/screenshot.7.jpg)

# Asignación de Permisos
Para asignar los permisos a las carpetas `public_html`, se cambiarán las ACLs de la carpeta `public_html` para que Nginx no tenga problemas para leer, escribir y ejecutar en la carpeta.

Este comando ``setfacl`` se utiliza para configurar las Listas de Control de Acceso (ACL) en el directorio ``public_html``. Específicamente:

``-d``: Establece las ACL predeterminadas (default) para el directorio4.

``-R``: Aplica los cambios de forma recursiva a todos los subdirectorios y archivos dentro de public_html3.

``-m``: Modifica las ACL existentes sin eliminar las anteriores5.

``u:www-data:rw``: Otorga permisos de lectura (r) y escritura (w) al usuario www-data


![Asignacion de Permisos 2](./assets/images/screenshot.9.jpg)

![Asignacion de Permisos 2](./assets/images/screenshot.10.jpg)


# Creación de Páginas Web Estáticas
Se crearán dos archivos index.html, uno para cada usuario en las carpetas `public_html` de cada usuario. Esto servirá para comprobar que todo está funcionando correctamente.

![Creacion de Paginas Web Estaticas 1](./assets/images/screenshot.11.jpg)

![Creacion de Paginas Web Estaticas 2](./assets/images/screenshot.12.jpg)

# Configuración de Nginx
Para instalar Nginx, utilizamos el comando `apt-get install nginx` y para comprobar que se ha instalado correctamente, utilizamos el comando `systemctl status nginx`. En mi caso ya tenía instalado Nginx, por lo que no fue necesario instalarlo. Se ejecutará el comando `systemctl status nginx` para comprobar que el servicio está activo.

![Configuracion de Nginx 1](./assets/images/screenshot.13.jpg)

![Configuracion de Nginx 2](./assets/images/screenshot.14.jpg)

## Generación de Certificados SSL
Para generar los certificados SSL, utilizamos el comando `sudo openssl req -x509 -newkey rsa:4096 -keyout /etc/ssl/private/calistoweb.pem -out /etc/ssl/certs/calisto.pem -sha256 -days 365 --nodes`. Se nos pedirán una serie de datos que deberemos rellenar. Lo mismo se debreá hacer para el usuario `melibea`.	

![Generacion de Certificados SSL 1](./assets/images/screenshot.15.jpg)
![Generacion de Certificados SSL 2](./assets/images/screenshot.16.jpg)

## Configuración de los Hosts Virtuales
Para configurar los hosts virtuales, se creará un archivo de configuración en la carpeta `/etc/nginx/sites-available` para cada usuario. En mi caso, he creado los archivos `calisto` y `melibea`. En estos archivos se configurarán los hosts virtuales para cada usuario.

![Configuracion de los Hosts Virtuales 1](./assets/images/screenshot.17.jpg)

![Configuracion de los Hosts Virtuales 2](./assets/images/screenshot.18.jpg)

Hecho esto, se habilitarán los hosts virtuales con el comando `ln -s /etc/nginx/sites-available/calistoweb /etc/nginx/sites-enabled/` y `ln -s /etc/nginx/sites-available/melibeaweb /etc/nginx/sites-enabled/`. Al ejecitar estos comandos, se crearán enlaces simbólicos en la carpeta `/etc/nginx/sites-enabled/` que apuntarán a los archivos de configuración de los hosts virtuales, como se puede apreciar en la siguiente imagen.

Se ejecuta el comando `nginx -t` para comprobar que la configuración de los hosts virtuales es correcta y reiniciamos el servicio de Nginx con el comando `systemctl restart nginx`.

![Configuracion de los Hosts Virtuales 3](./assets/images/screenshot.19.jpg)

## Comprobación de los Hosts Virtuales
Se añadirán las direcciones IP y los nombres de los hosts virtuales en el archivo `C:\Windows\System32\drivers\etc\hosts` de nuestra máquina anfitriona.

![Comprobacion de los Hosts Virtuales 1](./assets/images/screenshot.20.jpg)

## Comprobación de Funcionamiento de los Hosts Virtuales
Se intentará acceder a las páginas web de los usuarios `calisto` y `melibea` desde un navegador web. Para ello, se abrirá un navegador web y se introducirán las direcciones `https://calisto` y `https://melibea`.

![Comprobacion de Funcionamiento de los Hosts Virtuales 1](./assets/images/screenshot.21.jpg)

![Comprobacion de Funcionamiento de los Hosts Virtuales 2](./assets/images/screenshot.24.jpg)

Al continuar y ver el certificado de seguridad, se podrá comprobar que el certificado SSL es válido.

![Comprobacion de Funcionamiento de los Hosts Virtuales 3](./assets/images/screenshot.22.jpg)

![Comprobacion de Funcionamiento de los Hosts Virtuales 4](./assets/images/screenshot.23.jpg)

Para finalizar, se clickará en el botón `Aceptar el riesgo y continuar` para acceder a la página web. Y si todo ha ido bien, se podrá ver la página web del usuario `calisto` y `melibea`.

![Comprobacion de Funcionamiento de los Hosts Virtuales 5](./assets/images/screenshot.26.jpg)

![Comprobacion de Funcionamiento de los Hosts Virtuales 6](./assets/images/screenshot.27.jpg)

Con esto concluye la práctica de configuración de un servidor Nginx con hosts virtuales y directorios de usuario.


