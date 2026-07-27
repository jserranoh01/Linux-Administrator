# 🐧 Instalación de Ubuntu Server 24.04 LTS

## 📌 Introducción

En este documento se describe el proceso de instalación y configuración inicial de **Ubuntu Server 24.04 LTS**.

El objetivo es crear una base limpia para un servidor Linux preparado para tareas de administración, servicios de red y prácticas de SysAdmin.

---

# 🖥️ Requisitos del sistema

## Hardware mínimo recomendado

| Recurso | Mínimo | Recomendado |
|---|---|---|
| CPU | 1 núcleo | 2-4 núcleos |
| RAM | 1 GB | 4 GB o más |
| Disco | 10 GB | 40 GB o más |
| Red | Ethernet | Ethernet Gigabit |

---

# 📥 Descarga de Ubuntu Server

La imagen ISO puede descargarse desde la página oficial:

https://ubuntu.com/download/server

Versión utilizada: Ubuntu Server 24.04 LTS


---

# 💿 Creación del medio de instalación

Herramientas utilizadas:

- Rufus (Windows)
- Balena Etcher (multiplataforma)
- Ventoy

Ejemplo con Ventoy:

1. Instalar Ventoy en el USB.
2. Copiar la ISO de Ubuntu Server.
3. Arrancar el equipo desde el USB.

---

# ⚙️ Proceso de instalación

## 1. Selección del idioma

Seleccionar: Spanish

---

## 3. Configuración de red

Ubuntu intentará configurar la red automáticamente mediante DHCP.

Ejemplo:


Interface: enp0s3

IP: 192.168.1.50

Gateway: 192.168.1.1

DNS: 8.8.8.8


---

## 4. Configuración del proxy

Si no existe un proxy:


Leave empty


---

## 5. Configuración del espejo de paquetes

Usar el servidor oficial:


http://archive.ubuntu.com/ubuntu


---

# 💾 Particionado del disco

Opciones disponibles:

## Uso automático del disco

Ubuntu configura las particiones automáticamente.

Recomendado para:

- Laboratorios
- Máquinas virtuales
- Pruebas

---

## Particionado manual

Ejemplo:


/boot
└── 1 GB

/
└── 30 GB

swap
└── 4 GB


---

# 👤 Creación del usuario administrador

Ejemplo:

Usuario: jose

Hostname: ubuntu-server


El usuario creado tendrá permisos mediante sudo.

Comprobar:

```bash
sudo whoami

Resultado esperado:

root
```

# 🔐 Configuración SSH

Durante la instalación activar:

```
Install OpenSSH Server
```

Esto permitirá administrar el servidor remotamente.

Comprobar el servicio:

```bash
systemctl status ssh
```

Debe mostrar:

```
active (running)
```

---

# 📦 Instalación de paquetes adicionales

Después del primer inicio:

## Actualizar repositorios

```bash
sudo apt update
```

## Actualizar paquetes

```bash
sudo apt upgrade -y
```

## Instalar herramientas básicas

```bash
sudo apt install -y \
nano \
vim \
curl \
wget \
git \
htop \
net-tools
```

---

# 🔄 Actualización inicial del sistema

Comando recomendado:

```bash
sudo apt update && sudo apt full-upgrade -y
```

Eliminar paquetes innecesarios:

```bash
sudo apt autoremove -y
```

Limpiar caché de paquetes:

```bash
sudo apt clean
```

---

# 🌐 Configuración del hostname

## Consultar hostname actual

```bash
hostnamectl
```

## Cambiar hostname

```bash
sudo hostnamectl set-hostname servidor-linux
```

Ejemplo:

```
servidor-linux.local
```

---

# 🔥 Configuración básica del firewall

## Instalar UFW

```bash
sudo apt install ufw
```

## Activar firewall

```bash
sudo ufw enable
```

## Permitir conexiones SSH

```bash
sudo ufw allow ssh
```

## Ver estado del firewall

```bash
sudo ufw status
```

Ejemplo:

```
Status: active

22/tcp ALLOW
```

---

# 🧪 Pruebas iniciales

## Ver información del sistema

```bash
hostnamectl
```

---

## Ver versión de Ubuntu

```bash
lsb_release -a
```

Ejemplo:

```
Ubuntu 24.04 LTS
```

---

## Ver recursos del sistema

### CPU

```bash
lscpu
```

### Memoria RAM

```bash
free -h
```

### Espacio en disco

```bash
df -h
```

---

# 📡 Primera conexión SSH

Desde otro equipo:

```bash
ssh usuario@IP_SERVIDOR
```

Ejemplo:

```bash
ssh jose@192.168.1.50
```

---

# ✅ Checklist final

- [x] Ubuntu Server instalado
- [x] Usuario administrador creado
- [x] SSH configurado
- [x] Sistema actualizado
- [x] Firewall activo
- [x] Herramientas básicas instaladas
- [x] Hostname configurado

---

# 📚 Próximos pasos

Después de completar esta instalación:

- Gestión de usuarios y grupos
- Configuración avanzada SSH
- Servidores web Apache/Nginx
- Docker
- Automatización con Bash
- Monitorización

---

# 📁 Estructura recomendada del proyecto

```text
linux-admin-lab/
│
├── 01-installation/
│   └── ubuntu-server-install.md
│
├── 02-users-permissions/
│   └── users.md
│
├── 03-networking/
│   └── network.md
│
├── 04-services/
│   ├── ssh.md
│   ├── apache.md
│   └── nginx.md
│
└── scripts/
    └── backup.sh
```


