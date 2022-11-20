# Memoria Phoronix y JMeter

Germán José Padua Pleguezuelo

## Ejercicio 1: Phoronix

### Instalación en Ubuntu

Descargamos el paquete que contiene a la aplicación con el comando:

```bash
wget http://phoronix-test-suite.com/releases/repo/pts.debian/files/phoronix-test-suite_10.8.3_all.deb
```

Ahora tenemos dos opciones para instalar el programa. Con el comando `dpkg`  y `apt install` , o con el instalador de paquetes gdebi. Si optamos por esta segunda opción, debemos instalar gdebi en primer lugar. Para ello:

```bash
sudo apt update
sudo apt install gdebi
sudo gdebi phoronix-test-suite_10.8.3_all.deb
sudo apt install php-sqlite3
sudo apt-get install lib32z1-dev
```

Los dos últimos comandos son necesarios ya que sino nos dará error de dependencias al intentar ejecutar phoronix o algún test.



### Instalación en CentOS

Descargamos el paquete correspondiente con:

```bash
sudo wget https://phoronix-test-suite.com/releases/phoronix-test-suite-10.8.3.tar.gz
```

Descomprimimos e instalamos:

```bash
sudo tar xvfz phoronix-test-suite-10.8.3.tar.gz
cd phoronix-test-suite
sudo ./install-sh
```

Para que no ocurra ningún error de dependencias ejecutaremos:

```bash
sudo yum install php-sqlite3
```



### Ejecución de Benchmarks

En primer lugar, tendremos que buscar dos tests en lá página https://openbenchmarking.org/tests . Después de buscar y probar unos cuantos,  me he quedado con dos que no ocupan mucho espacio y no tardan mucho en ejecutarse.



#### Test 1: blake2

