# Manual instalacion Chatwoot con Docker y Portainer
> **Autor:** Ing. Jose Miguel Cabrera
> **Fecha:** 27 de Marzo de 2026

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

## 4. Crear el stack para Chatwoot

Agregue esta configuracion en el menú de stack
```
services:
  postgresql:
    image: pgvector/pgvector:pg15
    restart: always
    volumes:
      - postgres_data_v2:/var/lib/postgresql/data
    environment:
      - POSTGRES_DB=chatwoot
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password

  redis:
    image: redis:6.2-alpine
    restart: always
    volumes:
      - redis_data:/data

  chatwoot:
    image: chatwoot/chatwoot:latest
    container_name: chatwoot-app
    restart: always
    depends_on:
      - postgresql
      - redis
    environment:
      - NODE_ENV=production
      - POSTGRES_HOST=postgresql
      - POSTGRES_DATABASE=chatwoot
      - POSTGRES_USERNAME=postgres
      - POSTGRES_PASSWORD=password
      - REDIS_URL=redis://redis:6379
      - SECRET_KEY_BASE=vwif9gieJtxZ3FrvjnmWeBN9u4akgC69
      - FRONTEND_URL=https://fruits-exhibitions-alaska-tutorials.trycloudflare.com
      - INSTALLATION_ENV=on-premise
      - FORCE_SSL=true
    command: sh -c "bundle exec rails db:prepare && bundle exec rails s -p 3000 -b 0.0.0.0"
    ports:
      - "3000:3000"

  worker:
    image: chatwoot/chatwoot:latest
    container_name: chatwoot-worker
    restart: always
    
    depends_on:
      - postgresql
      - redis
    environment:
      - NODE_ENV=production
      - POSTGRES_HOST=postgresql
      - POSTGRES_DATABASE=chatwoot
      - POSTGRES_USERNAME=postgres
      - POSTGRES_PASSWORD=password
      - REDIS_URL=redis://redis:6379
      - SECRET_KEY_BASE=vwif9gieJtxZ3FrvjnmWeBN9u4akgC69
      - FRONTEND_URL=https://fruits-exhibitions-alaska-tutorials.trycloudflare.com
      - INSTALLATION_ENV=on-premise
      - FORCE_SSL=true
    command: bundle exec sidekiq -C config/sidekiq.yml

  tunnel:
    image: cloudflare/cloudflared:latest
    command: tunnel --url http://chatwoot:3000
    restart: always

volumes:
  postgres_data_v2:
  redis_data:
```

## 5. Ingresar con la URL que asigno cloudflare

Agregue esta configuracion en el menú de stack



