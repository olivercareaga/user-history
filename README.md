# User History

Aplicación para la gestión de cupones únicos. Una vez realizado el alta y tras autenticarse en el sistema, el usuario tendrá la posibilidad de generar cupones y canjearlos. Los cupones quedarán asociados al usuario y se mantendrán almacenados para que este pueda consultarlos cuando los necesite.

## Despliegue del proyecto

Siguiendo estos pasos se podrá desplegar el proyecto en una máquina local para ejecutar en él las pruebas oportunas.

### Requisitos previos

Antes de comenzar con el despliegue, será necesario instalar los siguientes componentes sobre la máquina.

PHP > 7
```
apt-get update && apt-get upgrade
apt-get install php
```

Composer > 1.6
```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('SHA384', 'composer-setup.php') === '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
```

Desde la carpeta de instalación, añadir Composer a la variable de entorno PATH para tener acceso global:
```
mv composer.phar /usr/local/bin/composer
```

MySQL > 5.7
```
sudo apt-get install mysql-server
```

Desde la consola de MySQL, deberá crearse una base de datos donde ejecutar las migraciones del proyecto:
```
mysql -u[usuario] -p[contraseña]
```
```
CREATE DATABASE user_history;
```

### Despliegue

Una vez instalados los requisitos previos, se podrá comenzar el despliegue.

En primer lugar, se debe clonar el proyecto en la máquina local, utilizando este mismo repositorio:
```
git clone https://github.com/olivercareaga/user-history.git
```

Será necesario establecer permisos de escritura al propietario de la carpeta storage, ejecutando desde la raíz del proyecto el siguiente comando:
```
chmod -R 755 storage
```

Una vez cambiados los permisos, se instalarán las dependencias de Composer:
```
composer install
```

Se creará un fichero propio de configuración, utilizando el que Laravel proporciona a modo de ejemplo:
```
cp .env.example .env
```

Y se generará una nueva API key:
```
php artisan key:generate
```

Hecho esto, se establecerán en el fichero .env los parametros de conexión a la base de datos:
```
DB_HOST=localhost
DB_DATABASE=user_history
DB_USERNAME=[usuario]
DB_PASSWORD=[contraseña]
```

Ahora sí, con los parámetros modificados, es posible ejecutar las migraciones junto con los seeders:
```
php artisan migrate --seed
```

Si todas las migraciones se han ejecutado correctamente, es posible lanzar el aplicativo ejecutando el siguiente comando y accediendo a la ruta que devuelve:
```
php artisan serve
```

En la base de datos se habrá creado un usuario de prueba con las siguientes credenciales: 

* Email: hotelinking@laravel.com
* Contraseña: hotelinking

Este usuario, además, incluye diez cupones de prueba, marcando algunos de ellos como canjeados de manera aleatoria.

## Ejecutar pruebas

Esta aplicación ha sido desarrollada bajo una metodología orientada a pruebas. Es posible lanzar la ejecución de las mismas haciendo uso del siguiente comando en la carpeta raíz del proyecto:
```
vendor/bin/phpunit
```

## Autores

* **Oliver Careaga** - Para Hotelinking S.L.
