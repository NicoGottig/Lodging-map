# Mapa Interactivo de Precios de Hospedaje en Argentina 2023

## Resumen

Esta aplicación resume, para distintos hospedajes de Booking en el año 2023 (para dos personas adultas, sin hijos), los **precios**, la **cantidad** y la **puntuación** de hoteles, cabañas y otros tipos de alojamientos en Argentina.

La información se recopiló mediante técnicas de Web Scraping, se almacenó en una base de datos relacional, y se transformó y validó para evitar valores nulos. No se realizó un tratamiento de datos atípicos, ya que la selección del rango de precios depende exclusivamente del usuario.

Para la visualización, se desarrolló una aplicación interactiva *Shiny*, con un mapa dinámico generado a través del paquete *leaflet*.

- 📄 [Acceder al Script de Manipulación y Validación de Datos](https://github.com/NicoGottig/Lodging-map/blob/main/Scripts/02_tfi_manipulacion-validacion.R)
- 🌐 [Explorar la Aplicación Interactiva Shiny](https://mj8qpg-nicolas-gottig.shinyapps.io/Mapa_Interactivo_Hospedajes_Argentina/?_ga=2.91187288.370091405.1672937106-2073232725.1672937106)

---

## Objetivo y Metodología

El objetivo de esta aplicación es resumir y presentar información en función de las solicitudes del usuario. Para ello, se extrajo información sobre la localización, precio, descripción y puntaje de más de 150.000 hospedajes en Argentina.

Es **importante** considerar que el precio del hospedaje depende, entre otros factores, de la fecha de extracción de los datos. Por ejemplo, no es lo mismo buscar precios de hospedajes para enero en diciembre que en octubre.  
*En este caso, los datos fueron extraídos durante los meses de diciembre y enero.*

### Cálculo de Estadísticos de Precios

Se realizó un muestreo no probabilístico para calcular los estadísticos de precios, seleccionando $n$ ciudades por provincia en base a consultas con empleados de empresas de servicios turísticos y recomendaciones en línea.

El sitio *Booking* puede sugerir hospedajes de otras localidades, por lo que para una cantidad $n$ de ciudades solicitadas, se generará una cantidad $k$ de ciudades disponibles, cada una con $h$ hospedajes, y así sucesivamente para las 23 provincias (incluyendo a la Ciudad Autónoma de Buenos Aires dentro de la provincia de Buenos Aires).

---

## Variables de la Base de Datos

La base de datos construida mediante técnicas de Web Scraping incluye las siguientes variables:

- **Localización**: Ciudad del hospedaje.
- **Título**: Nombre del hospedaje tal como aparece en Booking.
- **Descripción**: Breve resumen del hospedaje.
- **Clasificación**: Puntuación del hospedaje (de 1 a 10).
- **Precio**: Costo del hospedaje por el total de días solicitados.
- **Impuesto**: Impuesto total por la cantidad de días solicitados.
- **Noches**: Número de noches solicitadas.
- **Provincia**: Provincia del hospedaje.
- **Checkin**: Fecha de ingreso al hospedaje.
- **Checkout**: Fecha de salida del hospedaje.

### Variables Adicionales Calculadas

- **Precio_noche**: Precio neto por noche del hospedaje (Precio_Noche=Precio/Noches).
- **Impuesto_noche**: Impuesto por noche del hospedaje (Impuesto_noche=Impuesto/Noches).
- **Bruto_noche**: Precio total por noche, incluyendo impuestos (Bruto_Noche=Precio_noche+Impuesto_Noche).

---

## Proceso de Desarrollo

### 1 - Extracción de Datos

Se programó una función para extraer datos mediante consultas específicas (e.g., hospedajes en la ciudad de Paraná), iterando sobre las páginas disponibles en Booking para recopilar información de cada alojamiento. En caso de no encontrar datos, se asigna un valor nulo. Las consultas se dividieron en dos grupos de 15 días para evitar sesgar los promedios.

### 2 - Manipulación y Validación

Se calculó el precio por noche sumando impuestos y dividiendo por el número de noches solicitadas. Se eliminaron valores nulos en los precios de hospedaje y se completaron valores de calificación según su distribución.  
[Más información en el script](https://github.com/NicoGottig/Lodging-map/blob/main/Scripts/02_tfi_manipulacion-validacion.R).

### 3 - Presentación de la Información

Se desarrolló una aplicación interactiva *Shiny* donde el usuario puede seleccionar opciones de estilo y filtros de cálculo para visualizar la información en un mapa. También es posible consultar tablas detalladas con la descripción de cada hospedaje.  
Para acceder a la aplicación, haga [click aquí](https://mj8qpg-nicolas-gottig.shinyapps.io/Mapa_Interactivo_Hospedajes_Argentina/?_ga=2.91187288.370091405.1672937106-2073232725.1672937106).

---

### Notas Finales

Los precios mostrados son considerados como *"precios de mercado"*, es decir, incluyen impuestos, subsidios y el valor agregado de las empresas de servicios turísticos. Por lo tanto, los estadísticos presentados no deben ser considerados indicadores definitivos de precios provinciales de hospedaje, sino más bien una exploración de los datos en un momento y región específicos.

---

# Interactive Map of Accommodation Prices in Argentina 2023

## Summary

This application summarizes, for various Booking accommodations in 2023 (for two adults without children), the **prices**, **quantity**, and **ratings** of hotels, cabins, and other types of lodgings in Argentina.

The data was collected using Web Scraping techniques, stored in a relational database, and then transformed and validated to avoid null values. No treatment for outlier data was performed since the selection of the price range is entirely user-dependent.

For visualization, an interactive *Shiny* application was developed, featuring a dynamic map generated using the *leaflet* package.

- 📄 [Access the Data Manipulation and Validation Script](https://github.com/NicoGottig/Lodging-map/blob/main/Scripts/02_tfi_manipulacion-validacion.R)
- 🌐 [Explore the Interactive Shiny Application](https://mj8qpg-nicolas-gottig.shinyapps.io/Mapa_Interactivo_Hospedajes_Argentina/?_ga=2.91187288.370091405.1672937106-2073232725.1672937106)


---

## Objective and Methodology

The objective of this application is to summarize and present information based on user requests. To achieve this, data was extracted on the location, price, description, and rating of over 150,000 accommodations in Argentina.

It's **important** to note that the price of accommodation depends, among other factors, on the date of data extraction. For example, searching for lodging prices for January in December will yield different results than searching in October.  
*In this case, the data extraction was conducted during December and January.*

### Price Statistics Calculation

A non-probabilistic sampling method was used to calculate price statistics, selecting $n$ cities per province based on consultations with employees of tourism service companies and online recommendations.

The *Booking* site may recommend accommodations from other locations. Therefore, for a requested number of $n$ cities, a different number of $k$ available cities will be generated, each containing $h$ accommodations, and this will apply across the 23 provinces (including the Autonomous City of Buenos Aires within the Buenos Aires province).

---

## Variables in the Database

The database constructed through Web Scraping techniques includes the following variables:

- **Location**: City of the lodging.
- **Title**: Name of the lodging as listed on Booking.
- **Description**: Brief summary of the lodging.
- **Classification**: Rating of the accommodation (from 1 to 10).
- **Price**: Cost of the accommodation for the total number of days requested.
- **Tax**: Total tax for the number of days requested.
- **Nights**: Number of nights requested.
- **Province**: Province of the lodging.
- **Checkin**: Date of entry into the lodging.
- **Checkout**: Date of departure from the lodging.

### Additional Calculated Variables

- **Price_night**: Net price per night of the lodging (Price_Night=Price/Nights).
- **Tax_night**: Tax per night of the lodging (Tax_night=Tax/Nights).
- **Gross_night**: Total price per night, including taxes (Gross_Night=Price_night+Tax_Night).

---

## Development Process

### 1 - Data Extraction

A function was programmed to extract data based on specific queries (e.g., lodgings in the city of Paraná), iterating over available pages on Booking to gather information on each lodging. If no data is found, a null value is assigned. To avoid biasing the average by fortnight, queries were divided into two 15-day groups, and the average of both was calculated.

### 2 - Data Manipulation and Validation

The final database was built by calculating the price per night, adding taxes, and dividing by the number of nights requested. Null values in accommodation prices were eliminated due to their specific characteristics, and rating values were filled in based on their distribution.  
[More information in the script](https://github.com/NicoGottig/Lodging-map/blob/main/Scripts/02_tfi_manipulacion-validacion.R).

### 3 - Presentation of Information

An interactive *Shiny* application was developed for information presentation. Users can select style options and calculation filters to view summarized information on a map. They can also access detailed and complete tables, allowing them to read the description of each lodging.  
To access the application, click [here](https://mj8qpg-nicolas-gottig.shinyapps.io/Mapa_Interactivo_Hospedajes_Argentina/?_ga=2.91187288.370091405.1672937106-2073232725.1672937106).

---

### Final Notes

The displayed prices are considered *"market prices"*; that is, they include taxes, subsidies, and the added value from the companies providing the service. Therefore, the presented statistics should not be considered definitive indicators of provincial lodging prices, limiting the analysis to data exploration within a specific moment and region.


