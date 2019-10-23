## Autodespliegue de página web en servidor Apache2

### ¿Qué necesitas?

- Un sistema operativo Debian (por ejemplo)
- Un servidor Apache2 iniciado en el OS
- Tener en Debian paquetes instalados como wget, crontab, unzip, etc...
- El código de tu web en un repositorio subido a github

1. Crea un fichero que se llame como quieras (EN EL SISTEMA DONDE ESTE EL SERVIDOR, NO EN LOCAL), en mi caso se llama 'autodeploy-angular' sin extensión y lo tengo dentro de /usr/share, para crearlo puedes usar cualquier editor de texto, o simplemente `$ sudo vim /usr/share/autodeploy-angular`

2. Edita el fichero creado anteriormente y haz tu script, en mi caso es para una web hecha en angular, antes que nada crea una carpeta temporal, por ejemplo

`mkdir ~/tmp/deploy`

```bash
#!/bin/bash

# La siguiente línea eliminará la carpeta temporal que genere
rm -rf ~/tmp/deploy/angular-autodeploy-apache-test-master/

# entramos en la carpeta ~/tmp/deploy
cd ~/tmp/deploy

# Descargamos el codigo de la web con wget
wget https://github.com/USERNAME/REPOSITORY/archive/master.zip

# Descomprimimos el archivo
unzip master.zip

# Eliminamos el .zip
rm -rf master.zip

# PARA PAGINAS WEB HECHAS CON ANGULAR:::::::::::

# Entramos en el directorio de la web, en este caso
cd ~/tmp/deploy/angular-autodeploy-apache-test-master/web

# Instalamos dependencias de angular
npm i

# Hacemos la build de la aplicacion web de angular
ng build

# Al hacer el build se ha creado la carpeta dist, movemos lo que haya dentro de ./dist/web al servidor, normalmente suele estar en /var/www/html
mv ~/tmp/deploy/angular-autodeploy-apache-test-master/web/dist/web/* /var/www/html/.

# Eliminamos la carpeta temporal
rm -rf ~/tmp/deploy/angular-autodeploy-apache-test-master/

```

Damos permisos al script
para ello
`sudo chmod +x /ruta/del/script/autodeploy-angular`

ahora vamos a hacer que este script se ejecute continuamente

para ello en la consola escribes `crontab -e`
te va a pedir un editor [nano, vim, emacs, etc...], eliges tu favorito (aka vim), y abajo del todo pones esto

```bash
* * * * * /ruta/del/script/autodeploy-angular
```

Esto hará que el script se ejecute cada minuto, si quieres que se ejecute cada 5 minutos por ejemplo

```bash
*/5 * * * * /ruta/del/script/autodeploy-angular
```

Si quieres que se ejecute a las en punto (ej: 14:00, 15:00, 16:00......)

```bash
0 * * * * /ruta/del/script/autodeploy-angular
```

A las y 27 (ej: 14:27, 15:27, 16:27)

```bash
27 * * * * /ruta/del/script/autodeploy-angular
```

mas configuraciones de crontab: https://www.ostechnix.com/a-beginners-guide-to-cron-jobs/



Con esto ya deberia de estar, espero que no se me haya olvidado nada :)