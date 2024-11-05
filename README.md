
---

### 1. Crear o ficheiro `docker-compose.yml`

Primeiro, crea o ficheiro `docker-compose.yml` coa seguinte configuración para lanzar BIND9:

```yaml
version: '3.8'

services:
  bind9:
    image: internetsystemsconsortium/bind9:9.18
    container_name: bind9
    ports:
      - "54:53/tcp"
      - "54:53/udp"
      - "127.0.0.1:953:953/tcp" # Porto de control de BIND
    volumes:
      - ./etc/bind:/etc/bind          # Configuración de BIND
      - ./var/cache/bind:/var/cache/bind  # Caché de BIND
      - ./var/log:/var/log            # Logs de BIND
    environment:
      - TZ=Europe/Madrid              # Zona horaria
      - BIND9_USER=root               # Usuario de BIND
    restart: unless-stopped
```

 - Debemos ter coidado. Ao utilizar a máquina virtual o 53 xa estase utilizando. Por isto deberemos cambialo polo 54. (Nota: esquerda:host, dereita:contenedor)

### 2. Proba o contedor

Para levantar o contedor en segundo plano, executa:

```bash
docker compose up -d
```

Comproba que o contedor está en execución usando:

```bash
docker ps
```

### 3. Subir a GitHub

1. Crea un novo repositorio en GitHub, por exemplo, chamado `docker-bind9-setup`.
2. Suba o ficheiro `docker-compose.yml` e o README.md ao repositorio.

```bash
git init
git add .
git commit -m "Add docker compose for BIND9 setup"
git branch -M main
git remote add origin https://github.com/lmrdmt/docker-bind9-setup.git
git push -u origin main
```

### 4. Engadir o README.md

No ficheiro `README.md`, engade a seguinte descrición para explicar as opcións de `docker compose.yaml`:

```markdown
# Docker BIND9 Setup

Este repositorio contén un arquivo `docker compose.yml` para lanzar un contedor de BIND9.

## Configuración de Volumes e Entorno

- `./etc/bind:/etc/bind`: Contén os ficheiros de configuración de BIND9.
- `./var/cache/bind:/var/cache/bind`: Caché de datos de BIND.
- `./var/log:/var/log`: Directorio para almacenar os logs.

## Variables de Entorno

- `TZ=Europe/Madrid`: Define a zona horaria.
- `BIND9_USER=root`: Especifica o usuario que executará BIND dentro do contedor.

## Portos

- `54/tcp` e `54/udp`: Consultas DNS.
- `953/tcp`: Porto de control para BIND.

## Como Usar

1. Clona o repositorio:
   ```bash
   git clone https://github.com/lmrdmt/docker-bind9-setup.git
   ```

2. Inicia o contedor:
   ```bash
   docker compose up -d
   ```

3. Verifica que BIND está funcionando:
   ```bash
   docker ps
   ```

4. Para deter o contedor:
   ```bash
   docker compose down
   ```
```

### Enlace ao repositorio

Aquí está o enlace ao repositorio de GitHub:

```
https://github.com/lmrdmt/PRACTICA5
```

Podes utilizar `docker container prune`, `docker network prune` e `docker image prune` para eliminar contedores, redes e imaxes de forma máis específica.

### En caso de error ao lanzar o docker compose up

- **Eliminar contedores detidos**:
  ```bash
  docker container prune
  ```

- **Eliminar redes non usadas** (non conectadas a contedores activos):
  ```bash
  docker network prune
  ```

- **Eliminar imaxes non usadas** (sen contedores asociados):
  ```bash
  docker image prune
  ```

Para realizar unha **limpeza completa** e liberar todo o espazo de recursos inactivos:

```bash
docker system prune --volumes
```


Este último comando inclúe volumes non asociados a contedores activos, así que utilízao con precaución.

O comando `prune` en Docker limpa recursos inactivos, liberando espazo ao eliminar contedores detidos, imaxes sen uso, redes sen conexión e volumes inactivos. É útil para manter o sistema limpo e liberar espazo innecesario de forma eficiente. A opción de incluír volumes debe usarse con precaución, xa que borra tamén datos almacenados non asociados a contedores activos.

