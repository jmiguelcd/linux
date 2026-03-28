# Manual instalacion n8n con Docker y Portainer
> **Autor:** Ing. Jose Miguel Cabrera
> **Fecha:** 16 de marzo de 2026

## 1. Actualizar librerías e instalar paquetes básicos
Copia y pega estos comandos en terminal:
```
sudo apt update
sudo apt upgrade -y
sudo apt install -y curl net-tools wget vim git gnupg2 software-properties-common apt-transport-https ca-certificates lsb-release
```

## 2. Instalar Docker
Ejecuta estos comandos
```
su -
curl -fsSL https://get.docker.com -o get-docker.sh && sudo sh get-docker.sh
```

## 3. Instalar Portainer

Ejecute este comando:
```
docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```




