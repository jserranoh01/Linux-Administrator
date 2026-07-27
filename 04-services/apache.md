# 🌐 Servidor Web Apache en Ubuntu Server

## 📌 Introducción

Apache HTTP Server es uno de los servidores web más utilizados en Internet.

Permite publicar páginas web, aplicaciones y servicios mediante el protocolo HTTP/HTTPS.

En este documento se explica la instalación, configuración y administración básica de **Apache en Ubuntu Server 24.04 LTS**.

---

# 📦 Instalación de Apache

Actualizar repositorios:

```bash
sudo apt update
```

Instalar Apache:

```bash
sudo apt install apache2 -y
```

Comprobar versión instalada:

```bash
apache2 -v
```

Ejemplo:

```
Server version: Apache/2.4.xx
```

---

# ⚙️ Gestión del servicio Apache con Systemd

## Consultar estado del servicio

```bash
systemctl status apache2
```

Resultado esperado:

```
active (running)
```

---

## Iniciar Apache

```bash
sudo systemctl start apache2
```

---

## Detener Apache

```bash
sudo systemctl stop apache2
```

---

## Reiniciar Apache

```bash
sudo systemctl restart apache2
```

---

## Recargar configuración sin detener el servicio

```bash
sudo systemctl reload apache2
```

---

## Activar inicio automático

```bash
sudo systemctl enable apache2
```

Comprobar:

```bash
systemctl is-enabled apache2
```

Resultado:

```
enabled
```

---

# 🌍 Primera prueba del servidor web

Desde el propio servidor:

```bash
curl localhost
```

Desde otro equipo:

```
http://IP_DEL_SERVIDOR
```

Ejemplo:

```
http://192.168.1.50
```

Debe aparecer la página por defecto de Apache.

---

# 🔥 Configuración del Firewall

Permitir tráfico HTTP:

```bash
sudo ufw allow http
```

Permitir HTTPS:

```bash
sudo ufw allow https
```

También se puede utilizar:

```bash
sudo ufw allow 'Apache Full'
```

Comprobar reglas:

```bash
sudo ufw status
```

Ejemplo:

```
80/tcp   ALLOW
443/tcp  ALLOW
```

---

# 📁 Estructura de directorios de Apache

Archivos principales:

```
/etc/apache2/
```

Contenido:

```
apache2.conf
ports.conf
sites-available/
sites-enabled/
mods-available/
mods-enabled/
conf-available/
conf-enabled/
```

---

# 📄 Directorio raíz web

Por defecto Apache utiliza:

```
/var/www/html
```

Archivo principal:

```
index.html
```

Ver contenido:

```bash
cat /var/www/html
```

---

# ✏️ Crear una página web básica

Editar página principal:

```bash
sudo nano /var/www/html/index.html
```

Ejemplo:

```html
<!DOCTYPE html>
<html>
<head>
<title>Servidor Apache</title>
</head>

<body>

<h1>Servidor Apache funcionando</h1>

<p>Ubuntu Server 24.04 LTS</p>

</body>
</html>
```

Guardar y acceder:

```
http://IP_SERVIDOR
```

---

# 👤 Permisos del directorio web

Cambiar propietario:

```bash
sudo chown -R www-data:www-data /var/www/html
```

Permisos recomendados:

```bash
sudo chmod -R 755 /var/www/html
```

---

# 🏠 Virtual Hosts

Los Virtual Hosts permiten alojar múltiples páginas web en un mismo servidor.

Ejemplo:

```
web1.local
web2.local
```

Cada sitio tendrá su propia configuración.

---

# 📂 Crear un nuevo sitio web

Crear directorio:

```bash
sudo mkdir -p /var/www/jose
```

Crear página:

```bash
sudo nano /var/www/misitio/index.html
```

Ejemplo:

```html
<h1>Mi primer Virtual Host Apache</h1>
```

---

# ⚙️ Crear configuración del Virtual Host

Crear archivo:

```bash
sudo nano /etc/apache2/sites-available/jose.conf
```

Contenido:

```apache
<VirtualHost *:80>

    ServerAdmin admin@jose.local
    ServerName jose.local

    DocumentRoot /var/www/jose

    ErrorLog ${APACHE_LOG_DIR}/jose_error.log
    CustomLog ${APACHE_LOG_DIR}/jose_access.log combined

</VirtualHost>
```

---

# ✅ Activar Virtual Host

Activar sitio:

```bash
sudo a2ensite jose.conf
```

Desactivar sitio:

```bash
sudo a2dissite jose.conf
```

Comprobar configuración:

```bash
sudo apachectl configtest
```

Resultado esperado:

```
Syntax OK
```

Aplicar cambios:

```bash
sudo systemctl reload apache2
```

---

# 📝 Configurar archivo hosts local

Para pruebas internas:

Editar:

```bash
sudo nano /etc/hosts
```

Añadir:

```
192.168.1.50 jose.local
```

Ahora acceder:

```
http://jose.local
```

---

# 🔐 Habilitar HTTPS con SSL

Instalar Certbot:

```bash
sudo apt install certbot python3-certbot-apache -y
```

Obtener certificado:

```bash
sudo certbot --apache
```

Comprobar certificados:

```bash
sudo certbot certificates
```

---

# 📊 Logs de Apache

Los registros se encuentran en:

```
/var/log/apache2/
```

---

## Log de accesos

```bash
tail -f /var/log/apache2/access.log
```

---

## Log de errores

```bash
tail -f /var/log/apache2/error.log
```

---

# 🔍 Módulos Apache

Ver módulos activos:

```bash
apache2ctl -M
```

---

Activar módulo:

```bash
sudo a2enmod modulo
```

Ejemplo:

```bash
sudo a2enmod rewrite
```

Reiniciar:

```bash
sudo systemctl restart apache2
```

---

# 🛠️ Resolución de problemas

## Apache no inicia

Comprobar configuración:

```bash
sudo apachectl configtest
```

Consultar errores:

```bash
journalctl -xeu apache2
```

---

## Puerto ocupado

Comprobar puertos:

```bash
sudo ss -tulnp
```

Apache utiliza normalmente:

```
80 HTTP
443 HTTPS
```

---

## Página no encontrada

Comprobar:

```bash
ls -la /var/www/html
```

Verificar permisos:

```bash
ls -ld /var/www/html
```

---

# 🧪 Ejercicio práctico

Instalar Apache:

```bash
sudo apt install apache2 -y
```

Crear página personalizada:

```bash
sudo nano /var/www/html/index.html
```

Abrir firewall:

```bash
sudo ufw allow 'Apache Full'
```

Comprobar servicio:

```bash
systemctl status apache2
```

Acceder desde otro equipo:

```
http://IP_SERVIDOR
```

---

# ✅ Checklist final

- [x] Apache instalado
- [x] Servicio gestionado con Systemd
- [x] Firewall configurado
- [x] Página web creada
- [x] Virtual Hosts configurados
- [x] Logs revisados
- [x] HTTPS configurado
- [x] Módulos administrados

---
- Seguridad avanzada Apache.
- Monitorización del servidor.
