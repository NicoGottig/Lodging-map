# Mapa Interactivo de Precios de Hospedaje en Argentina 2023

## Resumen
Esta aplicación resume, según distintos hospedajes de Booking para el año 2023 (y para dos personas adultas, sin hijos), los **precios**, la **cantidad** y la **puntuación** de hoteles, cabañas y otros tipos de albergues en Argentina.

La información se recolectó a través de técnicas de Web Scraping, se almacenó en una base de datos relacional, se transformó y se validó para evitar valores nulos. No se realizó un tratamiento de datos atípicos debido a que la selección del rango de precios depende exclusivamente del usuario. 

Para la visualización se realizó una aplicación interactiva *Shiny*, con un mapa dinámico generado a través del paquete *leaflet*.
<br>

## Objetivo y metodología
El objetivo de la aplicación es resumir y exponer información en función de las solicitudes del usuario.
Para esto, se extrajo información sobre la localización, el precio, la descripción y el puntaje de más de 150.000 hospedajes en Argentina.<br>
Es **importante** tener en cuenta que el precio del hospedaje depende, entre otros factores, de la fecha de extracción de los datos. Por ejemplo, no es lo mismo buscar precios de hospedajes para el mes de enero, durante el mes de diciembre que durante el mes de octubre.  <br>
*En este caso, la extracción de los datos se realizó durante el mes de diciembre y enero.*<br>

Por otro lado, para el **cálculo de los estadísticos** de precios, se realizó un muestreo no probabilístico; se seleccionaron $n$ ciudades por provincia en función de consultas a empleados de empresas de servicios turísticos, y recomendaciones online.

El sitio *Booking* puede recomendar hospedajes de otras localidades, por lo tanto, para una cantidad $n$ de ciudades solicitadas se generará una cantidad $k$ de ciudades disponibles, conteniendo cada una $h$ hospedajes, y así para las 23 provincias (la Ciudad Autónoma de Buenos Aires está incluida en la provincia de Buenos Aires) <br>

La base de datos construida a través de las técnicas de web scraping estará compuesta por las siguientes variables:  <br>
+ **Localización**: Indica la ciudad del hospedaje.<br>
+ **Titulo**: Indica el título del hospedaje, como está expuesto en Booking.<br>
+ **Descripción**: Breve resumen del hospedaje.<br>
+ **Clasif**: Calificación del hospedaje (del 1 al 10).<br>
+ **Precio**: Precio del hospedaje por el total de días solicitados.<br>
+ **Impuesto**: Impuesto total por la cantidad de días solicitados.<br>   
+ **Noches**: Cantidad de noches solicitadas. <br>
+ **Provincia**: Indica la provincia del hospedaje.<br>
+ **Checkin**: Fecha solicitada para el ingreso al hospedaje.<br>
+ **Checkout**: Fecha solicitada para el egreso del hospedaje.<br>
<br>

Además, se construyen las siguientes variables:  <br>
+ **Precio_noche**: Precio neto por noche del hospedaje (Precio_Noche=Precio/Noches)  <br>
+ **Impuesto_noche**: Impuesto por noche del hospedaje (Impuesto_noche=Impuesto/Noches)<br>  
+ **Bruto_noche**: Precio total (incluyendo impuestos) (Bruto_Noche=Precio_noche+Impuesto_Noche)<br>

Con respecto a las fuentes de información, se utilizará Booking por su facilidad de acceso a la información.<br>

Por lo tanto, los precios son considerados *"precios de mercado"*; es decir, incluyen los impuestos o subsidios, además del valor agregado de las empresas que ofrecen el servicio.<br>

Con estas consideraciones, los estadísticos expuestos no pueden ser considerados un "indicador" de precios provinciales de hospedaje, limitando el análisis a la exploración de los datos contextualizada en un momento determinado y en una región dada.<br>

## Etapas:

#### 1 - Extracción de los datos:
Para la extracción de los datos se programó una función que recoge una consulta (ej. Hospedajes de la ciudad de Paraná) e itinera las páginas disponibles en Booking recogiendo datos sobre cada hospedaje.
En caso de no encontrar datos, se reemplaza la celda por un valor nulo. Esto sucede principalmente en la calificación de los hospedajes.
Para evitar sesgar el promedio por quincena, se dividieron las consultas en dos grupos de 15 días, y se calculó el promedio de ambas.<br>

