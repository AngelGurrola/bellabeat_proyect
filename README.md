Bellabeat: proyecto final
================

## Contenido

- Primero
- Segundo

## Contexto

En este proyecto simulamos trabajar para la empresa Bellabeat. Urška
Sršen y Sando Mur fundaron Bellabeat, una empresa de alta tecnología que
fabrica productos inteligentes focalizados en el cuidado de la salud.

¿Qué ha hecho?

- Invirtió en medios tradicionales y en marketing digital
- Invierte en google search y mantiene activas sus redes sociales
- Anuncios de video en YT y avisos publicitarios en Red de Display
  Google en fechas clave.

Orden de trabajo:

Concentrarse en un producto Bellabeat y analizar losdatos de uso de
dispositivos inteligentes para conocer cómo las personas están usando
sus dispositivos inteligentes. Emitir recomendaciones de alto nivel
sobre cómo estas tendencias pueden colaborar en la estrategia de
marketing de Bellabeat.

## Paso cero

A modo de replicar mi trabajo en el futuro desde una perspectiva donde
no recuerde absolutamente nada de lo que estoy haciendo ahora mismo, vi
necesario incluir este paso donde primero tuve que decidir en que
plataforma realizaria la publicación de mi trabajo y resolví que github
es la plataforma más actualizada que ademas de alojar mi proyecto
serviría para aprender a utilizar esta herramienta de control de
versiones y que tambien me ayudaría a crear un portafolio inicial.

Tuve que aprender desde como crear un archivo README en R a conectar Git
desde la misma interfaz de R studio y añadí mi proyecto a github
utilizando las siguientes lineas de comandos:

- git remote add origin
  <https://github.com/AngelGurrola/bellabeat_proyect.git>
- git branch -M main
- git push -u origin main

Utilice el video tutorial de Riffomonas Project para aprender como
conectar RStudio con github disponible en:
<https://www.youtube.com/watch?v=bUoN85QvC10&t=505s>

Una vez teniendo la plataforma lista comence con mi proyecto de analisis
de datos para Bellabeat.

## Preparar

Comenzamos con importar el conjunto de datos: descargamos la carpeta
comprimida desde: <https://www.kaggle.com/datasets/arashnic/fitbit>

Al descomprimir la carpeta de datos se observan 18 tablas con un peso de
322 MB en donde los titulos de cada una nos dan contexto del contenido y
el peso de las mismas nos ayudan a dimensionar la cantidad de datos que
poseen, siendo la tabla heartrate_seconds_merged la más pesada con 87.4
MB y weightLogInfo_merged la mas lígera con 7 KB.

Se añadieron todos los archivos al directorio de trabajo despues de
darles un vistazo rápido desde hoja de cálculo.

La tabla base desde obtendremos las primeras hipotesis será
dailyActivity_merged, la cual provee un resumen diario de los totales de
información recolectada por los dispositivos de Bellabeat.

Entonces se procede a importar las tablas como conjunto de datos de R.

``` r
library(readr)
```

    ## Warning: package 'readr' was built under R version 4.2.2

``` r
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
pesos <- read.csv(file="C://Users//an_96//Documents//Bellabeat//Bellabeat//archive//Fitabase Data 4.12.16-5.12.16//weightLogInfo_merged.csv")
pasos <- read.csv(file="C://Users//an_96//Documents//Bellabeat//Bellabeat//archive//Fitabase Data 4.12.16-5.12.16//hourlySteps_merged.csv")
diario <- read.csv(file="C://Users//an_96//Documents//Bellabeat//Bellabeat//archive//Fitabase Data 4.12.16-5.12.16//dailyActivity_merged.csv")
```

Durante la revisión rápida en hojas de cálculo se observaron notables
diferencias en la cantidad de datos, por lo tanto, se procedió a
confirmar la cantidad de Id que contenian algunas de las tablas.

``` r
n_distinct(pesos$Id)
```

    ## [1] 8

``` r
n_distinct(pasos$Id)
```

    ## [1] 33

``` r
n_distinct(diario$Id)
```

    ## [1] 33

Se observa como la tabla de pesos inlcuye unicamente los datos de 8
individuos por lo que deja de ser relevante para el analisis al no ser
una variable que incluyan en todas las tablas.

*Desde este punto considero importante mantener un registro más habitual
del peso de los usuarios mediante una notificación de recordatorio a
registrar tu peso.*

Para explorar las tablas se utilizó la libreria de tidyverse

