# Instalación de Servidor Web Nginx en Debian

Este documento describe los pasos para instalar y configurar el servidor web Nginx en una distribución Debian.

## Requisitos

- Una instalación de Debian con acceso a privilegios de superusuario (root) o un usuario con permisos `sudo`.
- Conexión a Internet para descargar los paquetes necesarios.

## Pasos de Instalación:

### 1. Actualizar los repositorios
Es importante actualizar los repositorios de paquetes para asegurarse de que estamos instalando la última versión disponible de Nginx.

Abre una terminal y ejecuta el siguiente comando:

```bash
sudo apt update
```

### 2. Instalar Nginx
Una vez que los repositorios estén actualizados, instala Nginx ejecutando el siguiente comando:

```bash
sudo apt install nginx
```

### 3. Verificar la instalación
Después de la instalación, podemos comprobar que el servidor Nginx está funcionando correctamente ejecutando el siguiente comando:

```bash
systemctl status nginx
```


## Ejercicio 2

### 1. Creación de la Carpeta del Sitio Web

Al igual que ocurre con Apache, en Nginx los archivos de un sitio web se organizan en carpetas. Estas carpetas suelen estar dentro de `/var/www`. Para crear la carpeta de tu sitio web o dominio, utiliza el siguiente comando:

```bash
sudo mkdir -p /var/www/servidor_web/html
```

### 2. Clonación del Repositorio de la Página Estática
A continuación, debes clonar un repositorio con una página web estática en la carpeta html que acabas de crear. Para ello, ejecuta los siguientes comandos dentro de la carpeta html:

```bash
cd /var/www/servidor_web/html
sudo git clone https://github.com/cloudacademy/static-website-example
```

### 3. Configuración de los Permisos Correctos
Para que Nginx pueda servir los archivos sin generar errores de acceso, necesitas cambiar la propiedad de los archivos al usuario que ejecuta el servicio de Nginx, que típicamente es www-data. Ejecuta el siguiente comando:

```bash
sudo chown -R www-data:www-data /var/www/servidor_web/html
```

Luego, debes configurar los permisos adecuados para permitir que el servidor web lea los archivos y los sirva a los usuarios:

```bash
sudo chmod -R 755 /var/www/servidor_web
```

### 4. Verificación de la Configuración del Servidor
Finalmente, para verificar que el servidor está sirviendo correctamente el sitio web, puedes acceder a él desde tu navegador introduciendo la dirección IP de tu máquina virtual o servidor:

```bash
http://IP-maq-virtual
```

### Paso 1: Instalación de vsftpd
Primero, actualiza la lista de paquetes disponibles y luego instala el servidor FTP vsftpd.

```bash
sudo apt-get update
sudo apt-get install vsftpd
```
Paso 2: Crear un directorio FTP
Ahora, crea una carpeta en tu directorio home donde se almacenarán los archivos accesibles por FTP.

```bash
mkdir /home/vagrant/ftp
```
Reemplaza nombre_usuario por tu nombre de usuario.

Paso 3: Generar certificados SSL para cifrado
El siguiente paso es crear los certificados SSL necesarios para habilitar la capa de cifrado en el servidor FTPS. Utilizaremos openssl para crear los certificados de forma que se cifren las conexiones FTP, similar a cómo funciona HTTPS.

```bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/vsftpd.key -out /etc/ssl/certs/vsftpd.crt
```

Paso 4: Configuración del archivo vsftpd.conf
A continuación, edita el archivo de configuración de vsftpd para habilitar SSL/TLS y configurar otros parámetros de seguridad.

Abre el archivo de configuración de vsftpd con tu editor de texto favorito. Aquí usamos nano:
```bash
sudo nano /etc/vsftpd.conf
```

Busca las siguientes líneas en el archivo de configuración y elimínalas completamente:
```bash
rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key
ssl_enable=NO
```

Agrega las siguientes líneas en su lugar para habilitar SSL/TLS y utilizar los certificados que acabamos de crear:
```bash
rsa_cert_file=/etc/ssl/certs/vsftpd.crt
rsa_private_key_file=/etc/ssl/private/vsftpd.key
ssl_enable=YES
allow_anon_ssl=NO
force_local_data_ssl=YES
force_local_logins_ssl=YES
ssl_tlsv1=YES
ssl_sslv2=NO
ssl_sslv3=NO
require_ssl_reuse=NO
ssl_ciphers=HIGH
local_root=/home/nombre_usuario/ftp
```

Guarda los cambios y cierra el editor.

Paso 5: Reiniciar el servicio vsftpd
Después de realizar los cambios, es necesario reiniciar el servicio de vsftpd para que se apliquen las nuevas configuraciones:

```bash
sudo systemctl restart vsftpd
```

### Ejercicio 5
Edita el archivo de configuración para habilitar FTPES:

```bash
sudo nano /etc/vsftpd.conf
```


Agrega o modifica las siguientes líneas:

conf
Copiar código
ssl_enable=YES
allow_anon_ssl=NO
force_local_data_ssl=YES
force_local_logins_ssl=YES
ssl_tlsv1=YES
ssl_sslv2=NO
ssl_sslv3=NO
rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key

Reinicia el servicio vsftpd:

```bash
sudo systemctl restart vsftpd
```

Transferir archivos mediante FileZilla
Abre FileZilla y sigue estos pasos:

Host: nginx
Protocolo: FTPES (FTP sobre TLS implícito)
Puerto: 21
Usuario: El usuario configurado en tu servidor Debian.
Contraseña: La contraseña asociada a dicho usuario.
Navega al directorio remoto /var/www/ y crea la carpeta new_web si aún no existe.

Sube todos los archivos de tu sitio desde tu máquina local a /var/www/new_web.

### Paso 3: Configurar el servidor web en Debian
Configurar el archivo de host virtual para el dominio nginx
Edita o crea el archivo de configuración en /etc/nginx/sites-available/nginx.conf:

```bash
sudo nano /etc/nginx/sites-available/nginx.conf
```

Agrega lo siguiente:

conf
Copiar código
server {
    listen 80;
    server_name nginx www.nginx;
    root /var/www/new_web;
    index index.html index.php;

    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log;

    location / {
        try_files $uri $uri/ =404;
    }
}

Habilitar el sitio y reiniciar Nginx

```bash
sudo ln -s /etc/nginx/sites-available/nginx.conf /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

### Paso 4: Configuración de permisos
Asignar el propietario y permisos correctos

```bash
sudo chown -R www-data:www-data /var/www/new_web
sudo chmod -R 755 /var/www/new_web
```