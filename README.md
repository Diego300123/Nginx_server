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