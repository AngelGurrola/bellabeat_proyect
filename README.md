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
library(ggplot2)
library(tidyverse)
```

    ## Warning: package 'tidyverse' was built under R version 4.2.2

    ## ── Attaching packages
    ## ───────────────────────────────────────
    ## tidyverse 1.3.2 ──

    ## ✔ tibble  3.1.8     ✔ stringr 1.4.1
    ## ✔ tidyr   1.2.0     ✔ forcats 0.5.2
    ## ✔ purrr   0.3.4

    ## Warning: package 'forcats' was built under R version 4.2.2

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
pesos <- read.csv(file="C://Users//an_96//Documents//Bellabeat//Bellabeat//archive//Fitabase Data 4.12.16-5.12.16//weightLogInfo_merged.csv")
pasos <- read.csv(file="C://Users//an_96//Documents//Bellabeat//Bellabeat//archive//Fitabase Data 4.12.16-5.12.16//hourlySteps_merged.csv")
diario <- read.csv(file="C://Users//an_96//Documents//Bellabeat//Bellabeat//archive//Fitabase Data 4.12.16-5.12.16//dailyActivity_merged.csv")
sueño <- read.csv(file="C://Users//an_96//Documents//Bellabeat//Bellabeat//archive//Fitabase Data 4.12.16-5.12.16//sleepDay_merged.csv")
calorias <- read.csv(file="C://Users//an_96//Documents//Bellabeat//Bellabeat//archive//Fitabase Data 4.12.16-5.12.16//dailyCalories_merged.csv")
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

``` r
n_distinct(sueño$Id)
```

    ## [1] 24

``` r
n_distinct(calorias$Id)
```

    ## [1] 33

Se observa como la tabla de pesos incluye unicamente los datos de 8
individuos por lo que deja de ser relevante para el analisis al no ser
una variable que incluyan en todas las tablas.

Así como que la tabla de sueño contiene únicamente los registros de 24
usuarios.

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

Desde este punto se pueden determinar las respuestas a las preguntas
clave de esta parte del reto:

Los datos tienen un formato largo y se organizan en 18 tablas
almacenadas en archivos csv. Se detectan grandes problemas de sesgo al
tener únicamente 33 usuarios de los cuales no se añaden contexto
demográfico como su ubicación, edad, sexo (se infiere que son mujeres,
pero no se confirma) etc.

Si bien, la pregunta de los interesados está enfocada a la
implementación de una nueva campaña de marketing para la utilización de
los productos de bellabeat sobre los de la competencia, yo consideraría
la posibilidad de ampliar la cantidad de datos para obtener mejores
resultados y un nivel de confianza aceptable.

Para efectos del reto enfocaré los datos a esta pequeña población,
tratándola como un punto de partida para la formulación de hipótesis que
se podrán confirmar con un estudio posterior donde se observen
cantidades de datos significativas.

Se acotará el analisis a las variables de pasos y calorias al ambas
contener datos de los 33 usuarios.

## Procesar

Como se explicó anteriormente, se utilizará el lenguaje de programación
R desde RStudio Desktop para este proyecto, así como el sitio de Github
para su publicación y almacenamiento.

Se procede a buscar duplicados dentro del conjunto de datos.

``` r
sum(duplicated(diario))
```

    ## [1] 0

``` r
sum(duplicated(pasos))
```

    ## [1] 0

``` r
sum(duplicated(calorias))
```

    ## [1] 0

``` r
sum(is.na(diario))
```

    ## [1] 0

``` r
sum(is.na(pasos))
```

    ## [1] 0

``` r
sum(is.na(calorias))
```

    ## [1] 0

Al no encontrarse duplicados en las variables de analisis ni valores NA
se procedió a buscar la cantidad de registros para cada usuario, es
decir, la cantidad de dias que tenía cada usuario en su record diario.

``` r
dias <- diario %>% group_by(Id) %>% 
     summarise(conteo_dias=n(),
               .groups = 'drop')
