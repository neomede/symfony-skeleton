Symfony Skeleton
================

Symfomy skeleton with Sonata

**Spanish/Español**: Documentación en español en **README.es.md**

Installation
------------

First, you need to install composer [link](https://getcomposer.org/doc/00-intro.md)

1. Symfony Skeleton

    ```sh
    composer create-project neomede/symfony-skeleton <yourProyect/> dev-symfony
    ```

2. Symfony Skeleton + Sonata Admin Bundle

    ```sh
    composer create-project neomede/symfony-skeleton <yourProyect/> dev-sonata-admin
    ```

3. Symfony Skeleton + Sonata Admin Bundle + Sonata User Bundle

    ```sh
    composer create-project neomede/symfony-skeleton <yourProyect/> dev-sonata-user
    ```

4. Symfony Skeleton + Sonata Admin Bundle + Sonata User Bundle + Sonata Media Bundle

    ```sh
    composer create-project neomede/symfony-skeleton <yourProyect/> dev-sonata-media
    ```

Configure
---------

Run the following command to make sure that your system meets all the technical requirements:

```sh
php app/check.php
```

You need to visit the next url to check all the technical requirements in the web server and configure the symfony project:

```
localhost\YourProjectRoute\web\config.php
```

### Errors

#### Cache/Logs folder permissions

The *app/cache* and *app/logs* must be writable both by the web server and the command line user.

You need to ensure that the web server have the correct permissions. If you have problems with this, you can run the following commands:

1. Using ACL on a system that supports chmod +a

```sh
$ rm -rf app/cache/*
$ rm -rf app/logs/*

$ HTTPDUSER=`ps aux | grep -E '[a]pache|[h]ttpd|[_]www|[w]ww-data|[n]ginx' | grep -v root | head -1 | cut -d\  -f1`
$ sudo chmod +a "$HTTPDUSER allow delete,write,append,file_inherit,directory_inherit" app/cache app/logs
$ sudo chmod +a "`whoami` allow delete,write,append,file_inherit,directory_inherit" app/cache app/logs
```

2. Using ACL on a system that does not support chmod +a

```sh
$ HTTPDUSER=`ps aux | grep -E '[a]pache|[h]ttpd|[_]www|[w]ww-data|[n]ginx' | grep -v root | head -1 | cut -d\  -f1`
$ sudo setfacl -Rn -m u:"$HTTPDUSER":rwX -m u:`whoami`:rwX app/cache app/logs
$ sudo setfacl -dRn -m u:"$HTTPDUSER":rwX -m u:`whoami`:rwX app/cache app/logs
```

Sonata Media Bundle
-------------------

If they are not already created, you need to add specific folder to allow uploads from users:

```sh
mkdir web/uploads
mkdir web/uploads/media
chmod -R 0777 web/uploads
```

To serve uploaded files you can create a virtual host:

### **nginx**

1. Edit **/etc/hosts** and add a new hostname (**static.vhost.dev**):

    ```sh
    127.0.0.1       localhost       static.vhost.dev
    ```

2. Add new site (**statics**) in **sites-avaliable**:

    ```sh
    server {
            listen 8080 ;

            server_name static.vhost.dev;

            location / {
                root your_project_directory/web/uploads;
            }
    }
    ```

3. Create a new symbolic link to the new file in sites-enabled.

4. In **app/config/config.yml** edit the following lines:

    ```sh
    cdn:
        server:
            path: http://static.vhost.dev:8080/media
    ```
