
1.2 Respuesta:

El motivo es que la aplicación únicamente necesita las credenciales de jugador; no necesita conocer la contraseña de root. Esto sigue el principio de dar al servicio solamente los permisos y datos que necesita.


1.3 Resultado de SHOW GRANTS:

+--------------------------------------------------------------------------------------------------------+
| Grants for jugador@%                                                                                   |
+--------------------------------------------------------------------------------------------------------+
| GRANT USAGE ON *.* TO `jugador`@`%` IDENTIFIED BY PASSWORD '*DC52755F3C09F5923046BD42AFA76BD1D80DF2E9' |
| GRANT ALL PRIVILEGES ON `arcade`.* TO `jugador`@`%`                                                    |
+--------------------------------------------------------------------------------------------------------+
2 rows in set (0.001 sec)

Solo tiene permiso sobre la base de datos arcade y no se usa root para aislar la seguridad de la aplicación.


MONEDA 1:

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


MONEDA 2:

ARC-Q9M2



