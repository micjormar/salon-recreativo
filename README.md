
1.1. Descripción del proyecto

Este proyecto consiste en desplegar una aplicación web de un salón recreativo utilizando Docker y Docker Compose.

El stack está formado por:

Servidor web: Apache.
Intérprete: PHP 8.3.
SGBD: MariaDB 11.4.
Cliente: navegador web, donde se ejecuta el HTML, CSS y JavaScript.
Servidor: contenedor Docker con Apache y PHP, que ejecuta la aplicación y se comunica con MariaDB.
Base de datos: MariaDB, ejecutándose en un contenedor independiente.

La aplicación web está publicada en el puerto 8080 del equipo.

La comunicación entre Apache/PHP y MariaDB se realiza mediante una red interna de Docker, por lo que MariaDB no publica directamente su puerto hacia el exterior.


1.2. Diagrama de arquitectura en Mermaid:

```mermaid
graph LR N[Navegador] -->|HTTP :8080| W[Apache + PHP 8.3]
W -->|arcade_net| B[(MariaDB 11.4)]
W --> J[JavaScript en navegador]
N --> J
B --> V[(Volumen Docker)]
```


1.3. Comandos de despliegue:

--Despliegue local

Clonar el repositorio:

git clone <URL_DEL_REPOSITORIO>
cd salon-recreativo

Crear el fichero .env con las variables necesarias para la aplicación y la base de datos.

Después, iniciar los servicios: docker compose up -d --build

Comprobar que los contenedores están funcionando: docker compose ps

La aplicación estará disponible en: http://localhost:8080

Para detener los servicios: docker compose down

Para detenerlos y eliminar también los volúmenes: docker compose down -v


--Despliegue en Play with Docker

Acceder a Play with Docker.

Crear una nueva instancia.

Clonar el repositorio:

git clone <URL_DEL_REPOSITORIO>
cd salon-recreativo

Crear el fichero .env.

Levantar el stack: docker compose up -d --build

Comprobar los contenedores: docker compose ps

Abrir el puerto 8080 desde la interfaz de Play with Docker para acceder a la aplicación.


Nivel 1.2 Respuesta:

El motivo es que la aplicación únicamente necesita las credenciales de jugador; no necesita conocer la contraseña de root. Esto sigue el principio de dar al servicio solamente los permisos y datos que necesita.


Nivel 1.3 Resultado de SHOW GRANTS:

+--------------------------------------------------------------------------------------------------------+
| Grants for jugador@%                                                                                   |
+--------------------------------------------------------------------------------------------------------+
| GRANT USAGE ON *.* TO `jugador`@`%` IDENTIFIED BY PASSWORD '*DC52755F3C09F5923046BD42AFA76BD1D80DF2E9' |
| GRANT ALL PRIVILEGES ON `arcade`.* TO `jugador`@`%`                                                    |
+--------------------------------------------------------------------------------------------------------+
2 rows in set (0.001 sec)

Solo tiene permiso sobre la base de datos arcade y no se usa root para aislar la seguridad de la aplicación.


- MONEDA 1:

+----+--------------------+----------------+--------+
| id | jugador            | juego          | puntos |
+----+--------------------+----------------+--------+
|  1 | PAC-ANA            | Pac-Man        |  48200 |
|  2 | MARIO_84           | Donkey Kong    |  35100 |
|  3 | LARA_C             | Tetris         |  61750 |
|  4 | NEO                | Space Invaders |  27900 |
|  5 | BIMBA_XL           | Pac-Man        |  52300 |
|  6 | ZELDA              | Tetris         |  58400 |
|  7 | R2D2               | Space Invaders |  31200 |
|  8 | TRON               | Donkey Kong    |  29800 |
|  9 | MONEDA-1: ARC-7X3K | secreto        |      0 | <--ESTA ES LA FILA CORRECTA
+----+--------------------+----------------+--------+
9 rows in set (0.002 sec)


- MONEDA 2:

ARC-Q9M2


- MONEDA 3:

Dada en clase


- Problemas que me encontré:

Problema 1 — Los cambios del Dockerfile no aparecían

Al modificar el Dockerfile, los cambios relacionados con la configuración de Apache y PHP no aparecían inmediatamente.

El problema era que el contenedor estaba utilizando una imagen que ya había sido construida anteriormente.

Lo solucioné reconstruyendo la imagen sin utilizar la caché:

docker compose down
docker compose build --no-cache
docker compose up -d

Después comprobé las cabeceras del servidor con:

curl -I http://localhost:8080

y verifiqué que las versiones de Apache y PHP ya no se mostraban.


Problema 2 — git ls-files no funcionaba

Al intentar utilizar: git ls-files aparecía un mensaje indicando que la carpeta no era un repositorio Git.

El problema era que el comando se estaba ejecutando fuera de la carpeta que contenía el repositorio.

La solución fue situarse en la carpeta correcta del proyecto antes de ejecutar los comandos de Git: cd salon-recreativo y comprobar que el repositorio estaba inicializado correctamente.



