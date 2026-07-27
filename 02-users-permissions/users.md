# 👥 Gestión de Usuarios y Permisos en Linux

## 📌 Introducción

La administración de usuarios y permisos es una de las tareas fundamentales de un administrador Linux.

Linux utiliza un sistema basado en usuarios, grupos y permisos para controlar quién puede acceder a los recursos del sistema y qué acciones puede realizar.

En este documento se muestran las operaciones básicas para gestionar usuarios, grupos, permisos y propiedad de archivos en Ubuntu Server 24.04 LTS.

---

# 👤 Usuarios en Linux

Linux identifica cada usuario mediante:

- Nombre de usuario.
- UID (User ID).
- Grupo principal.
- Directorio personal.
- Shell asignada.

Consultar usuarios existentes:

```bash
cat /etc/passwd
```

Ejemplo:

```
jose:x:1000:1000:Jose:/home/jose:/bin/bash
```

Estructura:

```
usuario:x:UID:GID:nombre:directorio:shell
```

---

# 📋 Información del usuario actual

Consultar usuario conectado:

```bash
whoami
```

Consultar información completa:

```bash
id
```

Ejemplo:

```
uid=1000(jose)
gid=1000(jose)
groups=1000(jose),27(sudo)
```

---

# ➕ Creación de usuarios

## Crear usuario básico

```bash
sudo adduser nombre_usuario
```

Ejemplo:

```bash
sudo adduser administrador
```

Durante el proceso se solicitará:

- Contraseña.
- Nombre completo.
- Información adicional.

---

# 📂 Directorio personal

Al crear un usuario Linux genera automáticamente:

```
/home/nombre_usuario
```

Ejemplo:

```
/home/administrador
```

Comprobar:

```bash
ls /home
```

---

# 🔑 Cambiar contraseña de usuario

Cambiar contraseña del usuario actual:

```bash
passwd
```

Cambiar contraseña de otro usuario:

```bash
sudo passwd nombre_usuario
```

Ejemplo:

```bash
sudo passwd administrador
```

---

# 🔎 Consultar usuarios del sistema

Mostrar todos los usuarios:

```bash
cut -d: -f1 /etc/passwd
```

Mostrar usuarios normales:

```bash
awk -F: '$3 >= 1000 {print $1}' /etc/passwd
```

---

# ❌ Eliminar usuarios

Eliminar usuario manteniendo sus archivos:

```bash
sudo deluser nombre_usuario
```

Ejemplo:

```bash
sudo deluser administrador
```

---

Eliminar usuario y su directorio personal:

```bash
sudo deluser --remove-home nombre_usuario
```

Ejemplo:

```bash
sudo deluser --remove-home administrador
```

---

# 👥 Gestión de grupos

Los grupos permiten asignar permisos a varios usuarios al mismo tiempo.

Consultar grupos existentes:

```bash
cat /etc/group
```

---

# ➕ Crear grupos

Crear un grupo:

```bash
sudo groupadd nombre_grupo
```

Ejemplo:

```bash
sudo groupadd desarrolladores
```

---

# 👤 Añadir usuarios a grupos

Añadir usuario a un grupo:

```bash
sudo usermod -aG grupo usuario
```

Ejemplo:

```bash
sudo usermod -aG desarrolladores jose
```

La opción:

```
-aG
```

significa:

- `-a` → Añadir sin eliminar otros grupos.
- `-G` → Especificar grupo secundario.

---

# 📋 Ver grupos de un usuario

```bash
groups usuario
```

Ejemplo:

```bash
groups jose
```

Resultado:

```
jose : jose sudo desarrolladores
```

---

# 🔐 Administración de sudo

Los usuarios pertenecientes al grupo `sudo` pueden ejecutar comandos administrativos.

Comprobar grupos del usuario:

```bash
groups
```

Añadir usuario a sudo:

```bash
sudo usermod -aG sudo usuario
```

Ejemplo:

```bash
sudo usermod -aG sudo administrador
```

Comprobar:

```bash
sudo -l
```

---

# 📁 Permisos de archivos en Linux

Linux utiliza tres tipos de permisos:

| Permiso | Significado | Archivo | Directorio |
|---|---|---|---|
| r | Read | Leer | Listar contenido |
| w | Write | Modificar | Crear/eliminar |
| x | Execute | Ejecutar | Acceder |

---

# 👥 Propietarios de archivos

Cada archivo tiene:

- Usuario propietario.
- Grupo propietario.
- Permisos asignados.

Consultar permisos:

```bash
ls -l
```

Ejemplo:

```
-rwxr-xr-- 1 jose usuarios 4096 archivo.txt
```

Interpretación:

```
-rwx r-x r--
 |||  ||| |||
  |    |   |
  |    |   └── Otros usuarios
  |    └────── Grupo
  └─────────── Propietario
```

---

# 🔄 Cambiar propietario

Cambiar propietario de un archivo:

```bash
sudo chown usuario archivo
```

Ejemplo:

```bash
sudo chown jose documento.txt
```

Cambiar propietario y grupo:

```bash
sudo chown usuario:grupo archivo
```

Ejemplo:

```bash
sudo chown jose:desarrolladores proyecto.txt
```

---

# 🔢 Cambiar permisos con chmod

Los permisos pueden representarse mediante números:

| Número | Permiso |
|---|---|
| 4 | Lectura |
| 2 | Escritura |
| 1 | Ejecución |

Ejemplos:

## Permisos completos para propietario

```bash
chmod 700 archivo
```

Resultado:

```
rwx------
```

---

## Lectura y escritura propietario

```bash
chmod 600 archivo
```

Resultado:

```
rw-------
```

---

## Permisos estándar de un script

```bash
chmod 755 script.sh
```

Resultado:

```
rwxr-xr-x
```

---

# 📌 Permisos con modo simbólico

Añadir permiso de ejecución:

```bash
chmod +x archivo.sh
```

Quitar escritura:

```bash
chmod -w archivo.txt
```

Añadir permisos al grupo:

```bash
chmod g+w archivo.txt
```

---

# 📂 Permisos en directorios

Crear directorio:

```bash
mkdir proyecto
```

Cambiar permisos:

```bash
chmod 750 proyecto
```

Resultado:

```
rwxr-x---
```

---

# 🔒 Usuarios bloqueados

Bloquear usuario:

```bash
sudo passwd -l usuario
```

Ejemplo:

```bash
sudo passwd -l prueba
```

Desbloquear usuario:

```bash
sudo passwd -u usuario
```

---

# ⏳ Ver últimos accesos

Consultar últimos inicios de sesión:

```bash
last
```

Consultar usuarios conectados:

```bash
who
```

---

# 🧪 Ejercicio práctico

Crear un usuario de prueba:

```bash
sudo adduser prueba
```

Crear un grupo:

```bash
sudo groupadd laboratorio
```

Añadir usuario al grupo:

```bash
sudo usermod -aG laboratorio prueba
```

Crear carpeta:

```bash
sudo mkdir /laboratorio
```

Cambiar propietario:

```bash
sudo chown prueba:laboratorio /laboratorio
```

Asignar permisos:

```bash
sudo chmod 770 /laboratorio
```

Comprobar:

```bash
ls -ld /laboratorio
```

Resultado esperado:

```
drwxrwx--- prueba laboratorio /laboratorio
```

---

# ✅ Checklist final

- [x] Crear usuarios
- [x] Modificar usuarios
- [x] Crear grupos
- [x] Gestionar permisos sudo
- [x] Cambiar propietarios
- [x] Aplicar permisos con chmod
- [x] Administrar accesos

---
- Gestión de discos y almacenamiento.
- Automatización con Bash.
