# Brainstorm
* Temática Big Data.
* Realizar una web que muestre datos de cómo evoluciona el término “Big Data” en Twitter, y mostrar una estimación futura.
* También sería ideal mostrar información de sentimiento a lo largo del tiempo con ese mismo término.
* Sería interesante hacerlo también con los empleos directamente relacionados cómo (Data Analyst, Data Engineer, Data Scientist)
* Cómo futuro proyecto de escalada se puede tener en cuenta buscar desde otras fuentes como Reddit o Linkedln.

# Diseño del DAaaS
## Definición la estrategia del DAaaS
Una página web que muestre un Dashboard donde se detallen gráficos y conclusiones de cómo evoluciona la percepción del término Big Data y los empleos relacionados, dentro de grandes redes sociales como pudieran ser twitter y reddit, que será de ayuda para resolver dudas sobre el futuro del mismo.

## Arquitectura DAaaS
A continuación se detalla las herramientas softwares necesarias para realizar la arquitectura.
* Una aplicación Web (Google Cloud Computer)
* Una base de datos SQL (Google Cloud SQL)
* Cluster de Hadoop (Google Cloud Dataproc)
* Cluster de Kafka (Google Cloud Platform)
* Script 1 de python (VM PUB)
 * Scrapper de twitter con la librería tweepy
 * Librería kafka para enviar los datos
* Script 2 de python (VM SUB)
 * Librería kafka para recibir los datos
 * Análisis de sentimientos con la librería TextBlob

## DAaaS Operating Model Design and Rollout
Lá carga máxima de procesamiento se efectuará en un **Google Cloud Dataproc**, este va a leer desde un **Bucket de Google Storage** todos los datos aportado por el scrip 2 de python desde la **VM SUB** (Los datos de twitter optimizados con kafka, y con el análisis de sentimientos)
1. El **script 1** (scrapper de twitter), **KAFKA** se ejecutarán 1 hora cada día, buscando solo los tweets nuevos, para así disminuir el procesamiento.
2. Una vez que el script 2 recibe los datos y le realiza el análisis de sentimientos, guardaremos el resultado en un **Bucket de Google Storage**.
3. El cluster **Hadoop** de **Google Cloud Dataproc**, se encargará de leer los datos del **Bucket**, y con todos los datos, se encargará de procesarlos y generar los resultados (CSV)
4. Mediante una **Google Cloud Function**, cada semana introducirá el CSV a **Google Cloud SQL**
5. Por último, nuestra web se conectará a la base de datos mediante API, y actualizará el **Dashboard**..

Para conseguir que cada cosa se ejecute en su momento, se realizará mediante Python en una **Google Cloud Function**, impulsados por un **Google Cloud Scheduler**.

## Desarrollo de la plataforma DAaaS.
El CSV resultado tendrá en principio las siguientes columnas:

<table>
    <tr>
        <td>KEYWORD</td>
        <td>SENTIMENT_TWITTER</td>
        <td>PUBLICHED_TIME</td>
        <td>INSERT_TIME</td>
    </tr>
</table>

# Diagrama
![Diagráma de la práctica - BD Architecture.png](https://github.com/tonyzetag/Big-Data-Architecture/blob/main/Diagr%C3%A1ma%20de%20la%20pr%C3%A1ctica%20-%20BD%20Architecture.png?raw=true)
