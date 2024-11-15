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

```
bash
sudo apt install nginx
```

### 3. Verificar la instalación
Después de la instalación, podemos comprobar que el servidor Nginx está funcionando correctamente ejecutando el siguiente comando:

```
bash
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

```
bash
cd /var/www/servidor_web/html
sudo git clone https://github.com/cloudacademy/static-website-example
```

### 3. Configuración de los Permisos Correctos
Para que Nginx pueda servir los archivos sin generar errores de acceso, necesitas cambiar la propiedad de los archivos al usuario que ejecuta el servicio de Nginx, que típicamente es www-data. Ejecuta el siguiente comando:

```
bash
sudo chown -R www-data:www-data /var/www/servidor_web/html
```

Luego, debes configurar los permisos adecuados para permitir que el servidor web lea los archivos y los sirva a los usuarios:

```
bash
sudo chmod -R 755 /var/www/servidor_web
```

### 4. Verificación de la Configuración del Servidor
Finalmente, para verificar que el servidor está sirviendo correctamente el sitio web, puedes acceder a él desde tu navegador introduciendo la dirección IP de tu máquina virtual o servidor:

```
bash
http://IP-maq-virtual
```
