# Título
Proyecto 2

# Autores
- Juan Pablo Peña F.
- Juan Sebastián Sanín V.

# Software
_Wordpress_: _Google Cloud Platform_, _AWS_, _Docker_ y certificado _SSL_.

# Descripción
Página web _Wordpress_ en _Docker_ con certificado _SSL_ haciendo uso de _NGINX_, montada en _Google Cloud Platform_ y también se intentó montar en _AWS_. 

# Detalles del diseño
## _Google Cloud Platform_ (_GCP_)
1. Se adquirió un dominio de manera gratuita en _https://www.freenom.com/_
2. Se creó una instancia de base de datos de poco consumo en la sección _SQL_ de _GCP_.
3. Se creó una instancia para un sistema de archivos a través de la sección _Filestore_ de _GCP_. 
4. Se desplegó una instancia de bajo costo de _VM_ en _GCP_.
5. Se le asignó una IP estática a la instancia.
6. En _GCP_ se creó una zona en _Cloud DNS_ a la cual se le agregó un conjunto de registro de tipo A con la IP de la instancia y otro registro de tipo CNAME.
8. En la instancia se clonó este repositorio en el cual se encuentra la lógica básica de la aplicación, que se encuentra basada en el _GitHub_ de https://github.com/jmlcas/Docker-WordPress-SSL
9. Se desplegó la aplicación haciendo uso de _Docker_.
10. No se logró nada en cuanto al balanceador de cargas puesto que tras muchos intentos no lo logramos realizar, debido al desconcimiento del funcionamiento de _GCP_.
11. Todos los servicios son administrados puesto que a la hora de hacerlos no administrados se nos presentaban muchas fallas las cuales no fuimos capaces de resolver, principalmente por desconocimiento de como funcionan todas estas tecnologías.

## _Amazon Web Services_ (_AWS_)
1. La aplicación es totalmente monolítica.
2. Se adquirió un dominio de manera gratuita en _https://www.freenom.com/_
3. Se creó un grupo de seguridad que permitiera el acceso a HTTP y HTTPS.
4. Se creó una instancia con la imagen _"Amazon Linux 2 AMI (HVM), SSD Volume Type"_.
5. Se siguió el tutorial del profesor Edwin Montoya para desplegar la aplicación: https://github.com/st0263eafit/st026320212/tree/main/docker-nginx-wordpress-ssl-letsencrypt

__Nota:__ Se intentó realizar el _Laboratorio-EFS-AutoScaling_ que dejó el profesor, pero no se logró terminar debido a un error que se presentaba a la hora de crear un _NAT Gateway_.


# Instalación
## _Google Cloud Platform_ (_GCP_)
Primero, debemos crear una instancia en la sección _SQL_, nosotros la personalizamos con el fin de que fuera de bajo costo.

En la instancia debemos crear nuestra base de datos.

Luego, en la instancia, debemos ir a la sección de _Conexiones_ y agregar las redes que están autorizadas para conectarse a la instancia, nosotros para este caso decidimos darle acceso a todas colocando _0.0.0.0/0_

A continuación, debemos ir a la sección de _Filestore_ y crear una nueva instancia. Es recomendado que para crear la instancia se siga el siguiente tutorial: https://cloud.google.com/filestore/docs/quickstart-console, específicamente la parte de "_Create a Filestore instance_".

__Nota:__ póngale de nombre al archivo compartido el siguiente: _wpvol_

Después, en la sección de _Compute Engine_ debemos crear una instancia de _VM_, permitiendo el acceso a HTTP y HTTPS.

Tras creada la instancia, nos debemos conectar a esta dando click en _SSH_.

En la instancia, primero, debemos actualizar el sistema:
```
sudo apt-get update
```

Luego, debemos conectar la instancia con nuestro _Filestore_, para esto debemos ejecutar los siguientes comandos:
```
sudo apt-get -y update &&
sudo apt-get -y install nfs-common
sudo mkdir -p /mnt/wp
```

Acá recuerde cambiar _10.0.0.2_ por la dirección IP que se le asignó en su instancia de _Filestore_.
```
sudo mount 10.0.0.2:/wpvol /mnt/wp
sudo chmod go+rw /mnt/wp
```

Posteriormente, debemos instalar _Docker_ y _Docker-Compose_:
```
sudo apt-get install docker.io docker-compose
```

Seguidamente, debemos instalar _git_ para clonar el repositorio:
```
sudo apt-get install git
```

Después, debemos clonar este repositorio:
```
sudo git clone https://github.com/JPabloPena/Wordpress-Docker.git
```

Luego, tenemos que ingresar a la carpeta _wordpress-gcp_:
```
cd Wordpress-Docker/wordpress-gcp/
```

Ahora, debemos modificar el archivo _.env_ con nuestros valores, para hacerlo:
```
sudo su
nano .env
```

Cuando termine de modificarlos use Ctrl+s y Ctrl+x, para guardar y salir de nano. Luego escriba:
```
exit
```

Finalmente, debemos ejecutar:
```
$ sudo docker-compose up -d
```

Listo! Ya puede acceder a través de su página web y configurar su sitio _Wordpress_.

## _Amazon Web Services_ (_AWS_)
Se siguió el tutorial del profesor Edwin Montoya para desplegar la aplicación: https://github.com/st0263eafit/st026320212/tree/main/docker-nginx-wordpress-ssl-letsencrypt

# Ejecución
Para acceder solo debe ingresar a alguno de los siguientes enlaces:

https://proyectos-eafit.tk/

https://35.238.91.131/

# Cibergrafías
- https://labarta.es/instalar-wordpress-mysql-nginx-y-ssl-con-docker-compose/
- https://www.youtube.com/watch?v=N3xWxZt8x2s&list=PLxiktXxIte0tbF1UXLAAGk-1vyssaU4dd&index=36&t=53s
- https://cloud.google.com/filestore/docs/quickstart-console
- https://github.com/st0263eafit/st026320212/tree/main/docker-nginx-wordpress-ssl-letsencrypt
