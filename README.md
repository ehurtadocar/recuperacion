# Manual de instalación de ownCloud con Apache2, MySQL y PHP

Este documento describe el proceso necesario para instalar y preparar ownCloud en una máquina Ubuntu 24.04, utilizando Apache como servidor web, MySQL como base de datos y PHP 7.4 como entorno de ejecución.

> **Recomendación**: Este proceso debe realizarse en una máquina virtual proporcionada por IsardVDI o en un contenedor con sistema Linux.

---

## 1. Preparar el entorno e instalar PHP 7.4

### 1.1 Instalar herramientas necesarias para repositorios PPA

```bash
sudo apt install software-properties-common -y
```
![4](https://github.com/user-attachments/assets/32d9a52d-a262-4726-b7f9-b5ae58f6badf)


### 1.2 Añadir el repositorio oficial de PHP 7.4

```bash
LC_ALL=C.UTF-8 sudo add-apt-repository ppa:ondrej/php -y
```

![5](https://github.com/user-attachments/assets/840a4a0d-e636-4bbd-acc9-b6a5bb88c65f)


### 1.3 Actualizar la lista de paquetes

```bash
sudo apt update
```

![6](https://github.com/user-attachments/assets/77099384-b927-4a33-8ec1-b9e14c975885)


### 1.4 Instalar PHP 7.4

```bash
sudo apt install php7.4 -y
```

![7](https://github.com/user-attachments/assets/456cbdc5-2d04-4466-82ae-1fe0839b7932)


### 1.5 Instalar el módulo de PHP para Apache

```bash
sudo apt install -y php libapache2-mod-php7.4
```

![8](https://github.com/user-attachments/assets/07fff51e-5df7-4b94-ba51-0626f39a4429)


### 1.6 Instalar extensiones necesarias de PHP

```bash
sudo apt install -y php7.4-fpm php7.4-common php7.4-mbstring php7.4-xmlrpc php7.4-soap php7.4-gd php7.4-xml php7.4-intl php7.4-mysql php7.4-cli php7.4-ldap php7.4-zip php7.4-curl
```

![10](https://github.com/user-attachments/assets/ee9a1f5a-9760-4284-bbc2-debae0ce4b85)


### 1.7 Seleccionar la versión por defecto de PHP

```bash
sudo update-alternatives --config php
```

![1](https://github.com/user-attachments/assets/6cab0580-06b9-470b-92c5-55e790770adb)


### 1.8 Activar módulos necesarios en Apache

```bash
sudo a2enmod proxy_fcgi setenvif
sudo a2enconf php7.4-fpm
```

![9](https://github.com/user-attachments/assets/ff8f8f97-aaa5-46ac-abb6-b673d6abb6e8)


### 1.9 Reiniciar Apache2

```bash
sudo service apache2 restart
```

![13](https://github.com/user-attachments/assets/9e24547b-a724-4ef4-8f4f-79e4acfc202f)


---

## 2. Instalación de ownCloud y configuración base

### 2.1 Actualizar el sistema

```bash
sudo apt update
sudo apt upgrade
```

![12](https://github.com/user-attachments/assets/a65e155d-ffc0-4c8f-bbd6-6e1b6d9a5af4)


### 2.2 Instalar Apache2

```bash
sudo apt install -y apache2
```

![16](https://github.com/user-attachments/assets/2e6aefc7-b44b-4c5c-abe6-4e65e1990173)


### 2.3 Instalar MySQL Server

```bash
sudo apt install -y mysql-server
```

![11](https://github.com/user-attachments/assets/4bff5a6f-f1c1-4227-b4d6-d3f42e3fcda8)


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

![21](https://github.com/user-attachments/assets/04aa8c8b-ef82-4bea-8fe0-478dbf334464)
![22](https://github.com/user-attachments/assets/ef32ae53-dc52-4a2f-b791-e2eec33ccfbe)
![23](https://github.com/user-attachments/assets/d7d0fecb-7b2a-4147-a992-3e60437e1aca)

### 2.6 Verificar conexión con el nuevo usuario

```bash
mysql -u usuario -p
```

![26](https://github.com/user-attachments/assets/f7297dd3-7330-4ce1-a2b8-3117d2db3ea1)


---

## 3. Desplegar ownCloud en el servidor web

### 3.1 Descargar ownCloud

Descarga el archivo `.zip` desde la web oficial y guárdalo en tu carpeta de descargas:

[https://download.owncloud.com/server/stable/owncloud-complete-20240724.zip](https://download.owncloud.com/server/stable/owncloud-complete-20240724.zip)

![3](https://github.com/user-attachments/assets/7a2b0835-cac1-4ef8-9fc2-b1b58b9cfe73)


### 3.2 Mover y descomprimir el archivo

```bash
sudo cp ~/Descargas/owncloud-complete-20240724.zip /var/www/html
cd /var/www/html
sudo unzip owncloud-complete-20240724.zip
sudo cp -R owncloud/. /var/www/html
sudo rm -rf owncloud/
sudo rm -rf index.html
```

![28](https://github.com/user-attachments/assets/c7635956-fa1d-4997-bcfb-6bff31b0b2ab)
![29](https://github.com/user-attachments/assets/7ca6f8de-5b47-4d33-b8fa-208a3f1c9a10)
![31](https://github.com/user-attachments/assets/3e1256cb-de2b-42a4-9293-dd51833aac60)


### 3.3 Aplicar permisos a los archivos

```bash
cd /var/www/html
sudo chmod -R 775 .
sudo chown -R usuario:www-data .
```

![32](https://github.com/user-attachments/assets/273d6458-caaf-48e4-80d6-f32eae4df614)


---

## 4. Acceder a ownCloud desde el navegador

Abre el navegador y accede a la siguiente dirección:

```
http://localhost
```

![25](https://github.com/user-attachments/assets/f48c92b3-2cac-4058-8c57-f295b2779a91)

Rellena el formulario con los siguientes datos:

- **Usuario administrador**: (el que tú elijas)
- **Contraseña**: (elige una segura)
- **Usuario de la base de datos**: `usuario`
- **Contraseña de la base de datos**: `password`
- **Nombre de la base de datos**: `bbdd`
- **Servidor**: `localhost`
