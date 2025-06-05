# Sandbox de Wordpress

Este proyecto proporciona un Sandbox(entorno de prueba) para Wordpress utilizando Docker. Incluye contenedores para MySQL como base de datos y phpMyAdmin para la gestión de la base de datos.

## Tabla de contenidos
1. [Requisitos Previos](#requisitos)
2. [Configuracion del Repositorio](#configuracion-del-repositorio)
3. [Credenciales de Inicio de Sesión](#credenciales-de-inicio-de-sesión)
4. [Acceso a los servicios](#acceso-a-los-servicios)
5. [Instalación de plugins](#instalación-de-plugins)
6. [Utilización de WP-CLI](#utilizacion-de-wp-cli)
7. [Cómo eliminar containers y volumenes](#como-eliminar-containers-y-volumenes)
8. [Activación del modo multisitio](#activación-del-modo-multisitio)

## Requisitos
- Docker
- Docker Compose

## Configuración del repositorio

### 1. Clonar el repositorio
```bash
git clone git@github.com:sedici/ojs-sandox.git
```
Una vez que hayas clonado el repositorio, dirígete al directorio recién creado:
```bash
cd wordpress-sandbox
```
### 2. Levantar el entorno de Wordpress
Y luego utiliza el siguiente comando para levantar el entorno de WP:
```bash
docker compose up
```
Este comando construirá las imágenes necesarias y levantará los contenedores de Docker para que puedas acceder a tu instancia de WP

### 3. ¡Listo!

¡Eso es todo! Ahora deberías tener tu instancia de WP en ejecución. Abre tu navegador y visita http://localhost:80 para acceder a la aplicación.

Recuerda que este entorno de sandbox es para propósitos de desarrollo o pruebas, y puedes detenerlo en cualquier momento utilizando el siguiente comando en la terminal:
```bash
docker compose down
```


## Acceso a los servicios

- **URL de Wordpress:** [http://localhost:80](http://localhost:80)
- **URL de PhpMyAdmin:** [http://localhost:8080](http://localhost:8080)

## Instalación de plugins

Para instalar plugins wordpress debe ser capaz de escribir en el directorio wordpress :

- sudo chown -R user:group wordpress/
- Conviene que el usuario principal sea www-data para poder instalar plugins y que el grupo sea otro usuario
- Ademas, hay que darle permisos de escritura al grupo por si queremos modificar algun archivo (tipicamente el wp-config.php) : chmod -R g+w directorio

## Utilización de WP-CLI

- En el archivo `compose.yml` existe el servicio cli que permite ejecutar comandos de wp-cli
- Una vez que se ejecuta un comando, el container finaliza
- Para probar que funciona, puede ejecutar lo siguientes comandos : 
	- `docker exec wordpress-cli wp --info`
	-	`docker exec wordpress-cli wp core version`
- Para ejecutar un comando : `docker exec [CONTAINER NAME] [COMANDO]`

## Cómo eliminar Containers y Volumenes 

Para eliminar los containers junto con el volumen asociado :

- `docker-compose down -v`



## Activación del modo Multisitio

Por defecto, el proyecto permite levantar un entorno de wordpress single. Para gestionar un multisitio, siga las siguientes indicaciones :

- Agrega la siguiente línea en el archivo `wp-config.php` justo antes de la línea que dice `/* That's all, stop editing! Happy blogging. */`:
	- `define('WP_ALLOW_MULTISITE', true);`

- Luego, en el escritorio de Wordpress : `Herramientas` -> `Configuración de la red` y finalmente instalá las configuraciones necesarias

- Despues de terminar la instalación, se mostraran 2 porciones de codigo que hay que colocar en los archivos `wp-config.php` y `.htaccess` respectivamente

- Una vez hecho esto, para instalar plugins es necesario agregar en `wp-config.php`despues de la linea WP_DEBUG la siguiente linea : 
	- `define('FS_METHOD', 'direct');`