hist(dias$conteo_dias)
```

![](README_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

Mediante un histograma básico, se observó una cantidad distinta de
registros para los usuarios.

Se considera reducir el análisis a solo los individuos con más de 25
registros para evitar sesgos por registros incompletos que lleguen a
alterar las cifras completas, sin embargo, se plantea clasificar a los
usuarios como usuarios que no utilizan los productos habitualmente y se
busca aplicar un enfoque distinto creando una columna para el día de la
semana en que se realizó el registro.

Para añadir esta variable se extrae de la columna de fecha el dia de la
semana que se realizó el registro.

``` r
diario <- diario %>% mutate( Dia_semana = weekdays(as.Date(ActivityDate, "%m/%d/%Y")))
head(diario)
```

    ##           Id ActivityDate TotalSteps TotalDistance TrackerDistance
    ## 1 1503960366    4/12/2016      13162          8.50            8.50
    ## 2 1503960366    4/13/2016      10735          6.97            6.97
    ## 3 1503960366    4/14/2016      10460          6.74            6.74
    ## 4 1503960366    4/15/2016       9762          6.28            6.28
    ## 5 1503960366    4/16/2016      12669          8.16            8.16
    ## 6 1503960366    4/17/2016       9705          6.48            6.48
    ##   LoggedActivitiesDistance VeryActiveDistance ModeratelyActiveDistance
    ## 1                        0               1.88                     0.55
    ## 2                        0               1.57                     0.69
    ## 3                        0               2.44                     0.40
    ## 4                        0               2.14                     1.26
    ## 5                        0               2.71                     0.41
    ## 6                        0               3.19                     0.78
    ##   LightActiveDistance SedentaryActiveDistance VeryActiveMinutes
    ## 1                6.06                       0                25
    ## 2                4.71                       0                21
    ## 3                3.91                       0                30
    ## 4                2.83                       0                29
    ## 5                5.04                       0                36
    ## 6                2.51                       0                38
    ##   FairlyActiveMinutes LightlyActiveMinutes SedentaryMinutes Calories Dia_semana
    ## 1                  13                  328              728     1985     martes
    ## 2                  19                  217              776     1797  miércoles
    ## 3                  11                  181             1218     1776     jueves
    ## 4                  34                  209              726     1745    viernes
    ## 5                  10                  221              773     1863     sábado
    ## 6                  20                  164              539     1728    domingo

La tabla diario contiene todas las variables de estudio y los datos de
los usuarios reunidos en una única tabla, por lo que será seleccionada
como principal.

Tras haber realizado esta manipulación de los datos, y tomar en cuenta
las consideraciones previamente declaradas. Se procede al siguiente paso
del proyecto.

## Analizar

Primeramente recordaremos los encabezados de nuestra tabla

``` r
colnames(diario)
```

    ##  [1] "Id"                       "ActivityDate"            
    ##  [3] "TotalSteps"               "TotalDistance"           
    ##  [5] "TrackerDistance"          "LoggedActivitiesDistance"
    ##  [7] "VeryActiveDistance"       "ModeratelyActiveDistance"
    ##  [9] "LightActiveDistance"      "SedentaryActiveDistance" 
    ## [11] "VeryActiveMinutes"        "FairlyActiveMinutes"     
    ## [13] "LightlyActiveMinutes"     "SedentaryMinutes"        
    ## [15] "Calories"                 "Dia_semana"

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
    ##     Calories     Dia_semana       
    ##  Min.   :   0   Length:940        
    ##  1st Qu.:1828   Class :character  
    ##  Median :2134   Mode  :character  
    ##  Mean   :2304                     
    ##  3rd Qu.:2793                     
    ##  Max.   :4900

### Por dia de la semana

Para comparar los datos desde la perspectiva de día de la semana se
exportó el archivo csv que por cuestiones de tiempo facilitó el análisis
y la creación de gráficos.

write.csv(diario, file = “diario.csv”)

``` r
##diario$Dia_semana = factor(diario$Dia_semana, levels = c("lunes", "martes", "miércoles", "jueves", "sábado", "domingo"))
```

``` r
df %>%
ggplot(data = diario, mapping = aes(x = Dia_semana, y = TotalSteps, fill = Dia_semana))+ geom_bar(stat = "identity", position = "dodge") + labs(title = "Pasos por dia de la semana") 
```

![](README_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

``` r
library(forcats)
df %>%
ggplot(data = diario, mapping = aes(x = fct_rev(fct_reorder(Dia_semana, Calories)), y = TotalSteps, fill = Dia_semana))+ geom_bar(stat = "identity") + labs(title = "Pasos por dia de la semana") 
```

![](README_files/figure-gfm/unnamed-chunk-12-1.png)<!-- --> Para
corroborar el uso de los dispositivos de Bellabeat por día de la semana
se graficaron los registros ![Calorias por dia de la
semana.](~/Bellabeat/Bellabeat/archive/1.png) ![Calorias por dia de la
semana.](~/Bellabeat/Bellabeat/archive/2.png)

### Hipotesis de pesos

Aún cuando se descartó la importancia de la tabla de pesos resumiremos
los datos para conocer las características de los usuarios que sí
registraton datos.

``` r
usuariospesos <- pesos %>% 
  select(Id, BMI, IsManualReport) %>% 
  group_by(Id, IsManualReport) %>%  
  summarise(IMCpromedio = mean(BMI), n = n())
```

    ## `summarise()` has grouped output by 'Id'. You can override using the `.groups`
    ## argument.

``` r
arrange(usuariospesos, IMCpromedio)
```

    ## # A tibble: 8 × 4
    ## # Groups:   Id [8]
    ##           Id IsManualReport IMCpromedio     n
    ##        <dbl> <chr>                <dbl> <int>
    ## 1 2873212765 True                  21.6     2
    ## 2 1503960366 True                  22.6     2
    ## 3 6962181067 True                  24.0    30
    ## 4 8877689391 False                 25.5    24
    ## 5 4558609924 True                  27.2     5
    ## 6 4319703577 True                  27.4     2
    ## 7 5577150313 False                 28       1
    ## 8 1927972279 False                 47.5     1

De esta tabla esperaba obtener la cantidad de reportes efectuados
manualmente considerando el IMC de la persona. Buscando una relación
entre las variables, sin embargo, por la cantidad de datos se debe
resolver primero que se obtengan los datos de más participantes.
Considero que uno de los mejores indicadores de la salud de una persona
es el Indice de Masa Corporal(BMI en inglés) y por tanto se me ocurre
ofrecer paquetes de productos donde se utilicen productos que monitoreen
el IMC para que la aplicación de Bellabeat ofrezca notificaciones cuando
este se vea fuera de sus parámetros normales, pasando a un monitoreo
activo de tu IMC.
