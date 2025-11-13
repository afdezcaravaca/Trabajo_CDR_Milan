
-----

# 📶 Análisis de Datos CDR - Milán

Este repositorio contiene el análisis de los datos del **"Telecom Italia Big Data Challenge"**, enfocado en la actividad de la red de telecomunicaciones (SMS, llamadas, internet) en la ciudad de Milán.

El proyecto implementa un pipeline completo:

1.  **Carga (ETL):** Lectura de los archivos (`.txt`) crudos, limpieza (manejo de `NaN`) y carga en **MongoDB**.
2.  **Agregación:** Consultas complejas (`$group`, `$facet`, `$match`) en MongoDB para agregar los 32 millones de registros por celda y por hora.
3.  **Visualización:** Análisis geoespacial con `Geopandas` para crear mapas de calor de la actividad de la ciudad.

-----

## 📋 Índice

1.  [Sobre el Proyecto](https://www.google.com/search?q=%23-sobre-el-proyecto)
2.  [Datasets Requeridos](https://www.google.com/search?q=%23-datasets-requeridos)
3.  [Instalación y Entorno](https://www.google.com/search?q=%23-instalaci%C3%B3n-y-entorno)
4.  [Descripción de los Códigos](https://www.google.com/search?q=%23-descripci%C3%B3n-de-los-c%C3%B3digos)
5.  [Créditos y Agradecimientos](https://www.google.com/search?q=%23-cr%C3%A9ditos-y-agradecimientos)

-----

## 💡 Sobre el Proyecto

El objetivo principal de este análisis es procesar un conjunto de datos masivo (32 millones de registros) de Call Detail Records (CDR) para entender los patrones de comportamiento de la ciudad de Milán.

El análisis se centra en:

  * Identificar la actividad de la red (SMS, llamadas, internet) por celda (`cellId`).
  * Analizar la actividad internacional, identificando y limpiando los `countrycode`.
  * Encontrar las celdas con mayor y menor actividad en diferentes momentos.
  * Crear colecciones "acumuladas" en MongoDB que resuman los datos por `cellid` y por `cellid`-`hora`.
  * Generar visualizaciones geoespaciales de los patrones de actividad usando `geopandas`.

-----

## 💾 Datasets Requeridos

Para ejecutar los análisis, es necesario descargar los datasets oficiales del desafío. Debido a su tamaño y licencia, no están incluidos en este repositorio.

| Fichero | Descripción | Enlace de Descarga |
| :--- | :--- | :--- |
| `milano-grid.geojson` | Contiene la cuadrícula geoespacial (shapefile) de Milán, con el `cellId` de cada celda. | **[ENLACE AL GEOJSON AQUÍ]** |
| `sms-call-internet-mi-*.txt` | Datos CDR (SMS, llamadas, internet). Hay un fichero por cada día del periodo de estudio. | **[ENLACE A LA PÁGINA DEL DATASET AQUÍ]** |

⚠️ **Importante:** Una vez descargados, los ficheros `.txt` deben colocarse en la carpeta `./Datos/` (o actualizar la ruta en el notebook) para que el script de carga (`01_Carga_Datos.ipynb`) funcione.

-----

## ⚙️ Instalación y Entorno

Para replicar el análisis, se recomienda crear un entorno de Conda específico para asegurar la compatibilidad de las librerías, especialmente `geopandas`.

1.  Clona este repositorio:

    ```bash
    git clone https://github.com/[TU_USUARIO]/[TU_REPOSITORIO].git
    cd [TU_REPOSITORIO]
    ```

2.  Crea el entorno de Conda con las dependencias geoespaciales (como se usó en el proyecto):

    ```bash
    conda create -n geo_env -c conda-forge geopandas ipykernel python=3.10
    ```

3.  Activa el entorno e instala el resto de dependencias:

    ```bash
    conda activate geo_env
    pip install pymongo pandas matplotlib seaborn notebook
    ```

4.  Asegúrate de tener una instancia de **MongoDB** corriendo en `localhost:27017` (o actualiza la URI de conexión en los scripts).

-----

## 💻 Descripción de los Códigos

El análisis está dividido en varios notebooks de Jupyter:

### `01_Carga_Datos_MongoDB.ipynb`

  * **Propósito:** Script de ETL (Extracción, Transformación y Carga).
  * **Proceso:**
    1.  Lee los archivos `.txt` crudos de la carpeta `./Datos/` por lotes (chunks) usando `pandas`.
    2.  Limpia los datos en cada lote (reemplaza `NaN` por `0`).
    3.  Inserta los registros limpios en la colección `Milan_CDR_c` de MongoDB.

### `02_Analisis_y_Agregacion.ipynb`

  * **Propósito:** El análisis principal de los datos.
  * **Proceso:**
    1.  **Limpieza de `countrycode`:** Identifica los 299 códigos, los limpia y los mapea a un diccionario de países/zonas.
    2.  **Análisis de Actividad:** Utiliza pipelines de agregación (`$group`, `$facet`, `$sort`) para encontrar las celdas con la máxima actividad (SMS, llamadas, internet).
    3.  **Manejo de `NaN`:** Implementa la lógica de limpieza (`$cond`, `$eq`) para los valores `NaN` encontrados en las agregaciones.
    4.  **Creación de Colecciones:** Contiene los pipelines (`$out`) que generan las colecciones de "acumulados":
          * `celdas_Milan`: Un documento por celda con el total de actividad.
          * `celdas_por_horas_Milan`: Un documento por celda y hora con el total de actividad.

### `03_Visualizacion_Geoespacial.ipynb`

  * **Propósito:** Visualización de los resultados en un mapa.
  * **Proceso:**
    1.  Carga la colección `celdas_Milan` (creada en el notebook 02).
    2.  Carga el `milano-grid.geojson`.
    3.  Une (merge) los dos DataFrames usando `cellId`.
    4.  Genera mapas de calor (`explore()`) que muestran la intensidad de la actividad (Internet, SMS, etc.) en la ciudad.

-----

## 🙏 Créditos y Agradecimientos

### Artículo Original

Este proyecto se basa en el dataset público del "Telecom Italia Big Data Challenge". Se da crédito completo a los autores y organizadores.

  * **Artículo (Cita):** [AÑADE AQUÍ LA CITA DEL ARTÍCULO ORIGINAL, ej: Brea, J., et al. (2014)...]

### Agradecimientos

Quiero agradecer a **[NOMBRE DEL USUARIO/REPOSITORIO]** por su excelente repositorio **[NOMBRE DEL REPOSITORIO]** ([ENLACE A SU REPOSITORIO AQUÍ]). Su código me sirvió como una guía fundamental y una gran inspiración para las consultas de agregación y el enfoque del análisis.
