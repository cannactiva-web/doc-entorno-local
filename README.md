# Configuración del Entorno Local con Docker

## 1. Instala Docker Desktop
Si aún no tienes Docker Desktop, [descárgalo aquí](https://www.docker.com/products/docker-desktop).
Si Docker Desktop no funciona, fer servir [apache friends](https://www.apachefriends.org/)  y saltar al punt 5

## 2. Crea una carpeta para tu Docker
Esta carpeta será donde Docker creará los contenedores. Es recomendable hacerlo a nivel de `Users` para evitar problemas.

## 3. Copia este código y crea un archivo `docker-compose.yml` en la raíz de la carpeta.

```
# Copyright VMware, Inc.
# SPDX-License-Identifier: APACHE-2.0

version: '2'
services:
  mariadb:
    image: docker.io/bitnami/mariadb:11.0
    volumes:
      - 'mariadb_data:/bitnami/mariadb'
      - /Users/nombre/webs/cannactiva/data:/bitnami/mariadb
    environment:
      # ALLOW_EMPTY_PASSWORD is recommended only for development.
      - ALLOW_EMPTY_PASSWORD=yes
      - MARIADB_USER=bn_wordpress
      - MARIADB_DATABASE=bitnami_wordpress
  wordpress:
    image: docker.io/bitnami/wordpress:6
    ports:
      - '80:8080'
      - '443:8443'
    volumes:
      - 'wordpress_data:/bitnami/wordpress'
      - /Users/cristiancascante/webs/cannactiva:/bitnami/wordpress
    depends_on:
      - mariadb
    environment:
      # ALLOW_EMPTY_PASSWORD is recommended only for development.
      - ALLOW_EMPTY_PASSWORD=yes
        - WORDPRESS_DATABASE_HOST=mariadb
        - WORDPRESS_DATABASE_PORT_NUMBER=3306
        - WORDPRESS_DATABASE_USER=bn_wordpress
        - WORDPRESS_DATABASE_NAME=bitnami_wordpress
  volumes:
  mariadb_data:
    driver: local
  wordpress_data:
    driver: local

```
ó
```
version: '3'
services:
  mariadb:
    image: docker.io/bitnami/mariadb:11.0
    volumes:
      - mariadb_data:/bitnami/mariadb
      - /Users/cristiancascante/webs/cannactiva/web-cannactiva-old/data:/bitnami/mariadb
    environment:
      - ALLOW_EMPTY_PASSWORD=yes
      - MARIADB_USER=bn_wordpress
      - MARIADB_DATABASE=bitnami_wordpress

  wordpress:
    image: docker.io/bitnami/wordpress:6
    user: root
    ports:
      - '80:8080'
      - '443:8443'
    volumes:
      - wordpress_data:/bitnami/wordpress
      - /Users/cristiancascante/webs/cannactiva/web-cannactiva-old:/bitnami/wordpress
    depends_on:
      - mariadb
    environment:
      - ALLOW_EMPTY_PASSWORD=yes
      - WORDPRESS_DATABASE_HOST=mariadb
      - WORDPRESS_DATABASE_PORT_NUMBER=3306
      - WORDPRESS_DATABASE_USER=bn_wordpress
      - WORDPRESS_DATABASE_NAME=bitnami_wordpress

volumes:
  mariadb_data:
    driver: local
  wordpress_data:
    driver: local
```

Crear wp-config.pho con la siguiente configuración:

```
define('WP_HOME', 'https://localhost');
define('WP_SITEURL', 'https://localhost');

define('DB_NAME', 'bitnami_wordpress');
define('DB_USER', 'bn_wordpress');
define('DB_PASSWORD', '');
define('DB_HOST', 'mariadb');
define('DB_CHARSET', 'utf8');
define('DB_COLLATE', '');	
```

## 4. Configura la ruta de tu proyecto
Abre `docker-compose.yml` y ajusta la siguiente línea con la ruta de tu proyecto:

```
/Users/nombre/webs/cannactiva/public_html:/bitnami/wordpress
/Users/nombre/webs/cannactiva/public_html/data:/bitnami/wordpress
```

da permisos 777 a la carpeta data

```
sudo chmod -R 777 /home/cannactiva/Cannactiva/web-cannactiva/data
```
Una vez configurado, puedes iniciar Docker, abre la consola y ejecuta:

```
docker-compose up
```

Tu entorno local está listo para trabajar.


## 5. Instala la base de datos
Solicita copia de la base de datos al departamento de IT.

Copia la base de datos en tu contenedor Docker de Mariadb

```
docker cp miarchivo.sql abcd1234:/tmp/
```
Entra en el contenedor

```
docker exec -it abcd1234 /bin/bash
```

Navega al directoriio tmp y carga la bbdd

```
mysql -u [nombre_usuario] -p[nombre_base_de_datos] < mibasededatos.sql
```
ó
```
/opt/bitnami/mariadb/bin/mariadb -u bn_wordpress -p bitnami_wordpress < bbdd2.sql
```


## 6. Instala el repositorio, 
[https://github.com/cannactiva/web-cannactiva](https://github.com/cannactiva/web-cannactiva).