#### 2 - Manipulación y Validación:
Para constituir la base de datos final, se calcula el precio por noche sumando los impuestos y dividiendo por la cantidad de noches solicitadas. 
Por otro lado, se eliminan los valores nulos existentes en el precio de hospedaje debido a las particularidades de éstos y se rellenan los valores de la calificación en función de su distribución. [Más información en el script](https://github.com/NicoGottig/Lodging-map/blob/main/Scripts/02_tfi_manipulacion-validacion.R)<br>

#### 3 - Presentación de la información:
Para la presentación de la información se desarrolló una aplicación interactiva *Shiny*. El usuario puede ingresar opciones de estilo y filtros de cálculo para visualizar la información resumida en un mapa. Además, puede consultar tablas resumidas y completas, permitiendo acceder la descripción de cada hospedaje. Para acceder a la aplicación haga [click aquí. ](https://mj8qpg-nicolas-gottig.shinyapps.io/Mapa_Interactivo_Hospedajes_Argentina/?_ga=2.91187288.370091405.1672937106-2073232725.1672937106)<br>

# Interactive Map of Accommodation Prices in Argentina 2023

## Summary
This application summarizes, according to different Booking accommodations for 2023 (and for two adults, without children), the **prices**, the **quantity** and the **score** of hotels, cabins and others types of hostels in Argentina.

The information was collected through Web Scraping techniques, stored in a relational database, transformed and validated to avoid null values. Outlier data treatment was not performed because the selection of the price range depends exclusively on the user.

For visualization, an interactive *Shiny* application was made, with a dynamic map generated through the *leaflet* package.

## Purpose and methodology
The purpose of the application is to summarize and expose information based on user requests.
For this, information was extracted on the location, price, description and score of more than 150,000 lodgings in Argentina.<br>

It is **important** to know that the price of the accommodation depends, among other factors, on the data extraction date. For example, it is not the same to look for lodging prices for the month of January, during the month of December than during the month of October. <br>
*In this case, the data extraction was carried out during the months of December and January.*<br>

On the other hand, for the **calculation of the price statistics**, a non-probabilistic sampling was carried out; $n$ cities per province were selected based on consultations with employees of tourism service companies, and online recommendations.

The *Booking* site can recommend lodgings from other locations, therefore, for a quantity $n$ of cities requested, a quantity $k$ of available cities will be generated, each containing $h$ lodgings, and so on for the 23 provinces (the Autonomous City of Buenos Aires is included in the province of Buenos Aires) <br>

The database built through web scraping techniques will be composed of the following variables: <br>
+ **Location**: Indicates the city of the lodging.<br>
+ **Title**: Indicates the title of the lodging, as it is exposed in Booking.<br>
+ **Description**: Brief summary of the lodging.<br>
+ **Classif**: Rating of the accommodation (from 1 to 10).<br>
+ **Price**: Price of the accommodation for the total number of days requested.<br>
+ **Tax**: Total tax for the number of days requested.<br>
+ **Nights**: Number of nights requested. <br>
+ **Province**: Indicates the province of the accommodation.<br>
+ **Checkin**: Date requested for admission to the lodging.<br>
+ **Checkout**: Date requested for the departure of the lodging.<br>
<br>

In addition, the following variables are constructed: <br>
+ **Price_night**: Net price per night of the lodging (Price_Night=Price/Nights) <br>
+ **Tax_night**: Tax per night of the lodging (Tax_night=Tax/Nights)<br>
+ **Gross_night**: Total price (including taxes) (Gross_Night=Price_night+Tax_Night)<br>

Regarding the sources of information, Booking will be used for its ease of access to information.<br>

Therefore, the prices are considered *"market prices"*; that is, they include taxes or subsidies, in addition to the added value of the companies that offer the service.<br>

With these considerations, the exposed statistics cannot be considered an "indicator" of provincial lodging prices, limiting the analysis to the exploration of the data contextualized at a given moment and in a given region.<br>

## Process:

#### 1 - Data extraction:
To extract the data, a function was programmed that collects a query (eg Lodgings in the city of Paraná) and roams the pages available in Booking collecting data on each lodging.
If no data is found, the cell is replaced with a null value. This happens mainly in the rating of the lodgings.
To avoid biasing the average per fortnight, the consultations were divided into two groups of 15 days, and the average of both was calculated.<br>

#### 2 - Manipulation and Validation:
To build the final database, the price per night is calculated by adding the taxes and dividing by the number of nights requested.
On the other hand, the null values ​​existing in the lodging price are eliminated due to their particularities and the qualification values ​​are filled in based on their distribution. [More information in the script](https://github.com/NicoGottig/Lodging-map/blob/main/Scripts/02_tfi_manipulacion-validacion.R)<br>

#### 3 - Presentation of information:
For the presentation of the information an interactive application *Shiny* was developed. The user can enter style options and calculation filters to display summarized information on a map. In addition, you can consult summarized and complete tables, allowing you to access the description of each lodging. To access the application, click [here](https://mj8qpg-nicolas-gottig.shinyapps.io/Mapa_Interactivo_Hospedajes_Argentina/?_ga=2.91187288.370091405.1672937106-2073232725.1672937106)<br>

<br>
