# Configuración del Entorno Local con Docker

## 1. Instala Docker Desktop
Si aún no tienes Docker Desktop, [descárgalo e instálalo aquí](https://www.docker.com/products/docker-desktop).

## 2. Descarga el archivo de configuración
[Descarga el archivo `docker-compose.yml` aquí](#).

## 3. Crea una carpeta para tu proyecto
Esta carpeta será donde Docker creará los contenedores. Es recomendable hacerlo a nivel de `Users` para evitar problemas.

## 4. Configura la ruta de tu proyecto
Abre `docker-compose.yml` y ajusta la siguiente línea con la ruta de tu proyecto:

```/Users/nombre/webs/cannactiva/public_html:/bitnami/wordpress```

Una vez configurado, puedes iniciar Docker, abre la consola y ejecuta:

```
docker-compose up
```

tu entorno estará listo para trabajar.
