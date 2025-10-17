# PRACTICA 1 - CONFIGURAR GRUB Y RECUPERAR ROOT

# Editar archivo GRUB
sudo nano /etc/default/grub

# Cambiar el tiempo de espera
# GRUB_TIMEOUT=20

# Actualizar grub
sudo update-grub

# Verificar cambio
cat /etc/default/grub | grep GRUB_TIMEOUT

# Reiniciar
sudo reboot

# --- Recuperar contraseña root ---
# En el menú GRUB, presionar 'e' sobre la opción Debian
# Agregar al final de la línea que inicia con 'linux /boot/vmlinuz...'
# init=/bin/bash

# Montar sistema con permisos de escritura
mount -o remount,rw /

# Cambiar contraseña
passwd

# Reiniciar
exec /sbin/init


# PRACTICA 2 - SCRIPTS

# Script de backup con TAR
nano backup_home.sh

# Contenido del script:
# ----------------------------------------
# #!/bin/bash
# FECHA=$(date +"%d-%m-%Y:%H:%M")
# TARFILE="/home/$USER/backup_$FECHA.tar.gz"
# tar -czf "$TARFILE" /home/$USER
# echo "Backup creado en: $TARFILE"
# ----------------------------------------

chmod +x backup_home.sh
./backup_home.sh
ls /home/$USER | grep backup


# Script para guardar ifconfig en Escritorio
sudo apt update
sudo apt install net-tools -y
nano ifconfig_to_desktop.sh

# Contenido del script:
# ----------------------------------------
# #!/bin/bash
# read -p "Ingrese el nombre del archivo: " nombre
# ifconfig > "/home/$USER/Escritorio/${nombre}.txt"
# echo "Archivo creado en el escritorio con el nombre ${nombre}.txt"
# ----------------------------------------

chmod +x ifconfig_to_desktop.sh
./ifconfig_to_desktop.sh
ls /home/$USER/Escritorio


# PRACTICA 3 - RED Y SSH

# Ver IP
ifconfig

# Instalar SSH Server
sudo apt install openssh-server -y

# Habilitar y verificar servicio
sudo systemctl enable ssh
sudo systemctl start ssh
sudo systemctl status ssh

# Conectarse desde Windows:
# ssh tu_usuario@ip_debian

# En Windows, generar llaves
ssh-keygen

# Copiar llave pública al servidor
ssh-copy-id tu_usuario@ip_debian

# Si no tienes ssh-copy-id:
type $env:USERPROFILE\.ssh\id_rsa.pub
# Copiar el contenido y pegarlo en Debian:
mkdir -p ~/.ssh
nano ~/.ssh/authorized_keys

# Probar conexión sin contraseña
ssh tu_usuario@ip_debian
