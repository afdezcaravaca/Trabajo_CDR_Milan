
-----

# 📶 Análisis de Datos CDR - Milán
Trabajo CDR's Milán del Máster Inteligencia Computacional y el Internet de las Cosas de la Universidad de Córdoba.

Este repositorio contiene el análisis de los datos del **"Telecom Italia Big Data Challenge"** utilizando **MongoDB**, enfocado en la actividad de la red de telecomunicaciones (SMS, llamadas, internet) en la ciudad de Milán.

El proyecto implementa un pipeline completo:

1.  **Carga de datos:** Lectura de los archivos (`.txt`) crudos, limpieza (manejo de `NaN`) y carga en **MongoDB**.
2.  **Agregación:** Consultas complejas (`$group`, `$facet`, `$match`) en MongoDB para agregar los 32 millones de registros por celda y por hora.
3.  **Visualización:** Análisis geoespacial con `Geopandas` para crear mapas y poder visualizar las celdas en las que se dividie la ciudad.

-----

## 📋 Índice

1.  [Sobre el Proyecto](https://github.com/afdezcaravaca/Trabajo_CDR_Milan/blob/main/README.md#-sobre-el-proyecto)
2.  [Datasets Requeridos](https://github.com/afdezcaravaca/Trabajo_CDR_Milan/blob/main/README.md#-datasets-requeridos)
3.  [Descripción de los Códigos](https://github.com/afdezcaravaca/Trabajo_CDR_Milan/blob/main/README.md#descripción-de-los-códigos)
4.  [Créditos y Agradecimientos](https://github.com/afdezcaravaca/Trabajo_CDR_Milan/blob/main/README.md#créditos-y-agradecimientos)

-----

## 💡 Sobre el Proyecto

El objetivo principal de este análisis es procesar un conjunto de datos masivo (32 millones de registros) de Call Detail Records (CDR) para entender los patrones de comportamiento de la ciudad de Milán.

El análisis se centra en:

  * Identificar la actividad de la red (SMS, llamadas, internet) por celda (`cellid`).
  * Analizar la actividad internacional, identificando y limpiando los `countrycode`.
  * Encontrar las celdas con mayor y menor actividad en diferentes momentos.
  * Crear colecciones "acumuladas" en MongoDB que resuman los datos por `cellid` y por `cellid`-`hora`.
  * Generar visualizaciones geoespaciales de los patrones de actividad usando `geopandas`.

-----

## 💾 Datasets Requeridos

Para ejecutar los análisis, es necesario descargar los datasets oficiales del desafío. Debido a su tamaño y licencia, no están incluidos en este repositorio.

| Fichero | Descripción | Enlace de Descarga |
| :--- | :--- | :--- |
| `milano-grid.geojson` | Contiene la cuadrícula geoespacial de Milán, con el `cellid` de cada celda. | **[Enlace](https://www.kaggle.com/datasets/muzamalrazasoomro/milano-grid)** |
| `sms-call-internet-mi-*.txt` | Datos CDR (SMS, llamadas, internet). | **[Enlace](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/EGZHFV)** |

En concreto, se analizan los datos de la semana 22/12/2013 - 28/12/2013:
1. sms-call-internet-mi-2013-12-22.txt (Lunes)
2. sms-call-internet-mi-2013-12-23.txt
3. sms-call-internet-mi-2013-12-24.txt
4. sms-call-internet-mi-2013-12-25.txt
5. sms-call-internet-mi-2013-12-26.txt
6. sms-call-internet-mi-2013-12-27.txt
7. sms-call-internet-mi-2013-12-28.txt 

⚠️ **Importante:** Una vez descargados, los ficheros `.txt` deben colocarse en la carpeta `./Datos/` (o actualizar la ruta en el notebook) para que el script de carga (`CDR_Milan.ipynb`) funcione.


## 💻 Descripción de los Códigos

El análisis está dividido en varios notebooks de Jupyter:

### `CR_Milan.ipynb`

  * **Propósito:** Archivo main del trabajo, en el que se explica detalladamente el proceso de realización de la práctica.

### `visualizar_grid.ipynb`

  * **Propósito:** Permite visualizar las celdas en el grid de Milán.

-----

## 🙏 Créditos y Agradecimientos

### Artículo Original

Este proyecto se basa en el dataset público del "Telecom Italia Big Data Challenge". Se da crédito completo a los autores y organizadores.

  * **ArtículoBarlacchi, G., De Nadai, M., Larcher, R., Casella, A., Chitic, C., Torrisi, G., ... & Lepri, B. (2015). A multi-source dataset of urban life in the city of Milan and the Province of Trentino. Scientific Data, 2, Artículo 150055:** https://doi.org/10.1038/sdata.2015.55

### Agradecimientos

* **Repositorio Subbiah, A. (2021). milan-telecom-data-modeling:** https://github.com/arunasubbiah/milan-telecom-data-modeling. 