``` r
glimpse(diario)
```

    ## Rows: 940
    ## Columns: 15
    ## $ Id                       <dbl> 1503960366, 1503960366, 1503960366, 150396036…
    ## $ ActivityDate             <chr> "4/12/2016", "4/13/2016", "4/14/2016", "4/15/…
    ## $ TotalSteps               <int> 13162, 10735, 10460, 9762, 12669, 9705, 13019…
    ## $ TotalDistance            <dbl> 8.50, 6.97, 6.74, 6.28, 8.16, 6.48, 8.59, 9.8…
    ## $ TrackerDistance          <dbl> 8.50, 6.97, 6.74, 6.28, 8.16, 6.48, 8.59, 9.8…
    ## $ LoggedActivitiesDistance <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    ## $ VeryActiveDistance       <dbl> 1.88, 1.57, 2.44, 2.14, 2.71, 3.19, 3.25, 3.5…
    ## $ ModeratelyActiveDistance <dbl> 0.55, 0.69, 0.40, 1.26, 0.41, 0.78, 0.64, 1.3…
    ## $ LightActiveDistance      <dbl> 6.06, 4.71, 3.91, 2.83, 5.04, 2.51, 4.71, 5.0…
    ## $ SedentaryActiveDistance  <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    ## $ VeryActiveMinutes        <int> 25, 21, 30, 29, 36, 38, 42, 50, 28, 19, 66, 4…
    ## $ FairlyActiveMinutes      <int> 13, 19, 11, 34, 10, 20, 16, 31, 12, 8, 27, 21…
    ## $ LightlyActiveMinutes     <int> 328, 217, 181, 209, 221, 164, 233, 264, 205, …
    ## $ SedentaryMinutes         <int> 728, 776, 1218, 726, 773, 539, 1149, 775, 818…
    ## $ Calories                 <int> 1985, 1797, 1776, 1745, 1863, 1728, 1921, 203…

De esta exploracion se observa que la fecha podría convertirse en un
formato ISO para facilitar el analisis así como el tipo de datos que se
tienen en las distintas columnas.

Para obtener una mayor descripción de los datos utilizo la siguiente
función para obtener un resumen de los datos en la tabla de “diario”.

``` r
summary(diario)
```

    ##        Id            ActivityDate         TotalSteps    TotalDistance   
    ##  Min.   :1.504e+09   Length:940         Min.   :    0   Min.   : 0.000  
    ##  1st Qu.:2.320e+09   Class :character   1st Qu.: 3790   1st Qu.: 2.620  
    ##  Median :4.445e+09   Mode  :character   Median : 7406   Median : 5.245  
    ##  Mean   :4.855e+09                      Mean   : 7638   Mean   : 5.490  
    ##  3rd Qu.:6.962e+09                      3rd Qu.:10727   3rd Qu.: 7.713  
    ##  Max.   :8.878e+09                      Max.   :36019   Max.   :28.030  
    ##  TrackerDistance  LoggedActivitiesDistance VeryActiveDistance
    ##  Min.   : 0.000   Min.   :0.0000           Min.   : 0.000    
    ##  1st Qu.: 2.620   1st Qu.:0.0000           1st Qu.: 0.000    
    ##  Median : 5.245   Median :0.0000           Median : 0.210    
    ##  Mean   : 5.475   Mean   :0.1082           Mean   : 1.503    
    ##  3rd Qu.: 7.710   3rd Qu.:0.0000           3rd Qu.: 2.053    
    ##  Max.   :28.030   Max.   :4.9421           Max.   :21.920    
    ##  ModeratelyActiveDistance LightActiveDistance SedentaryActiveDistance
    ##  Min.   :0.0000           Min.   : 0.000      Min.   :0.000000       
    ##  1st Qu.:0.0000           1st Qu.: 1.945      1st Qu.:0.000000       
    ##  Median :0.2400           Median : 3.365      Median :0.000000       
    ##  Mean   :0.5675           Mean   : 3.341      Mean   :0.001606       
    ##  3rd Qu.:0.8000           3rd Qu.: 4.782      3rd Qu.:0.000000       
    ##  Max.   :6.4800           Max.   :10.710      Max.   :0.110000       
    ##  VeryActiveMinutes FairlyActiveMinutes LightlyActiveMinutes SedentaryMinutes
    ##  Min.   :  0.00    Min.   :  0.00      Min.   :  0.0        Min.   :   0.0  
    ##  1st Qu.:  0.00    1st Qu.:  0.00      1st Qu.:127.0        1st Qu.: 729.8  
    ##  Median :  4.00    Median :  6.00      Median :199.0        Median :1057.5  
    ##  Mean   : 21.16    Mean   : 13.56      Mean   :192.8        Mean   : 991.2  
    ##  3rd Qu.: 32.00    3rd Qu.: 19.00      3rd Qu.:264.0        3rd Qu.:1229.5  
    ##  Max.   :210.00    Max.   :143.00      Max.   :518.0        Max.   :1440.0  
    ##     Calories   
    ##  Min.   :   0  
    ##  1st Qu.:1828  
    ##  Median :2134  
    ##  Mean   :2304  
    ##  3rd Qu.:2793  
    ##  Max.   :4900

## Including Plots

You can also embed plots, for example:

![](README_files/figure-gfm/pressure-1.png)<!-- -->

Note that the `echo = FALSE` parameter was added to the code chunk to
prevent printing of the R code that generated the plot.
