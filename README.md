# Manual de instalación de ownCloud con Apache2, MySQL y PHP

Este documento describe el proceso necesario para instalar y preparar ownCloud en una máquina Ubuntu 24.04, utilizando Apache como servidor web, MySQL como base de datos y PHP 7.4 como entorno de ejecución.

> **Recomendación**: Este proceso debe realizarse en una máquina virtual proporcionada por IsardVDI o en un contenedor con sistema Linux.

---

## 1. Preparar el entorno e instalar PHP 7.4

### 1.1 Instalar herramientas necesarias para repositorios PPA

```bash
sudo apt install software-properties-common -y
```
![2](https://github.com/user-attachments/assets/6ce9091d-0a62-44bd-ab02-a5de30f5d3ae)


### 1.2 Añadir el repositorio oficial de PHP 7.4

```bash
LC_ALL=C.UTF-8 sudo add-apt-repository ppa:ondrej/php -y
```
![3](https://github.com/user-attachments/assets/c5ae0eff-d2b4-4785-99d4-d527ed066135)


### 1.3 Actualizar la lista de paquetes

```bash
sudo apt update
```
![4](https://github.com/user-attachments/assets/c27de53e-20ee-4edd-9e3f-b201f0cf169a)


### 1.4 Instalar PHP 7.4

```bash
sudo apt install php7.4 -y
```
![5](https://github.com/user-attachments/assets/6d738bd7-126d-4199-9490-a1fbdaeb816c)


### 1.5 Instalar el módulo de PHP para Apache

```bash
sudo apt install -y php libapache2-mod-php7.4
```
![6](https://github.com/user-attachments/assets/c6612ce2-d1e6-424f-9b14-9bb614dcd8b5)


### 1.6 Instalar extensiones necesarias de PHP

```bash
sudo apt install -y php7.4-fpm php7.4-common php7.4-mbstring php7.4-xmlrpc php7.4-soap php7.4-gd php7.4-xml php7.4-intl php7.4-mysql php7.4-cli php7.4-ldap php7.4-zip php7.4-curl
```
![7](https://github.com/user-attachments/assets/50819bd3-70d1-4d11-bbb4-fd0863cce18b)


### 1.7 Seleccionar la versión por defecto de PHP

```bash
sudo update-alternatives --config php
```
![8](https://github.com/user-attachments/assets/d5f3d161-9691-457a-9c13-29d18cfb9b86)


### 1.8 Activar módulos necesarios en Apache

```bash
sudo a2enmod proxy_fcgi setenvif
sudo a2enconf php7.4-fpm
```
![9](https://github.com/user-attachments/assets/161a3ec8-122c-487c-bc70-5198c61d042e)


### 1.9 Reiniciar Apache2

```bash
sudo service apache2 restart
```
![10](https://github.com/user-attachments/assets/a48d99c6-d8d8-4524-bd52-328a8d2e6704)


---

## 2. Instalación de ownCloud y configuración base

### 2.1 Actualizar el sistema

```bash
sudo apt update
sudo apt upgrade
```
![11](https://github.com/user-attachments/assets/a7f141f9-ea09-474d-b4eb-b518599b945d)


### 2.2 Instalar Apache2

```bash
sudo apt install -y apache2
```

![12](https://github.com/user-attachments/assets/563d8238-7576-4871-a124-7e3df67a0310)

### 2.3 Instalar MySQL Server

```bash
sudo apt install -y mysql-server
```

![13](https://github.com/user-attachments/assets/e5582eb4-7942-4c97-9612-e14a755af74e)

### 2.4 Acceder a la consola de MySQL

```bash
sudo mysql
```
![14](https://github.com/user-attachments/assets/0d3de1c3-fe05-47c1-a690-9f8edb28cdba)


### 2.5 Crear base de datos y usuario

```sql
CREATE DATABASE bbdd;
CREATE USER 'usuario'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
GRANT ALL ON bbdd.* TO 'usuario'@'localhost';
exit;
```
![15](https://github.com/user-attachments/assets/50222e22-2012-4bb1-a89c-f9a3226e5327)


### 2.6 Verificar conexión con el nuevo usuario

```bash
mysql -u usuario -p
```
![16](https://github.com/user-attachments/assets/c59f1a39-8703-44d4-a2bb-392b3aebdb6a)


---

## 3. Desplegar ownCloud en el servidor web

### 3.1 Descargar ownCloud

Descarga el archivo `.zip` desde la web oficial:

[https://download.owncloud.com/server/stable/owncloud-complete-20240724.zip](https://download.owncloud.com/server/stable/owncloud-complete-20240724.zip)

![1](https://github.com/user-attachments/assets/62fe8519-8971-4521-a2ed-92c42d428059)



### 3.2 Mover y descomprimir el archivo

```bash
sudo cp ~/Descargas/owncloud-complete-20240724.zip /var/www/html
cd /var/www/html
sudo unzip owncloud-complete-20240724.zip
sudo cp -R owncloud/. /var/www/html
sudo rm -rf owncloud/
sudo rm -rf index.html
```
![17](https://github.com/user-attachments/assets/680ab2d7-2b7d-4f89-abc2-6b1620109197)


### 3.3 Aplicar permisos a los archivos

```bash
cd /var/www/html
sudo chmod -R 775 .
sudo chown -R usuario:www-data .
```
![18](https://github.com/user-attachments/assets/b164eb2a-e632-4e7b-acec-00b16b8d52fb)


---

## 4. Acceder a ownCloud desde el navegador

Abre el navegador y accede a la siguiente dirección:

```
http://localhost
```

![19](https://github.com/user-attachments/assets/47f4d10d-de0a-43db-b0fa-6f6827484c72)


Rellena el formulario con los siguientes datos:

- **Usuario administrador**: (el que tú elijas)
- **Contraseña**: (elige una segura)
- **Usuario de la base de datos**: `usuario`
- **Contraseña de la base de datos**: `password`
- **Nombre de la base de datos**: `bbdd`
- **Servidor**: `localhost`

![20](https://github.com/user-attachments/assets/1cd0b3e2-bf30-4303-8977-baa4c550bc7b)
---

## 1. Configuracion de owncloud

Ahora nuestro objetivo va a ser crear una carpeta

Para esto nos iremos a "All files" y haremos click en en simbolo "+".
![image](https://github.com/user-attachments/assets/0f23ba8c-1d87-495c-9f87-81fb477a00cd)

Eligiremos "Folder".
![image](https://github.com/user-attachments/assets/df846d87-ee04-465b-bf3d-dfdcb2064f8a)

Ponemos nombre a nuestra carpeta y le damos a "create"
![image](https://github.com/user-attachments/assets/82d3220f-5c21-4790-b965-89d39242c4f6)



