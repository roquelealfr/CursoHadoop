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
    * [Aprendiendo sobre los datos en Hadoop](#aprendiendo-sobre-los-datos-en-hadoop)
    * [YARN](#yarn)
3. [MapReduce](#mapreduce)
    * [MapReduce](#mapreduce-1)
    * [Comprender el funcionamiento de MapReduce](#comprender-el-funcionamiento-de-mapreduce)
    * [Identificar tipos de datos en herramientas de BigData](#identificar-tipos-de-datos-en-herramientas-de-bigdata)
4. [Operaciones con hadoop](#operaciones-con-hadoop)
    * [Desarrollar un clúster de Hadoop](#desarrollar-un-clúster-de-hadoop)
    * [Administrar un clúster de Hadoop](#administrar-un-clúster-de-hadoop)
5. [Conociendo el toolkit de Hadoop](#conociendo-el-toolkit-de-hadoop)
    * [Conocer Flume](#conocer-flume)
    * [Conocer Sqoop e integrar Hbase](#conocer-sqoop-e-integrar-hbase)
    * [Conocer Parquet y Avro](#conocer-parquet-y-avro)
    * [Generando archivos tipo Parquet](#generando-archivos-tipo-parquet)
    * [Conocer Hive](#conocer-hive)
    * [Zookeper](#zookeper)

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
La imagen pesa 4.4GB, así que se demorará en descargar.

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

Found 6 items
drwxrwxrwx   - root supergroup          0 2021-01-17 17:42 /user/hive/warehouse/categories
drwxrwxrwx   - root supergroup          0 2021-01-17 17:43 /user/hive/warehouse/customers
drwxrwxrwx   - root supergroup          0 2021-01-17 17:45 /user/hive/warehouse/departments
drwxrwxrwx   - root supergroup          0 2021-01-17 17:46 /user/hive/warehouse/order_items
drwxrwxrwx   - root supergroup          0 2021-01-17 17:47 /user/hive/warehouse/orders
drwxrwxrwx   - root supergroup          0 2021-01-17 17:49 /user/hive/warehouse/products
```
Si vemos una tabla nos mostrará los archivos que esta contiene, por supuesto, lo que vemos en consola son carpetas, así que si accedemos a ella podremos ver la metadata y el archivo parquet que contiene la información de la tabla.
```bash
hdfs dfs -ls /user/hive/warehouse/categories
Found 3 items
drwxr-xr-x   - root supergroup          0 2021-01-17 17:41 /user/hive/warehouse/categories/.metadata
drwxr-xr-x   - root supergroup          0 2021-01-17 17:42 /user/hive/warehouse/categories/.signals
-rw-r--r--   1 root supergroup       1956 2021-01-17 17:42 /user/hive/warehouse/categories/b6ec217b-98dc-4d82-bd25-b1eeb85f6871.parquet
```
Ingresamos a la plataforma de HUE, localhost:8888.

![HUE](/images/hue.png)

user: cloudera - pass: cloudera

Entramos al servicio de impala y corremos los siguientes comandos.
```
invalidate metadata;

show tables;

 	name
1	categories
2	customers
3	departments
4	order_items
5	orders
6	products
```
Y ahora, utilizamos una consulta SQL para ver las 10 categorías más usadas en la DB.
```bash
select c.category_name, count(order_item_quantity) as count
from order_items oi
inner join products p on 
oi.order_item_product_id = p.product_id
inner join categories c on c.category_id = p.product_category_id
group by c.category_name
order by count desc
limit 10;

 	category_name	        count
1	Cleats	                24551
2	Men's Footwear	        22246
3	Women's Apparel	        21035
4	Indoor/Outdoor Games	19298
5	Fishing	                17325
6	Water Sports	        15540
7	Camping & Hiking	    13729
8	Cardio Equipment	    12487
9	Shop By Sport	        10984
10	Electronics	            3156
```

Lo que hicimos fue Tener una DB SQL, moverla hacia una entorno de Hadoop, se comprimió esta DB y lo que se obtuvo fue un resultado de las categorías.

Del lado izquierdo de la plataforma de Hadoop podemos ver las tablas, podemos ver que están comprimidos en una versión de snappy y están guardados dentro del ambiente HDFS, cada uno redujo su tamaño para optimizar los recursos que se estén utilizando.

![Plataforma](/images/platform.png)

## Aprendiendo sobre los datos en Hadoop

Formatos de datos que podemos utilizar en Hadoop.

* Se genera una red:
```
sudo docker network create --driver=bridge hadoop
```
Para esta clase se tiene una serie de datos preparados en la [siguiente carpeta](/code/curso-hadoop-platzi/2_Aprendiendo_sobre_los_datos_en_hadoop).
* Levantamos nuestro ambiente.
    ```
    sudo ./start-container.sh
    ```
* En otra terminal nos dirigimos a la [documentación del repo](/code/curso-hadoop-platzi/2_Aprendiendo_sobre_los_datos_en_hadoop/README.md) y descargamos los libros.
    ```bash
    wget -b https://raw.githubusercontent.com/terranigmark/curso-hadoop-platzi/2_Aprendiendo_sobre_los_datos_en_hadoop/2_Aprendiendo_sobre_los_datos_en_hadoop/books/Alices_adventures.txt
    wget -b https://raw.githubusercontent.com/terranigmark/curso-hadoop-platzi/2_Aprendiendo_sobre_los_datos_en_hadoop/2_Aprendiendo_sobre_los_datos_en_hadoop/books/SYMBOLIC_LOGIC.txt
    wget -b https://raw.githubusercontent.com/terranigmark/curso-hadoop-platzi/2_Aprendiendo_sobre_los_datos_en_hadoop/2_Aprendiendo_sobre_los_datos_en_hadoop/books/the_hunting_of_snark.txt
    ```
* Dentro del ambiente de hadoop se crea otro directorio y se pegan los comandos para descargar los libros.
    ```
    mkdir input

    wget....

    ls -la
    total 648
    drwxr-xr-x 2 root root   4096 Jan 18 03:13 .
    drwx------ 1 root root   4096 Jan 18 03:13 ..
    -rw-r--r-- 1 root root 107595 Jan 18 03:13 Alices_adventures.txt
    -rw-r--r-- 1 root root 474438 Jan 18 03:13 SYMBOLIC_LOGIC.txt
    -rw-r--r-- 1 root root  54270 Jan 18 03:13 the_hunting_of_snark.txt
    -rw-r--r-- 1 root root    813 Jan 18 03:13 wget-log
    -rw-r--r-- 1 root root   1336 Jan 18 03:13 wget-log.1
    -rw-r--r-- 1 root root    742 Jan 18 03:13 wget-log.2
    ```
* Borramos los logs.
    ```
    rm -f wget*
    ls -la
    total 636
    drwxr-xr-x 2 root root   4096 Jan 18 03:14 .
    drwx------ 1 root root   4096 Jan 18 03:13 ..
    -rw-r--r-- 1 root root 107595 Jan 18 03:13 Alices_adventures.txt
    -rw-r--r-- 1 root root 474438 Jan 18 03:13 SYMBOLIC_LOGIC.txt
    -rw-r--r-- 1 root root  54270 Jan 18 03:13 the_hunting_of_snark.txt
    ```
* volvemos a la carpeta anterior y se comprime mediante el algoritmo de tar.
    ```
    tar -czvf input lewis.tar.gz input/*

    input/Alices_adventures.txt
    input/SYMBOLIC_LOGIC.txt
    input/input
    input/the_hunting_of_snark.txt
    ```
    * c: Generar el archivo.
    * z: Comprime el archivo.
    * v: visualizarlo.
    * f: Darle nombre y ubicación.
* Se crea un directorio en el entorno de hadoop.
    ```
    hdfs dfs mkdir -p test

    hdfs dfs -ls
    ```
* ubicamos el archivo comprimido dentro de Hadoop.
    ```
    hdfs dfs -put input/lewis.tar.gz
    ```
* Si se lista se puede observar que quedó fuera de la carpeta input, se mueve a esta carpeta.
    ```
    hdfs dfs -mv lewis.tar.gz input
    ```
* Se ejecuta el siguiente comando para que se genere un job para que lea el archivo comprimido.
    ```
    hadoop jar $HADOOP_HOME/share/hadoop/mapreduce/sources/hadoop-mapreduce-examples-2.7.2-source.jar org.apache.hadoop.examples.WordCount input output
    ```
* Se ejecutó una tarea de mapreduce. Se listan los archivos de la carpeta output que se generó y se observan 2. El log que se generó muestra una lista de las palabras que están en el documento y la cantidad de veces que se repite en el documento.

## YARN

Yarn (Yet Another Resource Negotiator) es una pieza fundamental en el ecosistema Hadoop. Es el framework que permite a Hadoop soportar varios motores de ejecución incluyendo MapReduce, y proporciona un planificador agnóstico a los trabajos que se encuentran en ejecución en el clúster. Esta mejora de Hadoop también es conocida como Hadoop 2.

Yarn separa las dos funcionalidades principales: la gestión de recursos y la planificación y monitorización de trabajos. Con esta idea, es posible tener un gestor global (Resource Manager) y un Application Master por cada aplicación.

![YARN](/images/yarn.png)

Permite crear la arquitectura de la solución mediante este clúster de herramientas. Va a estar debajo del ecosistema de Hadoop y va a estar negociando las recursos de acuerdo a las capacidades de nuestro sistema. En este sentido es HDFS otras herramientas que se van a explorar en el curso.

# MapReduce

## MapReduce

MapReduce es un marco con la que podemos escribir aplicaciones para procesar grandes cantidades de datos, paralelamente, en grandes grupos de componentes de hardware de manera confiable.

El algoritmo MapReduce contiene dos tareas importantes, a saber Mapa y reducir. Mapa toma un conjunto de datos y se convierte en otro conjunto de datos, en el que los elementos se dividen en tuplas (pares clave/valor). En segundo lugar, reducir tarea, que toma la salida de un mapa como entrada y combina los datos tuplas en un conjunto más pequeño de tuplas. Como la secuencia de MapReduce el nombre implica, la reducción se realiza siempre después de que el mapa.

Este framework se puede explorar desde diferentes estructuras, como tal podemos levantar trabajos o estructuras con MapReduce y configurarlos.

![MapReduce](/images/mapreduce.png)

* Input: Es la entrada de la información.
* Splitting: Separa la información, en una arquitectura generalmente se configuran varios parámetros para separar nuestros datos. Por ejemplo, en un PB podemos separarlos en GB, MB, etc. para poder analizar estas fracciones de datos o en su conjunto.
* Mapping: Identifica elementos y los empieza a agrupar y a contar.
* Shuffling Sort: Organiza la información, estos parámetros se determinan cuando se define la estructura.
* Reducing: Reduce la información, en este caso, agrupó las palabras Big y data puesto que se repiten.
* Final Result: Podemos analizar bloques de información.

## Comprender el funcionamiento de MapReduce

* Paradigma MapReduce por lo general se basa en enviar el ordenador a donde residen los datos.
* MapReduce programa se ejecuta en tres etapas, a saber: mapa etapa, shuffle, y reducir.
    * Mapa etapa : El mapa o mapa de trabajo es la de procesar los datos de entrada. Por lo general, los datos de entrada se encuentra en la forma de archivo o directorio y se almacena en el sistema de archivos Hadoop (HDFS). El archivo de entrada se pasa a la función mapa línea por línea. El mapper procesa los datos y crea varios pequeños fragmentos de datos.
    * Reducir etapa: Esta etapa es la combinación de la reproducción aleatoria y la etapa Reducir. La pieza de trabajo es la de procesar los datos que llegan desde el mapa. Después de un proceso de elaboración, se genera un nuevo conjunto de la producción, que se almacena en el HDFS.
* Durante el trabajo, Hadoop MapReduce envía el mapa y reducir las tareas a los servidores correspondientes en el clúster.
* El marco de trabajo administra todos los detalles de los datos de tareas tales como la emisión, verificar la realización de las tareas, y copia de los datos en todo el clúster entre los nodos.
* La mayoría de los informáticos se lleva a cabo en los nodos de datos en los discos locales que reduce el tráfico de la red.
* Tras la finalización de las tareas asignadas, el grupo recopila y disminución de los datos que forman un resultado adecuado y lo envía de vuelta al servidor Hadoop.

### Cómo ejecutarlo en la terminal

* Nos dirigimos a la carpeta de trabajo [MapReduce](/code/curso-hadoop-platzi/3_MapReduce) e iniciamos los contenedores.
    ```
    sudo ./start-container.sh
    ```
* Levantamos la configuración de Hadoop.
    ```
    ./start-hadoop.sh
    ```
* Revisamos la versión de Hadoop y la de Java, ¿Por qué Java? Porque Hadoop es una tecnología que se desarrollo con Java.
    ```
    hadoop version
    Hadoop 2.7.2
    Subversion Unknown -r Unknown
    Compiled by root on 2016-05-27T18:05Z
    Compiled with protoc 2.5.0
    From source with checksum d0fda26633fa762bff87ec759ebe689c
    This command was run using /usr/local/hadoop/share/hadoop/common/hadoop-common-2.7.2.jar

    javac -version
    javac 1.7.0_201
    ```
* Generamos el arhivo WordCount de Java, cuyó código está en la [documentación de la carpeta](/code/curso-hadoop-platzi/3_MapReduce/README.md).
```java
import java.io.IOException;
import java.util.StringTokenizer;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

public class WordCount {

  public static class TokenizerMapper
       extends Mapper<Object, Text, Text, IntWritable>{

    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();

    public void map(Object key, Text value, Context context
                    ) throws IOException, InterruptedException {
      StringTokenizer itr = new StringTokenizer(value.toString());
      while (itr.hasMoreTokens()) {
        word.set(itr.nextToken());
        context.write(word, one);
      }
    }
  }

  public static class IntSumReducer
       extends Reducer<Text,IntWritable,Text,IntWritable> {
    private IntWritable result = new IntWritable();

    public void reduce(Text key, Iterable<IntWritable> values,
                       Context context
                       ) throws IOException, InterruptedException {
      int sum = 0;
      for (IntWritable val : values) {
        sum += val.get();
      }
      result.set(sum);
      context.write(key, result);
    }
  }

  public static void main(String[] args) throws Exception {
    Configuration conf = new Configuration();
    Job job = Job.getInstance(conf, "word count");
    job.setJarByClass(WordCount.class);
    job.setMapperClass(TokenizerMapper.class);
    job.setCombinerClass(IntSumReducer.class);
    job.setReducerClass(IntSumReducer.class);
    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(IntWritable.class);
    FileInputFormat.addInputPath(job, new Path(args[0]));
    FileOutputFormat.setOutputPath(job, new Path(args[1]));
    System.exit(job.waitForCompletion(true) ? 0 : 1);
  }
}
```
* en la terminal de Hadoop realizamos un update, e instalamos nano.
```
apt update

apt install nano
```
* Creamos el archivo WorkCount.java, lo abrimos, pegamos el código de java y guardamos el documento.
```
nano WordCount.java

# Clic derecho, pegar, CTRL + X para salir, Y para confirmar guardar el documento.
```
* Se crea un directorio 'input_file' y dentro de este se crea un documento con un texto cualquiera.
```
root@hadoop-master:~# mkdir input_file
root@hadoop-master:~# echo "Hola mundo desde mapreduce" > input_file/input.txt
```
* Ahora, dentro del ambiente de Hadoop se crea una carpeta para mover el input y correr la configuración de java para mapreduce.
```
oot@hadoop-master:~# hdfs dfs -mkdir /WordCountTutorial      
root@hadoop-master:~# hdfs dfs -mkdir /WordCountTutorial/Input
root@hadoop-master:~# hdfs dfs -put input_file/input.txt /WordCountTutorial/Input
hdroot@hadoop-master:~# hdfs dfs -ls /WordCountTutorial/Input
Found 1 items
-rw-r--r--   2 root supergroup         27 2021-01-18 04:20 /WordCountTutorial/Input/input.txt
```
* Creamos las clases para configurar el archivo de Java y correr el job dentro de mapreduce. Primero crearemos la carpeta de las clases, luego exportamos lasvariables de ambiente, las cuales nos van a permitir ejecutar nuestro archivo de configuración de Java. Luego se crea las clases de ambiente. Se crea un archivo jar `wc.jar` y se ejecuta alrededor del algoritmo de MapReduce.
```bash
mkdir tutorial_classes

# Creación de variables de ambiente.
root@hadoop-master:~# export JAVA_HOME=/usr/java/default
root@hadoop-master:~# export PATH=${JAVA_HOME}/bin:${PATH}
root@hadoop-master:~# export HADOOP_CLASSPATH=/usr/lib/jvm/java-7-openjdk-amd64/lib/tools.jar

# Creación de las clases de ambiente.
hadoop com.sun.tools.javac.Main WordCount.java
# Se revisan que existen. Son las extensiones .class
root@hadoop-master:~# ls -la
total 68
drwx------ 1 root root 4096 Jan 18 04:30 .
drwxr-xr-x 1 root root 4096 Jan 18 04:06 ..
-rw-r--r-- 1 root root 3106 Feb 20  2014 .bashrc
drwx------ 2 root root 4096 Jan 18 04:07 .cache
-rw-r--r-- 1 root root  140 Feb 20  2014 .profile
drwx------ 1 root root 4096 Jan 18 04:07 .ssh
-rw-r--r-- 1 root root 1739 Jan 18 04:30 WordCount$IntSumReducer.class
-rw-r--r-- 1 root root 1736 Jan 18 04:30 WordCount$TokenizerMapper.class
-rw-r--r-- 1 root root 1501 Jan 18 04:30 WordCount.class
-rw-r--r-- 1 root root 2089 Jan 18 04:14 WordCount.java
drwxr-xr-x 1 root root 4096 Jun  5  2020 hdfs
drwxr-xr-x 2 root root 4096 Jan 18 04:17 input_file
-rwxr-xr-x 1 root root  695 Jun  1  2020 run-wordcount.sh
-rwxr-xr-x 1 root root  120 Jun  1  2020 start-hadoop.sh
drwxr-xr-x 2 root root 4096 Jan 18 04:21 tutorial_classes

# creamos el archivo wc.jar
jar cf wc.jar WordCount*.class

# Ejecutamos el contador de palabras alrededor del algoritmo de MapReduce.
hadoop jar $PWD/wc.jar WordCount /WordCountTutorial/Input /WordCountTutorial/Output
```
Una vez que el job terminó se puede observar que es similar al obtenido en la clase anterior, sin embargo, la diferencia es que aquí se corrió nuestro propio contador de palabras y en este caso el algoritmo de MapReduce terminó de ejecutarse.

Se revisa el resultado final.
```
hdfs dfs -cat /WordCountTutorial/Output/part-r-00000
Hola	    1
desde	    1
mapreduce	1
mundo	    1
```

[Documentación: MapReduce Tutorial](https://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html)

## Identificar tipos de datos en herramientas de BigData

![Datos](/images/datos.png)

En Hadoop podemos usar diferentes tipos de datos, Json es el más popular, sin embargo, podemos usar archivos ya específicos para clusters o para big data, estos permite comprimir a altas cantidades de Bytes para nuestros archivos, destacan Avro y Parquet. Son archivos que podemos manejar dentro de nuestro ambiente HDFS.

# Operaciones con hadoop

## Desarrollar un clúster de Hadoop

Nos movemos a la [carpeta correspondiente](/code/curso-hadoop-platzi/4_Desarrollar_un_cluster_de_hadoop), y levantamos el contenedor de cloudera.
```
sudo docker run --hostname=quikstart.cloudera --privileged=true -it -v $PWD:/src -p 8888:8888 -p 8080:8080 -p 7180:7180 -p 3306:3306 cloudera/quickstart /usr/bin/docker-quickstart
```
Instalamos el manager de cloudera
```
/home/cloudera/cloudera-manager --express --force
```
En mi computador no ejecuta este manager, puesto que mi memoria ram es insuficiente. 

*Edit*: Funciona al agregarle el parámetro `--force`.

## Administrar un clúster de Hadoop

Levantamos el contenedore de cloudera.
```
sudo docker run --hostname=quikstart.cloudera --privileged=true -it -v $PWD:/src -p 8888:8888 -p 8080:8080 -p 7180:7180 -p 3306:3306 cloudera/quickstart /usr/bin/docker-quickstart
```
Instalamos el servicio
```
service hive-metastore start
```
Se reinicia el servicio
```
sudo sudo service hive-server2 restart
```
Nos dirigimos al explorador a ver las configuraciones que hacen falta.
```
127.0.0.1:7180

cloudera:cloudera
```
![Cloudera Manager](/images/cloudera-manager.png)

Debido a que mi computador no tiene la suficiente potencia, muestra muchos errores de configuración de las herramientas, por eso se muestrán íconos amarillos al lado de cada herramienta, tampoco los gráficos se muestran.

# Conociendo el toolkit de Hadoop

## Conocer Flume

s un producto que forma parte del ecosistema Hadoop, y conforma una solución Java distribuida y de alta disponibilidad para recolectar, agregar y mover grandes cantidades de datos desde diferentes fuentes a un data store centralizado.

Surge para subir datos de aplicaciones al HDFS de Hadoop.

Es un proyecto de Apache aún en incubación (una vez salió la versión 0.9.4 se decidió que debía rediseñarse).

Su Arquitectura se basa en flujos de streaming de datos, ofrece mecanismos para asegurar la entrega y mecanismos de failover y recuperación.

Ofrece una gestión centralizada.

Los conceptos que maneja Flume son:

* **Evento**: Un payload de bytes con encabezados opcionales que representan la unidad de datos que Flume puede transportar desde su punto de origen hasta su destino final.
* **Flujo**: Movimiento de eventos desde el punto de origen hasta su destino final.
* **Cliente**: Implementación que opera en el punto de origen de los eventos y los entrega a un agente Flume. Por ejemplo, Log4J appender de Flume es un cliente.
* **Agente**: Un proceso independiente que aloja los componentes Fume como las Fuentes (Source), canales y sumideros (Sink), tiene la capacidad de recibir, almacenar y reenviar eventos a su próximo destino.
* **Fuente (Source)**: Implementación que puede consumir eventos entregados a él a través de un mecanismo (por ejemplo, una fuente de Avro se puede utilizar para recibir los eventos Avro de clientes u otros agentes en el flujo). Cuando una fuente recibe un evento, se lo entrega a uno o más canales.
* **Canal (Channel)**: es un almacenamiento temporal para eventos, donde los eventos se entregan al canal a través de fuentes que operan con del agente.
    Un evento puesto en un canal permanece en ese canal hasta que un Sumidero (Sink) lo elimina.
    Un ejemplo de canal es el canal de JDBC que utiliza una base de datos embebida para persistir los eventos que se eliminan por un sumidero. Los canales desempeñan un papel importante en garantizar la durabilidad de los flujos.

* **Sumidero (Sink)**: Implementación que puede eliminar eventos de un canal y transmitirlos al siguiente agente en el flujo, o hasta el destino final del evento.
    Los sumideros que transmiten el evento hacia su destino final son también conocidos como sumideros de terminales.

Los elementos se relacionan así:

![Flume](/images/flume.png)

Flume nos va a permitir mover datos, transmitir datos de un lado a otro.

En la terminal, ubicados en la [carpeta de Flume](/code/curso-hadoop-platzi/5_Conocer_Flume), seguimos los siguientes pasos:
* Construimos el contenedor de flume en docker.
```
sudo docker build -t myflume .
```
* Levantamos el contenedor creado.
```
sudo docker run -it --rm -p 9090:9090 myflume
```
* Con esto ya podemos empezar a monitorear datos. Ahora, en la última línea tiene que decir started para empezar a monitorear comunicación. En ella escribimos nuestro mensaje: `hola mundo desde flume`.
* en otra terminal usamos el siguiente comando para ver la transmisión de datos.
```
curl -X POST -H 'Content-Type: application/json; charset=UTF-8' -d '[{"headers":{"header.key":"header.value"}, "body":"Ha ingresado un nuevo estudiante"}]' 127.0.0.1:9090
```
* En la terminal de flume vemos el siguiente contenido:
```
2021-01-18 21:09:06,672 (SinkRunner-PollingRunner-DefaultSinkProcessor) [INFO - org.apache.flume.sink.LoggerSink.process(LoggerSink.java:94)] Event: { headers:{header.key=header.value} body: 48 61 20 69 6E 67 72 65 73 61 64 6F 20 75 6E 20 Ha ingresado un  }
```
Esta es una comunicación que nos va a permitir ver datos en tiempo real. Podemos ver que funciona a partir de agentes y que se puede observar la comunicación a partir de la terminal.

## Conocer Sqoop e integrar Hbase

Con Sqoop se puede realizar flujos de DB hacia Hadoop.

Sqoop es una herramienta cuya principal funcionalidad es transferir datos entre bases de datos relacionales o Data Warehouse y Hadoop. Sqoop automatiza la mayor parte de los procesos de transferencia, basándose en la base de datos para describir el esquema de los datos a importar, además para su funcionamiento utiliza MapReduce para importar y exportar los datos, lo que proporciona una operación en paralelo, así como tolerancia a fallos.

Sqoop le permite a los usuarios especificar la ubicación de destino dentro de Hadoop (pueden tablas Hive o HBASE) e instruir a Sqoop para mover datos de Oracle, Sql Server, Teradata u otras bases de datos relacionales al destino.

### ¿Cómo Trabaja Sqoop?

Sqoop funciona como una capa intermedia entre las bases de datos relacionales y Hadoop:

![Sqoop](/images/Sqoop.jpeg)

#### Import Sqoop

Sqoop escribe desde las tablas o consultas Sql específicas registro por registro paralelamente, por lo cual el resultado pueden ser múltiples archivos almacenados en HDFS con una copia de los datos importados. Estos archivos podrían ser txt separados por comas o tabulaciones, binarios Avro o SequenceFiles

![Sqoop Import](/images/sqoop-import.png)

#### Argumentos Comando Import

Al ver la siguiente lista de argumentos para realizar la importación, podemos ver que el proceso inicial no es del otro mundo y utiliza casi la misma estructura que si estuviéramos realizando una conexión a una base de datos específica desde cualquier lenguaje de programación, adicionándole por supuesto los datos de Hadoop:

![Argumentos Sqoop](/images/Command-Import.jpeg)

Por ejemplo:

Importando desde Mysql
```bash
$ sqoop import –connect jdbc:mysql://database.example.com/employees –username jacagudelo –password 678456
```
Importando desde SQl Server
```
$ sqoop import –driver com.microsoft.jdbc.sqlserver.SQLServerDriver –connect <connect-string> …
```
 
Seleccionando datos a importar

Generalmente Sqoop selecciona todos los campos de la tabla o vista origen a importar manteniendo el orden natural de los mismos.
```
$ sqoop import   –query ‘SELECT a.*, b.* FROM a JOIN b on (a.id == b.id) WHERE $CONDITIONS’ –split-by a.id /

–target-dir /user/foo/joinresults
```
 
#### Donde guarda los datos importados?

Por defecto Sqoop almacena los archivos de la importación en el directorio /foo dentro del directorio principal del sistema HDFS. Por ejemplo si el usuario utilizado es jacagudelo, Sqoop dejará los archivos de los resultados en la ruta /usr/jacagudelo/foo/(files).

Para ajustar este valor podemos hacer lo siguientes:
```
$ sqoop import –connnect <connect-str> –table emp –target-dir /dest
```
 
### Export Sqoop

La herramienta de exportación exporta un conjunto de archivos de HDFS a un RDBMS. Los archivos dados como entrada a Sqoop contienen registros, que se llaman como filas en la tabla. Éstos se leen y analizan en un conjunto de registros y se delimitan con el delimitador especificado por el usuario.

![Export Sqoop](/images/Export-Sqoop.png)

#### Argumentos Comando Export

![Argumentos Export](/images/Command-Export.jpeg)

Los principales parametros  utilizar son `––export-dir` el cual especifica el directorio en HDFS que contiene los datos de los archivos y  `––table` o `––call` que identifican respectivamente la tabla destina o procedimiento almacenado a ejecutar.

Algo a tener en cuenta al momento de exportar es que Sqoop toma por defecto todas las columnas, pero podremos especificar columnas puntuales con el comando `––columns` delimitándolas por comas. La advertencia aquí es que las colummnas que no se eincluyan en el proceso serán enviadas con valor NULL por defecto por lo que si la tabla destino no tiene configurado dicho permiso terminará generando error el proceso Sqoop.

Por ejemplo:
```
$ sqoop export –connect jdbc:mysql://db.example.com/foo –table retail –export-dir /results/bar_data
```

Sqoop por defecto realiza un append en la tabla de destino, en esencia realiza un insert sobre cada registro. Al igual que con los campos en el caso anterior, las tablas destino podrían tener Primary Key con los cual podrían generar error de duplicidad. Este modo está destinado principalmente a exportar registros a una nueva tabla vacía destinada a recibir estos resultados.

En caso de que el destino sea una tabla existente, debemos adicionar el parámetro – – update – key donde Sqoop realizará la modificación del conjunto de datos existente en la base de datos destino. Cada registro de entrada se tratará como una instrucción UPDATE que modifica una fila existente. La modificación de la fila se determina por el nombre de columna especificado como llave –update-key.

Si ejecutamos el comando:
```
$ sqoop export –table bar –update-key id  –connect jdbc:mysql://db.example.com/foo  –export-dir /results/bar_data
```
Internamente el Job de Sqoop realizará lo siguiente:
```
UPDATE foo SET msg=’this is a test’, bar=42 WHERE id=0;

UPDATE foo SET msg=’some more data’, bar=100 WHERE id=1;
```
 
### Tipos de Destino en Hadoop

Una de las principales virtudes de Hadoop es que nos brinda una gran variedad de proyectos disponibles para usar de acuerdo a nuestras necesidades, para este caso puntual contamos con 3 proyectos específicos:

* **Hive**: Como vimos en el artículo Hive nos proporciona un ambiente de Data Warehouse sobre Hadoop con su propio lenguae de consulta  muy similar al SQL, lo cual nos vendría conveniente al querer explorar nuestros datos.
* **HBase**: HBase es una base de datos no relacional que nos permite realizar búsquedas rápidas de baja latencia en Hadoop. Agregar capacidades transaccionales a Hadoop, permitiendo a los usuarios realizar actualizaciones, inserciones y eliminaciones.
* **Accumulo**: Aunque muy poco conocida pero no menos importante, tenemos esta base de datos NoSql de tipo Key-Column la cual podemos utilizar a nuestras necesidades.

### Conclusión sobre Sqoop

Como acabamos de ver, Sqoop es una poderosa herramienta para poblar desde nuestras bases de datos relacionales y evolucionar nuestros proyectos analíticos, con sus comandos podemos trasladar nuestros datos, analizarlos mediante cualquier otra herramienta como Pig o Hive sobre Hadoop y volver a retornar el resultado a nuestras bases. Una funcionalidad muy práctica y eficaz para nuestros proyectos.

[Sqoop: Poblando Hadoop desde RDBMS](http://blog.jacagudelo.com/sqoop-hadoop/)

### Pasos realizados en la clase

* En la terminal, en el [directorio de Sqoop](/code/curso-hadoop-platzi/5_Conocer_Sqoop_e_integrar_Hbase), levantamos el servicio de cloudera:
```bash
sudo docker run --hostname=quickstart.cloudera --privileged=true -it -v $PWD:/src --publish-all=true -p 8888:8888 -p 8080:8080 -p 7180:7180 -p 3306:3306 cloudera/quickstart /usr/bin/docker-quickstart
```
* Una vez levantado el servicio, levantamos el servicio de Hbase para poder conectarnos y hacer consultas. En la consola de cloudera usamos:
```
hbase shell

2021-01-18 21:31:17,534 INFO  [main] Configuration.deprecation: hadoop.native.lib is deprecated. Instead, use io.native.lib.available
HBase Shell; enter 'help<RETURN>' for list of supported commands.
Type "exit<RETURN>" to leave the HBase Shell
Version 1.2.0-cdh5.7.0, rUnknown, Wed Mar 23 11:39:14 PDT 2016
```
* Creamos una tabla:
```
create 'newtbl', 'knowledge'

row(s) in 1.6110 seconds

=> Hbase::Table - newtbl
```
* Podemos usar acá comandos SQL para ver más información:
```
describe 'newtbl'

Table newtbl is ENABLED                                                         
newtbl                                                                          
COLUMN FAMILIES DESCRIPTION                                                     
{NAME => 'knowledge', DATA_BLOCK_ENCODING => 'NONE', BLOOMFILTER => 'ROW', REPLI
CATION_SCOPE => '0', VERSIONS => '1', COMPRESSION => 'NONE', MIN_VERSIONS => '0'
, TTL => 'FOREVER', KEEP_DELETED_CELLS => 'FALSE', BLOCKSIZE => '65536', IN_MEMO
RY => 'false', BLOCKCACHE => 'true'}                                            
1 row(s) in 0.2820 seconds
```
* Algo importante que resaltar es que se está trabajando con HDFS, lo que quiere decir que los datos están distribuidos en los clúster, a diferencia de otras DB.
* Para agregar datos se hace de la siguiente manera:
```
put 'newtbl', 'r1', 'knowledge:book','Alicia en el país de las maravillas'
0 row(s) in 0.2850 seconds

hbase(main):004:0> put 'newtbl', 'r2', 'knowledge:book','Odisea'
0 row(s) in 0.0200 seconds

hbase(main):005:0> scan 'newtbl'
ROW                   COLUMN+CELL                                               
 r1                   column=knowledge:book, timestamp=1611008004407, value=Alic
                      ia en el pa\xC3\xADs de las maravillas                    
 r2                   column=knowledge:book, timestamp=1611008032831, value=Odis
                      ea                                                        
2 row(s) in 0.0940 seconds
```
* Nos dirigimos al explorador para poder practicar. `127.0.0.1:8888`. *En este punto cloudera no funcionó en mi pc por falta de recursos.*
* Vamos a usar el servicio de impala.
* Invalidamos la metadata de scoop.
```
invalidate metadata;
```
* Mostramos las tablas.
```
show tables;
```
* Seleccionamos los datos.
```
select * from customers;
```
* Para visualisar los datos no relacionales vamos a Data Browsers > Hbase. Y ahí podemos ver los datos no relacionales que creamos.

## Conocer Parquet y Avro

Avro comprime archivos para la optimización de filas.

Parquet permite optimizar nuestros archivos de acuerdo a las columnas que tenemos en ellas.

Nos dirigimos a la [carpeta de la clase](/code/curso-hadoop-platzi/5_Conocer_Parquet_y_Avro).

* Levantamos los servicios de docker compose: `sudo docker-compose up`.
* En otra terminal, en la misma carpeta, usamos el siguiente comando para mover los datos.
```
sudo docker run --hostname=quickstart.cloudera --privileged=true -it -v $PWD:/src --publish-all=true -p 8888:8888 -p 8080:8080 -p 3306:3306 -p 7180:7180 cloudera/quickstart /usr/bin/docker-quickstart
```
* En otra terminal, primero verificamos los contenedores que están activos:
```
sudo docker ps

CONTAINER ID   IMAGE                 COMMAND                  CREATED              STATUS              PORTS                                                                                            NAMES
0ac544ff0bed   cloudera/quickstart   "/usr/bin/docker-qui…"   About a minute ago   Up About a minute   0.0.0.0:3306->3306/tcp, 0.0.0.0:7180->7180/tcp, 0.0.0.0:8080->8080/tcp, 0.0.0.0:8888->8888/tcp   blissful_kare
6f953f1cbfb1   dbs_hadoop_mysql      "docker-entrypoint.s…"   3 minutes ago        Up 3 minutes        3306/tcp, 33060/tcp                                                                              mysql_db
```
* Inspeccionamos el contenedor MySQL, con el fin de obtener la IP.
```
sudo docker inspect mysql_db

"IPAddress": "172.19.0.2",
```
* Nos conectamos.
```
mysql --host=172.19.0.2 --user=root --password=rootpwd -e "select * from db_name.user_details" -B > user_data.tsv
```
* Podemos ver tanto en la terminal de quickstart como en la de la DB de Hadoop que se encuentran juntas, esto porque es una bind mount.
```bash
# Terminal CursoHadoop/code/curso-hadoop-platzi/5_Conocer_Parquet_y_Avro/dbs_hadoop
ls -la
total 24
drwxrwxrwx 3 oscar-dev oscar-dev 4096 ene 18 18:22 .
drwxrwxrwx 3 oscar-dev oscar-dev 4096 ene 18 17:57 ..
-rwxrwxrwx 1 oscar-dev oscar-dev  167 ene 18 18:12 docker-compose.yml
-rwxrwxrwx 1 oscar-dev oscar-dev  248 ene  7 21:35 README.md
drwxrwxrwx 2 oscar-dev oscar-dev 4096 ene  7 21:35 SQL_db
-rw-rw-r-- 1 oscar-dev oscar-dev  701 ene 18 18:30 user_data.tsv

# Terminal Quickstart
ls -la
total 24
drwxrwxrwx 3 1000 1000 4096 Jan 18 23:22 .
drwxr-xr-x 1 root root 4096 Jan 18 23:15 ..
-rwxrwxrwx 1 1000 1000  167 Jan 18 23:12 docker-compose.yml
-rwxrwxrwx 1 1000 1000  248 Jan  8 02:35 README.md
drwxrwxrwx 2 1000 1000 4096 Jan  8 02:35 SQL_db
-rw-rw-r-- 1 1000 1000  701 Jan 18 23:30 user_data.tsv
```
* Comprimimos el elemento `user_data.tsv` en la terminal de quickstart.
```bash
gzip src/user_data.tsv

# Listamos
ls -la src
total 24
drwxrwxrwx 3 1000 1000 4096 Jan 18 23:38 .
drwxr-xr-x 1 root root 4096 Jan 18 23:15 ..
-rwxrwxrwx 1 1000 1000  167 Jan 18 23:12 docker-compose.yml
-rwxrwxrwx 1 1000 1000  248 Jan  8 02:35 README.md
drwxrwxrwx 2 1000 1000 4096 Jan  8 02:35 SQL_db
-rw-rw-r-- 1 1000 1000  444 Jan 18 23:30 user_data.tsv.gz
```
* Montamos el archivo en el servicio de Hadoop, pero primero nos ubicamos en la carpeta src.
```bash
hdfs dfs -mkdir /user/cloudera/raw_data/
hdfs dfs -put *.gz /user/cloudera/raw_data
hdfs dfs -ls /user/cloudera/raw_data

Found 1 items
-rw-r--r--   1 root cloudera        444 2021-01-19 00:00 /user/cloudera/raw_data/user_data.tsv.gz
```

## Generando archivos tipo Parquet

Nos dirigimos al navegador: `127.0.0.1::8080`. *El entorno no funciona debido a los pocos recursos de mi shitty machine*.
* Le damos a la opción Navigation > Hue UI.
* El usuario y contraseña es: cloudera:cludera.
* Query Editors.
    * Hive: Permite hace queries grandes para procesar datos.
    * Impala: Permite hacer queries cortas para procesar datos de forma más eficiente.
* Abrimos Hive:
    ```sql
    create database mysql_raw;
    # Si ocurre un error es porque la DB ya existe.

    # Creamos tabla externa en donde vamos a poner nuestra información.
    create external table if not exists
    mysql_raw.users_tsv(user_id int
    first_name string,
    username string,
    last_name string,
    gender string,
    password string,
    status, int)

    comment 'users' row format delimited fields terminated by '\t'
    stored as textfile LOCATION 'users/cloudera/raw_data/'
    tblproperties ("skip.header.line.count"="1");
    ```
* Para revisar que la tabla se haya creado usamos:
    ```sql
    show create table mysql_raw.users_tsv;
    ```
    Podemos observar el formato en el que se creo.
* Creamos una DB master:
    ```sql
    create database mysql_master;
    ```
* Creamos una tabla donde estos elementos van a estar guardados de una forma muy comprimida mediante parquet.
    ```sql
    create table mysql_master.users stored as parquet
    location '/user/cloudera/user_data.tsv.gz'
    tblproperties ('PARQUET.COMPRESS'='SNAPPY')
    AS SELECT * FROM mysql_raw.users_tsv;
    ```
* Visualizamos los datos de nuestra tabla.
    ```sql
    # De esta manera vemos que la tabla se haya creado.
    show create table mysql_master.users;

    # De esta manera visualizamos los datos creados.
    select * from mysql_master.users;
    ```

## Conocer Hive

No se hizo demostración con hive, simplemente se mostraron las gráficas que pueden obtenerse de los datos con esta herramienta, igualmente se mostró graficamente la información de las tablas. Los datos fueron comprimidos con Snappy, lo que quiere decir que es una DB muy pequeña.

## Zookeper

ZooKeeper es un proyecto de Apache que nos provee de un servicio centralizado para diversas tareas como por ejemplo mantenimiento de configuración, naming, sincronización distribuida o servicios de agrupación, servicios que normalmente son consumidos por otras aplicaciones distribuidas. 

Para ejecutar zookeeper en el manager de cloudera, le damos clic derecho a la herramienta y luego a start. Toma tiempo, pues es una herramienta muy pesada.

No se mostró usos, ni cómo levantar servicios con esta herramienta.