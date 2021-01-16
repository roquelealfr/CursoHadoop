# Curso de Hadoop

![Logo](https://static.platzi.com/media/achievements/badge-hadoop-f448c04e-7e3e-4d85-aeed-e6a262900683.png)

---

# Indice

1. [Introduccion a Hadoop](#introduccion-a-hadoop)
    * [Presentación del curso](#presentación-del-curso)
    * [Introducción a Hadoop](#introducción-a-hadoop)
    * [Usar Hadoop en lo profesional](#usar-hadoop-en-lo-profesional)
    * [Configurar nuestro entorno con Hadoop](#configurar-nuestro-entorno-con-hadoop)
    * [Crear tu propio dockerfile para tu entorno de pruebas con Hadoop](#crear-tu-propio-dockerfile-para-tu-entorno-de-pruebas-con-hadoop)
2. [Fundamentos de hadoop](#fundamentos-de-hadoop)
    * [Comparar otras herramientas frente a Hadoop](#comparar-otras-herramientas-frente-a-hadoop)
    * [Elementos de Hadoop HDFS](#elementos-de-hadoop-hdfs)
    * [Reconocer y diseñar flujo de datos](#reconocer-y-diseñar-flujo-de-datos)

---

# Introduccion a Hadoop

## Presentación del curso

### Data to BSN (Business)
* Recolección
* Análisis
* Custodia-trazabilidad
* "Convertir los datos en negocio"

## Introducción a Hadoop

![Por qué?](/images/por-que.png)

Hadoop: Nos permite un gran a análisis de información (terabytes, gigas,…). Es un conjunto de herramientas, software libre, escalable, distribuido (la información esta en clusters) y confiable.

Big data: Explorar esta información a grandes velocidades.

### ¿Cómo surge Hadoop?
Comenzó como un motor de búsqueda (Doug Cutting). Se unió con Cloudera.

### ¿Elementos de Hadoop?
YARN, MapReduce,HDFS

### ¿Qué es HADOOP?

* Es un conjunto de herramientas, es un ambiente de trabajo en el que se pueden explorar diferentes soluciones, en este sentido es un software libre que se puede usar y modificar. Es además escalable, porque se pueden crear clusters de información e irlas escalando horizontalmente para que se pueda albergar mayor análisis de procesamiento.
* Se puede instalar otras herramientas, de esta manera se puede escalar aún mas el volumen y el procesamiento de los datos.
* Es un sistema distribuido, como trabaja en clusters se puede conectar y desconectar estos servicios para administradorlos.
* La distribución hace que el sistema sea confiable. Como tiene servicios distribuidos se controla con másfacilidad quién entra, quién sale, cómo entra, cómo sale, cómo se almacena la información, y esto es lo que hace que Hadoop sea una herramienta poderosa.

## Usar Hadoop en lo profesional

SETI@home: Es un proyecto en el que se estaba buscando explorar la vida en otros planetas (1999). Se empieza a hablar de usar la computadoras de manera modular (cluster de computadoras).

### ¿Cómo es Hadoop en el mundo real?

Se puede obtener datos de diferentes fuentes (RRSS, compras, …). Hadoop nos puede ayudar a analizar toda esta información.

## Configurar nuestro entorno con Hadoop

* Virtualización de un entorno virtual de ubuntu con VirtualBox.
* [Play with Docker](https://labs.play-with-docker.com/).

## Crear tu propio dockerfile para tu entorno de pruebas con Hadoop

Lo primero que hacemos es crear una red de docker para que se conecte a Hadoop.
```bash
sudo docker network create --driver=bridge hadoop
[sudo] password for oscar-dev: 
0bc066ab1becf0eecce066ec810723ebc0f47bfa32aeaa6e3e9be514e9c311e6
```
El siguiente paso es iniciar los contenedores, para ello existe el archivo [start_container.sh](/code/curso-hadoop-platzi/1_Configurar_nuestro_entorno_con_Hadoop/start-container.sh).
```bash
sudo ./start-container.sh
start hadoop-master container...
start hadoop-slave1 container...
start hadoop-slave2 container...
start hadoop-slave3 container...
start hadoop-slave4 container...
start hadoop-slave5 container...
```
Dado que es la primera vez que se ejecuta, tiene que descargarse la imagen, eso toma tiempo. Una vez terminó, se queda en la terminal de la consola interactiva del contenedor.

Levantamos el servicio, usamos [./start-hadoop.sh](/code/curso-hadoop-platzi/1_Configurar_nuestro_entorno_con_Hadoop/config/start-hadoop.sh).

Ahora se crea un archivo de texto con la siguiente configuración:
```bash
echo "Texto para Hadoop." > ejemplo.txt

ls
ejemplo.txt  hdfs  run-wordcount.sh  start-hadoop.sh
```
Se mueve el archivo de texto al servicio de Hadoop que está corriendo.
```bash
hdfs dfs -mkdir -p platzi
```
Una vez el directorio esté creado, lo que vamos a hacer es mover ese archivo hacia la carpeta.
```bash
hdfs dfs -put ejemplo.txt platzi/
```
Esto es una práctica muy común en la que se mueven archivos de cualquier tipo, cosas que vamos a analizar en este tipo de entornos distribuidos.
```bash
hdfs dfs -ls platzi/

Found 1 items
-rw-r--r--   2 root supergroup         19 2021-01-14 02:27 platzi/ejemplo.txt
```
Bien, ahora a la página web: `localhost:50070`.

![Overview](/images/overview.png)

Para poder acceder al documento de texto que cargamos, vamos a la derecha arriba, le damos clic a 'utilities', luego a 'Browse the file system', y en la lista que nos sale, a la derecha podemos navegar entre carpetas.

![Platzi directory](/images/platzi-directory.png)

Así mismo, si le damos clic al nombre del archivo tendremos diferentes opciones, lo más resaltante de esto, es que podemos visualizar en qué nodos existe este archivo.

![Nodos](/images/nodos.png)

# Fundamentos de hadoop

## Comparar otras herramientas frente a Hadoop

En esta clase, se montará un contenedor con elastic search, a modo de comparación con Hadoop.

1. Creamos la red:
    ```bash
    sudo docker network create -d bridge hadoopNet
    [sudo] password for oscar-dev: 
    9527f244b5c7f0b4fb30159499d19afd7c4c3563991cfb1ad54e80a6ca62cfb5
    ```
2. Creamos el contenedor de elasticsearch exponiendo los puertos 9200 y 9300, asimismo, asignamos el entrypoint `"discovery.type=single-node"` e instalamos la imagen de elasticsearch 7.6.2.
    ```bash
    sudo docker run -p 9200:9200 -p 9300:9300 --net=hadoopNet --name elasticsearch -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.6.2
    Unable to find image 'docker.elastic.co/elasticsearch/elasticsearch:7.6.2' locally
    7.6.2: Pulling from elasticsearch/elasticsearch
    c808caf183b6: Pull complete 
    d6caf8e15a64: Pull complete 
    b0ba5f324e82: Pull complete 
    d7e8c1e99b9a: Pull complete 
    85c4d6c81438: Pull complete 
    3119218fac98: Pull complete 
    914accf214bb: Pull complete
    ```
3. En otra terminal, puesto que este proceso demora. En el directorio de trabajo trae un archivo denominado [pipeline](/code/curso-hadoop-platzi/2_Comparar_otras_herramientas_frente_a_Hadoop/pipeline), el cuál es un archivo de configuración de [logstash](/code/curso-hadoop-platzi/2_Comparar_otras_herramientas_frente_a_Hadoop/pipeline/logstash.conf) y contiene lo siguiente:
    ```bash
    # logstash.conf

    input {
        stdin {}
    }

    output {
        elasticsearch {
            hosts => ["elasticsearch:9200"]
        }
    }
    ```
    Estos nos van a permitir configurar las entradas y salidas de la información dentro del archivo de configuración. Se levanta el servicio de logstash.
    ```bash
    sudo docker run -it --rm --net=hadoopNet --name logstash -v "$PWD/pipeline":/app --entrypoint bash docker.elastic.co/logstash/logstash:7.6.2
    ```
4. Se necesita levantar un nuevo servicio que se llama Kibana. En otra terminal.
    ```bash
    sudo docker run --rm --name kibana --net=hadoopNet -p 5601:5601 kibana:7.6.2
    ```
5. Una vez los servicios estén montados, nos dirigimos al servicio del logstash, y verificamos que exista el directorio pipeline.
    Levantamos el servicio:
    ```bash
    logstash -f /app/logstash.conf
    ```
6. Verificamos que los servicios estén corriendo con `sudo docker ps`.
7. Inspeccionamos la red de Hadoop. Verificamos que estén conectados los 3 servicios.
    ```bash
    "Containers": {
        "385741a965cbf3b3204fcfdef916e65856b6b4389d035834b915db50182e9d0b": {
            "Name": "elasticsearch",
            "EndpointID": "2c40b207e2b7790347639fa429956ea2d3c57fffeda64eb9fab4cd98256498fe",
            "MacAddress": "02:42:ac:13:00:02",
            "IPv4Address": "172.19.0.2/16",
            "IPv6Address": ""
        },
        "654b8a72c283a942a288b27bc3883c20bfb2ec1f661bc578b740c4a38acd04ca": {
            "Name": "logstash",
            "EndpointID": "e01df9d95cbd17b2f26df76da16f85adfbe08dfd2720e47b380c73bff634f547",
            "MacAddress": "02:42:ac:13:00:03",
            "IPv4Address": "172.19.0.3/16",
            "IPv6Address": ""
        },
        "b919d9954f2f716d2e07fd11af981d3888769aa75e11b7fd01ce36726715645a": {
            "Name": "kibana",
            "EndpointID": "2e4a9ee90e8772de986e26a4b6a73a644c1f0a58471e07312877cdddd89a2a7e",
            "MacAddress": "02:42:ac:13:00:04",
            "IPv4Address": "172.19.0.4/16",
            "IPv6Address": ""
        }
    },
    ```
    El servicio de logstash nos muestra un mensaje que fue lanzado existosamente. De esta manera ya podemos enviar datos en tiempo real hacia nuestros servicios.
8. En la consola de logstash escribimos lo siguiente: "Platzi comparando herramientas de hadoop con elasticstack", y tipeamos en una terminal aparte lo siguiente:
    ```bash
    {
        "_index" : "logstash-2021.01.14-000001",
        "_type" : "_doc",
        "_id" : "1PO-AXcBxj718CtXwbW4",
        "_score" : 1.0,
        "_source" : {
        "host" : "654b8a72c283",
        "@timestamp" : "2021-01-14T16:33:34.516Z",
        "message" : "Platzi comparando herramientas de hadoop con elasticstack",
        "@version" : "1"
        }
    }
    ```
    Esta es la forma en la que se manda código desde la terminal hacia elastic search y esa transmisión de datos, algo similar que se puede hacer con Hadoop es mandar o hacer información, pero, la diferencia es que cuando se están analizando entornos con Hadoop lo más común es que son arquitecturas que se mandan por batch, entonces son bloques de archivos que se están mandando que pueden ser comprimidos, puede tener diferentes extensiones de bases de datos e inclusive archivos de texto o diferentes elementos que se pueden poner en esta composición distribuída, a diferencia de  elastic search. En elastic search existe una limitante que son archivos de tipo Json o log, o CSV que se están mandando.

## Elementos de Hadoop HDFS

![Arquitectura](/images/arquitectura.png)

HDFS (Hadoop Distributed File System) es el componente principal del ecosistema Hadoop. Esta pieza hace posible almacenar data sets masivos con tipos de datos estructurados, semi-estructurados y no estructurados como imágenes, vídeo, datos de sensores, etc. Está optimizado para almacenar grandes cantidades de datos y mantener varias copias para garantizar una alta disponibilidad y la tolerancia a fallos. Con todo esto, HDFS es una tecnología fundamental para Big Data, o dicho de otra forma, es el Big Data File System o almacenamiento Big Data por excelencia.

![Estructura](/images/estructura.png)

Comprende 2 grandes bloques.
* Job nodes: Procesa los datos distribuídos.
* Name nodes: Dónde se apunta la ubicación de dónde se encuentran los datos.
* Data nodes.
Viéndolo como un sistema un poco más simple, podríamos asimilarlo como el sistema de una biblioteca, la bibliografía o las tarjetas bibliográficas donde se concentra la información, esa sería nuestro name node, y la enciclopedia, dónde está el contenido, sería el data node. De esa forma forma, bastante distribuída, es como se gestionan las bibliotecas. Así mismo funciona Hadoop.

## Reconocer y diseñar flujo de datos

Nos dirigimos al directorio de cómo [reconocer y diseñar flujos de datos](/code/curso-hadoop-platzi/2_Reconocer_y_diseñar_flujo_de_datos).

Se crea el siguiente contenedor.
```bash
sudo docker run --hostname=quickstart.cloudera --privileged=true -it -v $pwd:/src --publish-all=true -p 8888:8888 -p 8080:8080 -p 7180:7180 cloudera/quickstart /usr/bin/docker-quickstart
```
La imagen pesa 4.4GB, así que se demorará en descargar. *No funciona*.

Se trabaja con una herramienta denominada sqoop. Permite que a través de una DB vamos a mover información a nuestro cluster de Hadoop.
```bash
sqoop import-all-tables -m 1 --connect jdbc:mysql://quickstart:3306/retail_db --username=retail_dba --password=cloudera --compression-codec=snappy --as-parquetfile --warehouse-dir=/user/hive/warehouse --hive-import
```
* `import-all-tables`: Importa todas las tablas de la DB.
* `-m 1`: Es la configuración del split de la información.
* `--connect`: Conecta a la DB mencionada en el parámetro.
* `--username`: Usuario de la DB.
* `--password`: Contraseña del usuario.
* `--compression-codec`: Nappy es una librería de compresión/descompresión, optimiza para compresión o descompresión ultra rápido.
* `--as-parquetfile`: Formato de archivo, es bastante ligero.
* `--warehouse-dir`: Dónde se va a guardar.
* `--hive-import`: Se utiliza para poder analizar los datos en hive.

Ahora, vemos el archivo de configuración.
```bash
hdfs dfs -ls /user/hive/warehouse/
```
