# 🌐 Administración de Red en Linux

## 📌 Introducción

La administración de red es una de las tareas principales de un administrador Linux.

En este documento se explican las operaciones básicas para configurar y diagnosticar conexiones de red en **Ubuntu Server 24.04 LTS**.

Se trabajarán conceptos como:

- Interfaces de red.
- Direcciones IP.
- Configuración con Netplan.
- DNS.
- Puertas de enlace.
- Rutas.
- Herramientas de diagnóstico.

---

# 🖥️ Conceptos básicos de red

Una configuración de red básica está formada por:

| Elemento | Descripción |
|---|---|
| IP | Dirección del equipo dentro de la red |
| Máscara | Define el rango de la red |
| Gateway | Puerta de salida hacia otras redes |
| DNS | Traduce nombres de dominio a IP |
| Interfaz | Tarjeta de red física o virtual |

Ejemplo:

```
IP:
192.168.1.50

Máscara:
255.255.255.0

Gateway:
192.168.1.1

DNS:
8.8.8.8
```

---

# 🔍 Identificar interfaces de red

Mostrar interfaces disponibles:

```bash
ip link
```

Ejemplo:

```
enp0s3
lo
```

---

Mostrar información completa de red:

```bash
ip addr
```

Ejemplo:

```
2: enp0s3:
inet 192.168.1.50/24
```

---

# 📡 Consultar dirección IP

Mostrar IP actual:

```bash
ip address
```

Forma corta:

```bash
ip a
```

Ejemplo:

```
192.168.1.50/24
```

---

# 🌐 Configuración de red con Netplan

Ubuntu Server utiliza **Netplan** para gestionar la configuración de red.

Los archivos se encuentran en:

```bash
/etc/netplan/
```

Consultar archivos disponibles:

```bash
ls /etc/netplan/
```

Ejemplo:

```
50-cloud-init.yaml
```

---

# ⚙️ Configuración IP dinámica (DHCP)

Ejemplo de configuración DHCP:

```yaml
network:
  version: 2
  ethernets:
    enp0s3:
      dhcp4: true
```

Aplicar cambios:

```bash
sudo netplan apply
```

---

# 🔒 Configuración IP estática

Ejemplo:

```yaml
network:
  version: 2
  ethernets:
    enp0s3:
      addresses:
        - 192.168.1.50/24
      routes:
        - to: default
          via: 192.168.1.1
      nameservers:
        addresses:
          - 8.8.8.8
          - 1.1.1.1
```

Aplicar configuración:

```bash
sudo netplan apply
```

---

# 🧪 Probar configuración Netplan

Antes de aplicar cambios:

```bash
sudo netplan try
```

Permite probar la configuración y volver atrás si falla.

---

# 🚪 Puerta de enlace (Gateway)

Consultar rutas actuales:

```bash
ip route
```

Ejemplo:

```
default via 192.168.1.1 dev enp0s3
```

La línea:

```
default via
```

indica la salida hacia Internet.

---

# 🌍 Configuración DNS

Consultar servidores DNS:

```bash
resolvectl status
```

Ejemplo:

```
DNS Servers:
8.8.8.8
1.1.1.1
```

---

# 🔎 Resolver nombres DNS

Probar resolución:

```bash
nslookup google.com
```

Ejemplo:

```
Name:
google.com

Address:
142.250.x.x
```

Otra herramienta:

```bash
dig google.com
```

---

# 🧭 Tabla de rutas

Mostrar rutas:

```bash
ip route show
```

Ejemplo:

```
default via 192.168.1.1
192.168.1.0/24 dev enp0s3
```

---

# ➕ Añadir una ruta temporal

Ejemplo:

```bash
sudo ip route add 10.0.0.0/24 via 192.168.1.1
```

Verificar:

```bash
ip route
```

---

# ❌ Eliminar una ruta

```bash
sudo ip route delete 10.0.0.0/24
```

---

# 🔌 Herramientas de diagnóstico

## Ping

Comprobar conectividad:

```bash
ping google.com
```

Ejemplo:

```
64 bytes from google.com
```

Detener:

```
CTRL + C
```

---

## Traceroute

Muestra el camino hasta un destino:

Instalar:

```bash
sudo apt install traceroute
```

Uso:

```bash
traceroute google.com
```

---

## Ver puertos abiertos

Herramienta moderna:

```bash
ss -tulnp
```

Ejemplo:

```
LISTEN 0 128 0.0.0.0:22
```

Indica que SSH está escuchando.

---

# 📋 Información de red avanzada

Ver conexiones activas:

```bash
ss -tunap
```

---

Mostrar tabla ARP:

```bash
ip neigh
```

---

Ver estadísticas de interfaz:

```bash
ip -s link
```

---

# 🔥 Firewall y red

Consultar reglas UFW:

```bash
sudo ufw status verbose
```

Permitir un puerto:

```bash
sudo ufw allow 80/tcp
```

Ejemplo:

Permitir servidor web:

```bash
sudo ufw allow http
```

---

# 📦 Instalación de herramientas de red

Instalar herramientas habituales:

```bash
sudo apt install -y \
net-tools \
dnsutils \
traceroute \
nmap
```

---

# 🔍 Nmap básico

Escanear un equipo:

```bash
nmap 192.168.1.50
```

Escanear una red:

```bash
nmap 192.168.1.0/24
```

---

# 🛠️ Resolución de problemas

## No tengo IP

Comprobar interfaz:

```bash
ip a
```

Reiniciar red:

```bash
sudo netplan apply
```

---

## No tengo Internet

Comprobar gateway:

```bash
ip route
```

Probar IP externa:

```bash
ping 8.8.8.8
```

Si funciona, revisar DNS:

```bash
ping google.com
```

---

## No resuelve nombres DNS

Comprobar DNS:

```bash
resolvectl status
```

Probar:

```bash
nslookup google.com
```

---

# 🧪 Ejercicio práctico

Configurar una IP estática:

Datos:

```
IP: 192.168.1.100

Gateway: 192.168.1.1

DNS: 8.8.8.8
```

Editar Netplan:

```bash
sudo nano /etc/netplan/01-network.yaml
```

Aplicar:

```bash
sudo netplan try
```

Confirmar:

```bash
ip a
```

Probar conexión:

```bash
ping 8.8.8.8
```

---

# ✅ Checklist final

- [x] Identificar interfaces de red
- [x] Configurar DHCP
- [x] Configurar IP estática
- [x] Gestionar Netplan
- [x] Configurar DNS
- [x] Consultar rutas
- [x] Diagnosticar problemas de red
- [x] Usar herramientas de red

---
- Automatización de tareas de red con Bash.
