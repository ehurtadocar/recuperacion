# Manual de instalación de ownCloud con Apache2, MySQL y PHP

Este documento describe el proceso necesario para instalar y preparar ownCloud en una máquina Ubuntu 24.04, utilizando Apache como servidor web, MySQL como base de datos y PHP 7.4 como entorno de ejecución.

> **Recomendación**: Este proceso debe realizarse en una máquina virtual proporcionada por IsardVDI o en un contenedor con sistema Linux.

---

## 1. Preparar el entorno e instalar PHP 7.4

### 1.1 Instalar herramientas necesarias para repositorios PPA

```bash
sudo apt install software-properties-common -y
```

![Instalación software-properties](./img/4.png)

### 1.2 Añadir el repositorio oficial de PHP 7.4

```bash
LC_ALL=C.UTF-8 sudo add-apt-repository ppa:ondrej/php -y
```

![Añadir repositorio PHP](./img/5.png)

### 1.3 Actualizar la lista de paquetes

```bash
sudo apt update
```

![Actualizar paquetes](./img/6.png)

### 1.4 Instalar PHP 7.4

```bash
sudo apt install php7.4 -y
```

![Instalar PHP 7.4](./img/7.png)

### 1.5 Instalar el módulo de PHP para Apache

```bash
sudo apt install -y php libapache2-mod-php7.4
```

![PHP + módulo Apache](./img/8.png)

### 1.6 Instalar extensiones necesarias de PHP

```bash
sudo apt install -y php7.4-fpm php7.4-common php7.4-mbstring php7.4-xmlrpc php7.4-soap php7.4-gd php7.4-xml php7.4-intl php7.4-mysql php7.4-cli php7.4-ldap php7.4-zip php7.4-curl
```

![Extensiones de PHP](./img/10.png)

### 1.7 Seleccionar la versión por defecto de PHP

```bash
sudo update-alternatives --config php
```

![Seleccionar PHP por defecto](./img/1.png)

### 1.8 Activar módulos necesarios en Apache

```bash
sudo a2enmod proxy_fcgi setenvif
sudo a2enconf php7.4-fpm
```

![Activar módulos Apache](./img/9.png)

### 1.9 Reiniciar Apache2

```bash
sudo service apache2 restart
```

![Reiniciar Apache](./img/13.png)

---

## 2. Instalación de ownCloud y configuración base

### 2.1 Actualizar el sistema

```bash
sudo apt update
sudo apt upgrade
```

![Actualizar sistema](./img/12.png)

### 2.2 Instalar Apache2

```bash
sudo apt install -y apache2
```

![Instalar Apache](./img/16.png)

### 2.3 Instalar MySQL Server

```bash
sudo apt install -y mysql-server
```

![Instalar MySQL](./img/11.png)

### 2.4 Acceder a la consola de MySQL

```bash
sudo mysql
```

### 2.5 Crear base de datos y usuario

```sql
CREATE DATABASE bbdd;
CREATE USER 'usuario'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
GRANT ALL ON bbdd.* TO 'usuario'@'localhost';
exit;
```

![Crear base de datos](./img/21.png)  
![Crear usuario](./img/22.png)  
![Dar permisos](./img/23.png)

### 2.6 Verificar conexión con el nuevo usuario

```bash
mysql -u usuario -p
```

![Verificar conexión](./img/26.png)

---

## 3. Desplegar ownCloud en el servidor web

### 3.1 Descargar ownCloud

Descarga el archivo `.zip` desde la web oficial y guárdalo en tu carpeta de descargas:

[https://download.owncloud.com/server/stable/owncloud-complete-20240724.zip](https://download.owncloud.com/server/stable/owncloud-complete-20240724.zip)

![Descargar ownCloud](./img/3.png)

### 3.2 Mover y descomprimir el archivo

```bash
sudo cp ~/Descargas/owncloud-complete-20240724.zip /var/www/html
cd /var/www/html
sudo unzip owncloud-complete-20240724.zip
sudo cp -R owncloud/. /var/www/html
sudo rm -rf owncloud/
sudo rm -rf index.html
```

![Copiar y descomprimir](./img/28.png)  
![Mover contenido](./img/29.png)  
![Eliminar index.html](./img/31.png)

### 3.3 Aplicar permisos a los archivos

```bash
cd /var/www/html
sudo chmod -R 775 .
sudo chown -R usuario:www-data .
```

![Aplicar permisos](./img/32.png)

---

## 4. Acceder a ownCloud desde el navegador

Abre el navegador y accede a la siguiente dirección:

```
http://localhost
```

![Formulario instalación](./img/25.png)

Rellena el formulario con los siguientes datos:

- **Usuario administrador**: (el que tú elijas)
- **Contraseña**: (elige una segura)
- **Usuario de la base de datos**: `usuario`
- **Contraseña de la base de datos**: `password`
- **Nombre de la base de datos**: `bbdd`
- **Servidor**: `localhost`

---

## ✅ Instalación finalizada

Si todo ha salido correctamente, podrás acceder a ownCloud desde tu navegador. A partir de aquí puedes comenzar a gestionar archivos o continuar con la configuración de usuarios y permisos.
