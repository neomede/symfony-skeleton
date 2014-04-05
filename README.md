Symfony Skeleton
================

Esqueleto de aplicación Symfony

Configuración General
---------------------

En fichero **composer.json** ya están incluidas las dependencias necesarias, para actualizar composer:

```sh
composer update
```

Durante la instalación de symfony, nos pedira configurar varios parámetros.


Después instalamos revisamos si el systema tiene todo lo necesario para el funcionamiento de symfony, debemos arreglar todos los errores para un correcto funcionamiento:

```sh
php app/check.php
```

También revisamos los requerimientos del servidor web en la siguiente url:
```sh
localhost\SymfonySkeleton\web\config.php
```

### Posibles errores

#### Permisos de cache y logs

Si hay problemas al eliminar la cache, revisar los permisos de los directorios **cache** y **logs**, debe de poder escribir el servidor web (normalmente *www-data*).

Si el problema continua probar a configurar las ACL's:

* Usando ACL en un sistema que soporta **chmod +a**

```sh
$ rm -rf app/cache/*
$ rm -rf app/logs/*

$ HTTPDUSER=`ps aux | grep -E '[a]pache|[h]ttpd|[_]www|[w]ww-data|[n]ginx' | grep -v root | head -1 | cut -d\  -f1`
$ sudo chmod +a "$HTTPDUSER allow delete,write,append,file_inherit,directory_inherit" app/cache app/logs
$ sudo chmod +a "`whoami` allow delete,write,append,file_inherit,directory_inherit" app/cache app/logs
```

* Usando ACL en un sistema que no soporta **chmod +a**

```sh
$ HTTPDUSER=`ps aux | grep -E '[a]pache|[h]ttpd|[_]www|[w]ww-data|[n]ginx' | grep -v root | head -1 | cut -d\  -f1`
$ sudo setfacl -Rn -m u:"$HTTPDUSER":rwX -m u:`whoami`:rwX app/cache app/logs
$ sudo setfacl -dRn -m u:"$HTTPDUSER":rwX -m u:`whoami`:rwX app/cache app/logs
```
