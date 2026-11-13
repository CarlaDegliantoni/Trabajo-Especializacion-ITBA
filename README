Dengue en CABA y clima: viabilidad de los datos públicos para modelos predictivos

Código del Trabajo Final Integrador (TFI) de la Especialización en Ciencia de Datos, ITBA. Autora: Carla Degliantoni.

De qué trata

El trabajo se pregunta si los datos públicos disponibles de la Ciudad Autónoma de Buenos Aires (CABA) están en condiciones de sostener un modelo predictivo de brotes de dengue. Para responderlo, evalúa la calidad y la madurez de esos datos entre 2018 y 2025, e integra los registros epidemiológicos semanales por comuna con series climáticas.

El análisis cubre 393 semanas epidemiológicas (del 29/1/2018 al 4/8/2025) y tiene seis partes:

Integración y curación de los datos (casos y clima).
Análisis exploratorio: estacionalidad, evolución interanual, grupos etarios y diferencias entre comunas (con barrios populares del RENABAP).
Estructura de rezagos entre clima y casos: correlación por rezago y correlación cruzada (CCF).
Modelos univariados de pronóstico (SNAIVE, ARIMA y ETS) como línea de base.
Modelo de rezagos distribuidos no lineales (DLNM) sobre el Humidex.
Reducción de dimensionalidad con componentes principales (PCA) y DLNM con el primer componente (PC1).
Resultados principales
Datos: la base climática y la epidemiológica numeraban las semanas con criterios distintos (la semana climática empezaba en domingo y la ISO en lunes). El clima se agrega por semanas de lunes a domingo y se cruza por año y semana ISO, de modo que las 393 semanas del período tienen clima asociado.
Serie de casos: el brote de 2024 llegó a 93,21 casos cada 100.000 habitantes en la semana epidemiológica 12, casi cuatro veces el pico de 2023 (25,07). Ningún modelo univariado lo anticipó (mejor RMSE, SNAIVE: 20,542).
Clima y rezagos: la correlación con la temperatura crece hasta el rezago 7 y se estabiliza en el 8 (0,295). En el DLNM, el riesgo relativo de un Humidex alto (percentil 90) aumenta de forma sostenida con el rezago: 1,46 en la semana 3 (IC 95%: 1,15 a 1,85) y 3,68 en la semana 6 (IC 95%: 2,01 a 6,76). El riesgo relativo máximo es 5,3.
PCA: con las variables crudas, los dos primeros componentes explican el 90,8% de la varianza. Con las 24 variables rezagadas hacen falta 10 componentes para pasar el 85% (88,3%). El DLNM con PC1 reproduce el crecimiento del riesgo con el rezago, pero con menor magnitud (RR máximo de 3,9) y un pico en el borde de la distribución, por lo que no reemplaza al Humidex.
Comunas: el clima es una única serie para toda la Ciudad, por lo que las diferencias de carga entre comunas no pueden explicarse por el clima y hay que buscarlas en factores socio-urbanos.
Límites
El DLNM se estimó con rezagos de hasta 6 semanas (3 grados de libertad en la exposición y 2 en el rezago), porque con configuraciones más complejas el modelo resultaba inestable. No cubre las semanas 7 y 8, donde la correlación con la temperatura es más alta.
El clima proviene de una única estimación para toda la Ciudad (NASA POWER), validada con la estación Aeroparque.
Los modelos univariados de pronóstico son una línea de base: ninguno anticipó el brote de 2024, y no se entrenó un modelo supervisado con clima.
Estructura del repositorio
dengue_exploracion.Rmd      Análisis completo (código y texto que genera el informe)
README.md                   Este archivo
datos/                      Datos de entrada (ver más abajo)
Datos
Fuente	Contenido	Cómo se obtiene
Ministerio de Salud de la Nación, Vigilancia de Enfermedades por virus Dengue	Casos de dengue por semana epidemiológica y comuna (registros acotados a CABA), 2018-2025	Datos abiertos del Ministerio de Salud. El Rmd lee 11 archivos (df1 a df11) con distintos formatos
NASA POWER	Temperatura, humedad y precipitación diarias en el punto de CABA	Descarga automática con el paquete nasapower
Aeroparque (estación SABE)	Temperatura y humedad, para validar NASA POWER	Descarga automática con el paquete riem
RENABAP (información pública del 5/12/2023)	Barrios populares por comuna	CSV de datos abiertos, más un geojson que el Rmd descarga la primera vez
BA Data	Cartografía de las comunas	Descarga automática la primera vez

Los archivos de dengue y el CSV de RENABAP deben estar en datos/datos crudos - dengue/NACION/. Los mapas se guardan en datos/mapas/.

Cómo reproducirlo
Instalar los paquetes de R:
r
install.packages(c("tidyverse", "readxl", "sf", "writexl", "stringi", "lubridate",
                   "knitr", "RColorBrewer", "ggthemes", "ggrepel", "ISOweek",
                   "data.table", "httr", "jsonlite", "nasapower", "riem", "forecast",
                   "tsibble", "fable", "fabletools", "feasts", "urca", "dlnm"))
Colocar los datos de entrada en las carpetas indicadas arriba.
Ajustar las rutas. El Rmd tiene rutas de la computadora de la autora (G:/Mi unidad/ITBA/TFI/...). Hay que reemplazarlas por la ruta local, en setwd(), en la lectura de los archivos de dengue y de RENABAP, en ruta_mapas y en las exportaciones a .xlsx y .rds. El encabezado del Rmd también apunta a un reference_docx con el formato del informe.
Tejer el documento (Knit en RStudio, formato Word). Se necesita conexión a internet la primera vez, por las descargas de NASA POWER, Aeroparque y los mapas.
Orden del código
Sección del Rmd	Qué hace
Carga y preprocesamiento	Lee los 11 archivos de dengue, los unifica, corrige la semana en evento_nombre, normaliza textos y grupos etarios
Datos climáticos	Descarga NASA POWER y Aeroparque, y agrega el clima por semanas de lunes a domingo
Cruce dengue × clima	Une casos y clima por año y semana ISO
Análisis exploratorio	Estacionalidad, evolución, comunas de mayor carga, correlación por comuna y mapa con RENABAP
Análisis de rezagos	Correlación por rezago (1 a 8 semanas) y CCF con feasts
Descriptivo, grupos etarios e interanual	Resumen del dataset, carga por franja etaria y comparación de brotes por año
Estacionariedad y forecast	Tests ADF y KPSS, descomposición STL y modelos SNAIVE, ARIMA y ETS con validación temporal (80/20)
Modelo DLNM	Índice de calor compuesto (Humidex), crossbasis, ajuste quasipoisson, superficie de riesgo y cortes
PCA	PCA sobre variables crudas y rezagadas, y DLNM con PC1
Versión de R y paquetes

Versión de R utilizada: [completar]. Para fijar las versiones exactas de los paquetes, correr sessionInfo() al final del Rmd y guardar la salida.
