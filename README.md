# docker-controller-bot
[![](https://badgen.net/badge/icon/github?icon=github&label)](https://github.com/dgongut/docker-controller-bot)
[![](https://badgen.net/badge/icon/docker?icon=docker&label)](https://hub.docker.com/r/dgongut/docker-controller-bot)
[![](https://badgen.net/badge/icon/telegram?icon=telegram&label)](https://t.me/dockercontrollerbotnews)
[![Docker Pulls](https://badgen.net/docker/pulls/dgongut/docker-controller-bot?icon=docker&label=pulls)](https://hub.docker.com/r/dgongut/docker-controller-bot/)
[![Docker Stars](https://badgen.net/docker/stars/dgongut/docker-controller-bot?icon=docker&label=stars)](https://hub.docker.com/r/dgongut/docker-controller-bot/)
[![Docker Image Size](https://badgen.net/docker/size/dgongut/docker-controller-bot?icon=docker&label=image%20size)](https://hub.docker.com/r/dgongut/docker-controller-bot/)
![Github stars](https://badgen.net/github/stars/dgongut/docker-controller-bot?icon=github&label=stars)
![Github forks](https://badgen.net/github/forks/dgongut/docker-controller-bot?icon=github&label=forks)
![Github last-commit](https://img.shields.io/github/last-commit/dgongut/docker-controller-bot)
![Github last-commit](https://badgen.net/github/license/dgongut/docker-controller-bot)
![alt text](https://github.com/dgongut/pictures/blob/main/Docker-Controller-Bot/mockup.png)

Lleva el control de tus contenedores docker desde un único lugar.

- ✅ Listar contenedores
- ✅ Arrancar, parar y eliminar contenedores
- ✅ Obtener los logs tanto de manera directa como a través de fichero
- ✅ Extraer el docker-compose de tus contenedores
- ✅ Notificaciones cuando un contenedor se cae o se inicia
- ✅ Notificaciones cuando un contenedor tiene una actualización pendiente
- ✅ Actualizaciones de los contenedores
- ✅ Cambiar el tag (rollback o actualización)
- ✅ Limpia el sistema, eliminado contenedores, imagenes y otros objetos no utilizados.
- ✅ Soporte de idiomas (Spanish, English, Dutch, German, Russian, Galician, Italian)

¿Lo buscas en [![](https://badgen.net/badge/icon/docker?icon=docker&label)](https://hub.docker.com/r/dgongut/docker-controller-bot)?

**NUEVO** Canal de novedades en [![](https://badgen.net/badge/icon/telegram?icon=telegram&label)](https://t.me/dockercontrollerbotnews)

🖼️ Si deseas establecerle el icono al bot de telegram, te dejo [aquí](https://raw.githubusercontent.com/dgongut/pictures/main/Docker-Controller-Bot/Docker-Controller-Bot.png) el icono en alta resolución. Solo tienes que descargarlo y mandárselo al @BotFather en la opción de BotPic.

## Configuración en config.py

| CLAVE  | OBLIGATORIO | VALOR |
|:------------- |:---------------:| :-------------|
|TELEGRAM_TOKEN |✅| Token del bot |
|TELEGRAM_ADMIN |✅| ChatId del administrador (se puede obtener hablándole al bot Rose escribiendo /id). Admite múltiples administradores separados por comas. Por ejemplo 12345,54431,55944 |
|TELEGRAM_GROUP |❌| ChatId del grupo. Si este bot va a formar parte de un grupo, es necesario especificar el chatId de dicho grupo. Es necesario que el bot sea administrador del grupo |
|TELEGRAM_THREAD |❌| Thread del tema dentro de un supergrupo; valor numérico (2,3,4..). Por defecto 1. Se utiliza en conjunción con la variable TELEGRAM_GROUP |
|TELEGRAM_NOTIFICATION_CHANNEL |❌| Canal donde se publicarán exclusivamente los cambios de estado de los contenedores |
|CONTAINER_NAME |✅| Nombre del contenedor, lo que se le ponga en container_name en el docker-compose ha de ir aquí también |
|TZ |✅| Timezone (Por ejemplo Europe/Madrid) |
|CHECK_UPDATES |❌| Si se desea que compruebe actualizaciones. 0 no - 1 sí. Por defecto 1|
|CHECK_UPDATE_EVERY_HOURS |❌| Tiempo de espera en horas entre chequeo de actualizaciones (4 horas por defecto) | 
|BUTTON_COLUMNS |❌| Numero de columnas de botones en las listas de contenedores (2 columnas por defecto) | 
|LANGUAGE |❌| Idioma, puede ser ES / EN / NL / DE / RU / GL. Por defecto es ES (Spanish) | 
|EXTENDED_MESSAGES |❌| Si se desea que muestre más mensajes de información. 0 no - 1 sí. Por defecto 0 | 

### Anotaciones
Será necesario mapear un volumen para almacenar lo que el bot escribe en /app/schedule

### Ejemplo de Docker-Compose para su ejecución normal

```yaml
version: '3.3'
services:
    docker-controller-bot:
        environment:
            - TELEGRAM_TOKEN=
            - TELEGRAM_ADMIN=
            - CONTAINER_NAME=docker-controller-bot
            - TZ=Europe/Madrid
            #- TELEGRAM_GROUP=
            #- TELEGRAM_THREAD=1
            #- TELEGRAM_NOTIFICATION_CHANNEL=
            #- CHECK_UPDATES=1
            #- CHECK_UPDATE_EVERY_HOURS=4
            #- BUTTON_COLUMNS=2
            #- LANGUAGE=ES
            #- EXTENDED_MESSAGES=0
        volumes:
            - /var/run/docker.sock:/var/run/docker.sock # NO CAMBIAR
            - /ruta/para/guardar/las/programaciones:/app/schedule # CAMBIAR LA PARTE IZQUIERDA
        image: dgongut/docker-controller-bot:latest
        container_name: docker-controller-bot
        restart: always
        network_mode: host
        tty: true
```

### Funciones Extra mediante Labels/Etiquetas en otros contenedores

- Añadiendo la etiqueta `DCB-Ignore-Check-Updates` a un contenedor, no se comprobarán actualizaciones para él.
- Añadiendo la etiqueta `DCB-Auto-Update` a un contenedor, se actualizará automáticamente sin preguntar.

### Agradecimientos

Traducción al neerlandés: [ManCaveMedia](https://github.com/ManCaveMedia)
Traducción al alemán: [shedowe19](https://github.com/shedowe19)
Traducción al ruso: [leyalton](https://github.com/leyalton)
Traducción al gallego: [monfero](https://github.com/monfero)

---

## Solo para desarrolladores - Ejecución con código local


Para su ejecución en local y probar nuevos cambios de código, se necesita renombrar el fichero `.env-example` a `.env` con los valores necesarios para su ejecución.
Es necesario establecer un `TELEGRAM_TOKEN` y un `TELEGRAM_ADMIN` correctos y diferentes al de la ejecución normal.

La estructura de carpetas debe quedar:

```
docker-controller-bot/
    ├── .env
    ├── LICENSE
    ├── README.md
    ├── config.py
    ├── docker-controller-bot.py
    ├── Dockerfile_local
    ├── docker-compose.yaml
    └── locale
        ├── en.json
        ├── es.json
        ├── de.json
        ├── ru.json
        ├── gl.json
        └── nl.json
```

Para levantarlo habría que ejecutar en esa ruta: `docker compose up -d`

Para detenerlo y probar nuevos cambios habría que ejecutar en esa ruta: `docker compose down --rmi`
