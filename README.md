# Tutorial paso a paso: SSH, SFTP y SCP en Google Cloud (Ubuntu VM)

En este tutorial aprenderás a administrar servidores en Google Cloud utilizando **SSH, SFTP y SCP**.  
Te enseño paso a paso cómo crear una máquina virtual con un sistema operativo **Ubuntu**, conectarte de forma segura, transferir archivos desde tu PC al servidor y copiar archivos directamente entre dos instancias de Google Cloud.

📺 Video en YouTube: [Ver tutorial completo](https://youtu.be/rhWz6NVdPoE)

---

## 📌 Contenido
- [1. Creación de la Máquina Virtual (VM)](#1-creación-de-la-máquina-virtual-vm)
- [2. Conexión SSH](#2-conexión-ssh)
- [3. Transferencia de Archivos con SFTP](#3-transferencia-de-archivos-con-sftp)
- [4. Copia de Archivos entre Servidores con SCP](#4-copia-de-archivos-entre-servidores-con-scp)

---

## 1 Creación de la Máquina Virtual VM

Configura tu VM en Google Cloud con las siguientes características recomendadas:

- **Nombre**: descriptivo según el caso de uso  
- **Región y zona**: cercanas para reducir latencia  
- **CPU**: 4 núcleos  
- **RAM**: 4 GB  
- **Sistema Operativo**: Ubuntu  
- **Disco**: 40 GB  
- **Red**: habilitar tráfico HTTP y HTTPS  

---

## 2 Conexión SSH

1. Genera un par de claves SSH en tu equipo local:  
   ```bash
   ssh-keygen -t rsa -b 4096 -f C:\ruta\a\tu\carpeta\mi_clave -C "tu-usuario-de-gcloud"

2. Si deseas utilizar por defecto C:\\.ssh
   ```bash
   ssh-keygen -t rsa -b 4096 -C "tu-usuario-de-gcloud"

3. En nuestra maquina virtual añadimos nuestra clave publica  
4. Realizamos la conexión
   ```bash
   ssh -i clave usuario@IP

---

## 3 Transferencia de archivos SFTP

1. Conectate al servidor por medio de SFTP
    ```bash
    sftp -i clave usuario@IP

2. Subir archivos desde mi PC al server
    ```bash
    put path_local path_server
    put C:\PruebasDev\Archivos\ejemplo01.txt /home/usuario/sftp_archivos

3. Subir carpeta completa
    ```bash
    put -r path_local path_server
    put -r C:\PruebasDev\Archivos\Ejemplos /home/usuario/sftp_archivos

4. Descargar archivo desde Server a local
   ```bash
   get path_server path_local
   get /home/usuario/sftp_archivos/archivo01.txt C:\PruebasDev\Archivos

5. Descargar carpetas completas desde server a local
   ```bash
   get -r path_server path_local
   get -r /home/usuario/sftp_archivos/varios C:\PruebasDev\Archivos

---

## 4 Copia de Archivos entre Servidores con SCP

1. En el servidor origen, revisa si ya existe un par de claves SSH:
   ```bash
   ls ~/.ssh
  - Ejemplos de archivos: id_rsa, id_rsa.pub o id_ed25519, id_ed25519.pub.
 
2. Si no existe, créalo:
    ```bash
    ssh-keygen -t rsa -b 4096 -C "usuario@correo.com"

3. Copia la clave pública al servidor destino:
    ```bash
    cat ~/.ssh/id_rsa.pub

4. En el servidor destino, agrega la clave al archivo authorized_keys:
    ```bash
    mkdir -p ~/.ssh
    chmod 700 ~/.ssh
    nano ~/.ssh/authorized_keys
    chmod 600 ~/.ssh/authorized_keys

5. Conexión de prueba
    ```bash
    ssh usuario@IP_DESTINO

6. Copiar archivos con SCP
Ejemplo de transferencia de archivo desde el servidor origen al destino:
     ```bash
     scp /home/usuario/archivo.txt usuario@34.27.94.40:/home/usuario/archivo.txt