Blake2 es una función hash de encriptación más rápida que MD5, SHA-1, SHA-2 y SHA-3 (http://www.blake2.net/). 

Para ejecutar el test, introducimos el comando `phoronix-test-suite benchmark blake2`  en ambas máquinas.

Resultados en Ubuntu:

![phoronix1](/home/germanpadua/Desktop/phoronix/phoronix1.png)

Observamos que podemos ver el resultado en https://openbenchmarking.org/result/2205241-NE-BLAKE2UBU09

![phoronix2](/home/germanpadua/Desktop/phoronix/phoronix2.png)

![phoronix3](/home/germanpadua/Desktop/phoronix/phoronix3.png)

![phoronix4](/home/germanpadua/Desktop/phoronix/phoronix4.png)













Resultados en CentOS:

![phoronix5](/home/germanpadua/Desktop/phoronix/phoronix5.png)



Observamos que podemos ver el resultado en https://openbenchmarking.org/result/2205246-NE-BLAKE2CEN20



![phoronix6](/home/germanpadua/Desktop/phoronix/phoronix6.png)

![phoronix7](/home/germanpadua/Desktop/phoronix/phoronix7.png)



Notemos que en CentOS se obtiene un mejor resultado ya que el número de Ciclos por Byte es menor.











































#### Test 2: Webp

Este es un test de libwebp de Google con el codificador de imágenes webp y usando una imagen JPEG de 6000x4000 píxeles como entrada.

Para ejecutarlo, introducimos el comando `phoronix-test-suite benchmark webp`  en ambas máquinas.

Resultados en Ubuntu:

![phoronix8](/home/germanpadua/Desktop/phoronix/phoronix8.png)

Seleccionamos la configuración por defecto.

![phoronix10](/home/germanpadua/Desktop/phoronix/phoronix10.png)

Podemos ver el resultado en https://openbenchmarking.org/result/2205245-NE-WEBPUBUNT49

![phoronix13](/home/germanpadua/Desktop/phoronix/phoronix13.png)

![phoronix14](/home/germanpadua/Desktop/phoronix/phoronix14.png)

![phoronix15](/home/germanpadua/Desktop/phoronix/phoronix15.png)



Resultados en CentOS:

![phoronix9](/home/germanpadua/Desktop/phoronix/phoronix9.png)

![phoronix11](/home/germanpadua/Desktop/phoronix/phoronix11.png)

![phoronix12](/home/germanpadua/Desktop/phoronix/phoronix12.png)



Vemos que en este test Ubuntu tiene un mejor resultado ya que el tiempo de codificación es menor.



### Ejercicio Opcional

Una vez instalado docker (veremos en el segundo ejercicio cómo) en nuestra máquina, ejecutamos `sudo docker run -it phoronix/pts` tal y como se indica en https://www.phoronix.com/scan.php?page=article&item=docker-phoronix-pts&num=1 . 

Pero nos encontramos un problema al intentar ejecutar los test con `benchmark blake2` y `benchmark webp`, ya que debemos instalar dependencias en ubuntu.

Otra forma de realizar el ejercicio será instalar un contenedor de ubuntu y en él instalaremos phoronix.

Para ello, en nuestra máquina ejecutamos:

```bash
sudo docker pull ubuntu
docker run -it ubuntu /bin/bash
```

![phoronix19](/home/germanpadua/Desktop/phoronix/phoronix19.png)



E instalamos phoronix como en ubuntu:

```bash
wget http://phoronix-test-suite.com/releases/repo/pts.debian/files/phoronix-test-suite_10.8.3_all.deb
sudo apt update
sudo apt install gdebi
sudo gdebi phoronix-test-suite_10.8.3_all.deb
sudo apt install php-sqlite3
sudo apt-get install lib32z1-dev
```



Ejecutamos el test blake2 con `phoronix-test-suite benchmark blake2` y obtenemos:

![phoronix20](/home/germanpadua/Desktop/phoronix/phoronix20.png)

Visitando la página https://openbenchmarking.org/result/2205256-NE-BLAKE2DOC27

![phoronix21](/home/germanpadua/Desktop/phoronix/phoronix21.png)

![phoronix22](/home/germanpadua/Desktop/phoronix/phoronix22.png)



Y vemos que el resultado es mejor a los obtenidos con las máquinas virtuales.



















## Ejercicio 2

### Instalación de Docker

Seguimos las instrucciones de la documentación oficial para instalar docker, https://docs.docker.com/engine/install/ubuntu/#set-up-the-repository

![jmeter1](/home/germanpadua/Desktop/jmeter/jmeter1.png)



Añadimos el repositorio:

```bash
 sudo apt-get update
 sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

Instalamos docker y docker compose:

```bash
sudo apt update
sudo apt install docker-ce docker-compose
```

Añadimos el usuario al grupo docker, para así no tener que usar sudo más tarde:

```bash
sudo usermod -aG docker germanpp
```

Y como vemos que está inactivo deberemos ejecutar `sudo systemctl start docker`

![jmeter2](/home/germanpadua/Desktop/jmeter/jmeter2.png)



Reiniciamos la máquina para hacer efectivos los cambios de añadir el usuario.

Si ejecutamos `docker run hello-world` observamos que funciona correctamente:

![jmeter4](/home/germanpadua/Desktop/jmeter/jmeter4.png)



### Instalación de JMeter en nuestra máquina

Descargamos la versión correspondiente de la página oficial https://jmeter.apache.org/download_jmeter.cgi . Lo descomprimimos y en la carpeta bin ejecutamos jmeter.

![jmeter5](/home/germanpadua/Desktop/jmeter/jmeter5.png)

Nos encontramos con un error ya que no tengo instalado java 11. Lo instalaremos con:

```bash
sudo apt update
sudo apt install openjdk-11-jdk
```

Y ya podemos ejecutar jmeter.



### Realización del ejercicio

Clonamos el repositorio en Ubuntu con 

`git clone https://github.com/davidPalomar-ugr/iseP4JMeter.git`

![jmeter6](/home/germanpadua/Desktop/jmeter/jmeter6.png)

Nos dirigimos al directorio principal y ejecutamos `docker-compose up` :

![jmeter7](/home/germanpadua/Desktop/jmeter/jmeter7.png)

Es posible que nos de un error si no tenemos espacio suficiente. Así que tuve que volver a hacer todos estos pasos en una instantánea anterior en la que tenía más memoria libre.

Una vez completado, si habilitamos el puerto 3000 con `sudo ufw allow 3000/tcp` tal  y como pone en la documentación del git. Nos dirigimos a nuestro navegador http://192.168.56.105:3000/ donde se presenta la descripción básica de la api.

Ya tenemos todo listo para seguir con el ejercicio. Volvemos a JMeter y creamos el test con las correspondientes condiciones:

En primer lugar, vamos a parametrizar el host y el puerto. Podemos añadir las variables con el botón Add y cambiamos los campos dejándolos así:

![jmeter8](/home/germanpadua/Desktop/jmeter/jmeter8.png)



A continuación, debemos crear los dos grupos de hebras que nos pide el ejercicio. 

Pulsamos en Edit -> Add -> Threads -> Thread Group . Y modificamos los campos ( como valores he puesto los que aparecen en el ejemplo de la documentación oficial https://jmeter.apache.org/usermanual/build-web-test-plan.html ):

![jmeter9](/home/germanpadua/Desktop/jmeter/jmeter9.png)

Volvemos a hacerlo pero para Administradores:

![jmeter10](/home/germanpadua/Desktop/jmeter/jmeter10.png)



Como son peticiones http, nos dirigimos desde ETSII Alumnos API a 

Edit -> Add -> Config Element -> HTTP Request Defaults. Lo rellenamos tal que así:

![jmeter11](/home/germanpadua/Desktop/jmeter/jmeter11.png)



Lo siguiente es crear una petición http para hacer el Login del Alumno. Desde Alumnos hacemos Edit -> Add -> Sampler -> HTTP Request. Viendo la descripción de la api, es una petición POST a la dirección /api/v1/auth/login : 

![jmeter13](/home/germanpadua/Desktop/jmeter/jmeter13.png)



Necesitamos ahora los credenciales de los alumnos que se encuentran en el archivo alumnos.csv . Así que lo tendremos que descargar del git. Hacemos click derecho sobre Login Alumno -> Add -> Config Element -> CSV Data Set Config . Indicamos la ruta del archivo alumnos.csv y modificamos los parámetros dejándolos así (como nombre pondremos Credenciales Alumnos, más tarde lo cambio para que se quede como en la foto del git) : 

![jmeter14](/home/germanpadua/Desktop/jmeter/jmeter14.png)



Ahora necesitamos añadir el token JWT que devolverá el usuario que hizo la petición.  Este token tendremos que almacenarlo para realizar las peticiones GET futuras. Desde Login Alumno, pulsamo en Edit -> Add -> Post Processor -> Regular Expression Extractor. Lo modificaremos hasta quedar así: 

![jmeter15](/home/germanpadua/Desktop/jmeter/jmeter15.png)



Añadimos ahora la pausa aleatoria Gaussiana con click derecho en Alumnos -> Add -> Timer -> Gaussian Random Timer. 

![jmeter16](/home/germanpadua/Desktop/jmeter/jmeter16.png)



El siguiente paso es realizar una petición GET para recuperar los datos del alumno. Haremos como con la petición POST. Click derecho sobre Alumnos -> Add -> Sampler -> HTTP Request. Cambiamos el tipo y elegimos GET. El parámetro path depende del usuario, buscando encontré en https://stackoverflow.com/questions/14593183/url-encode-variable-in-jmeter la siguiente solución:

![jmeter17](/home/germanpadua/Desktop/jmeter/jmeter17.png)



Por último creamos un gestor de cabeceras HTTP que usa el JWT anterior. Click derecho en Alumnos -> Add -> Config Element -> HTTP Header Manager y lo ponemos debajo de Recuperar Datos Alumno. Añadimos el parámetro tal y como se indica en la descripción de la api:

![jmeter18.3](/home/germanpadua/Desktop/jmeter18.3.png)

Ahora mismo tenemos:

![jmeter18](/home/germanpadua/Desktop/jmeter/jmeter18.png)

Y buscamos:

![jmeter40](/home/germanpadua/Desktop/jmeter40.png)



Así que movemos Credenciales Alumnos a la posición correcta y seguimos con Administradores.

Pero antes, vamos a añadir la autorización para la API (Basic-Auth Api) con click derecho en ETSII Alumnos API -> Add -> Config Element -> HTTP Authorization Manager y lo movemos debajo de Access to ETSII API. Los datos los sacamos de la descripción básica de la api en  http://192.168.56.105:3000/

![jmeter31](/home/germanpadua/Desktop/jmeter/jmeter31.png)


Terminemos los Administradores. Comenzamos con click derecho sobre Administradores -> Add -> Config Element -> CSV Data Set Config y hacemos igual que con Alumnos pero con el fichero administradores.csv : 

![jmeter20](/home/germanpadua/Desktop/jmeter/jmeter20.png)


Realizamos el login de los administradores:

![jmeter21](/home/germanpadua/Desktop/jmeter/jmeter21.png)

Obtenemos y almacenamos el JWT:

![jmeter22](/home/germanpadua/Desktop/jmeter/jmeter22.png)



Para obtener los datos de diversos alumnos añadimos un Access Log Sampler con click derecho sobre Administradores -> Add -> Sampler -> Access Log Sampler. En Log File indicamos el fichero apiAlumnos.log

![jmeter23](/home/germanpadua/Desktop/jmeter/jmeter23.png)

Añadimos ahora un gestor de cabecera como en Alumnos:
![jmeter24](/home/germanpadua/Desktop/jmeter/jmeter24.png)

Y por último nos queda añadir la otra espera aleatoria. 

![jmeter25](/home/germanpadua/Desktop/jmeter/jmeter25.png)



Añadimos los Listeners para visualizar los resultados con click derecho en ETSII Alumnos API -> Add -> Listener -> Summary Report / View Results in Table / Aggregate Report. Y ejecutamos el test con el botón de Start.

![jmeter26](/home/germanpadua/Desktop/jmeter/jmeter26.png)

![jmeter27](/home/germanpadua/Desktop/jmeter/jmeter27.png)

![jmeter28](/home/germanpadua/Desktop/jmeter/jmeter28.png)

En Ubuntu:

![jmeter29](/home/germanpadua/Desktop/jmeter/jmeter29.png)



Obtenemos un error y si nos fijamos es por no haber movido correctamente algunas cosas. Si colocamos correctamente Credenciales Alumnos y JWT Token de Accesos Administradores vemos que ya funciona correctamente.

![jmeter34](/home/germanpadua/Desktop/jmeter/jmeter34.png)

![jmeter35](/home/germanpadua/Desktop/jmeter/jmeter35.png)

![jmeter36](/home/germanpadua/Desktop/jmeter/jmeter36.png)

![jmeter37](/home/germanpadua/Desktop/jmeter/jmeter37.png)
