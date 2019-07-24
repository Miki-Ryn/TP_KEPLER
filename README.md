# TP_KEPLER
Trabajo Prático hecho con Kepler.gl

# OBJETIVO: 

  Al venir del entorno de la construcción y el real estate, se tuvo la intención de aprovechar la riqueza de datos abiertos de estadísticas disponibles en las webs públicas para hacer una visualización que guíe la toma de decisión del privado en el momento de hacer una inversión en m2 para renta de alquiler. 
  Combinando la información accesible en opendata-ajuntament.barcelona.cat, las herramientas de Excel+Qgis y por último hacienod uso de los recursos provistos por Kepler.gl, realizar un visor en le que se pueda comparar la evolución de la rentabilidad de los barrios en el tiempo. 
  
# DESARROLLO:

## 1) Mapa Base
Generar Mapa base a partir de shapefiles descargados de ICGC. Mapa base que aporte la información geométrica-espacial de los barrios y los distritos que contienen a esos barrios para tod la ciudad de Barcelona.

## 2) Descargar Datasets de Dades Obertes - BCN. 
Encontramos en la pagina de datos abiertos del Ayuntamiento de Barcelona (opendata-ajuntament.barcelona.cat) series de valores a intervalos trimestrales entre los años 2014 y 2018 incluido. Las series fueron: 
  a) Superficie media de viviendas alquiladas [m2].
  b) Número de contratos de alquiler realizados [n].
  c) Valor monetario medio de los contratos realizado [€].
  
## 3) Procesar datos y emprolijar. 
Las series fueron descagadas en formato CSV por lo que se tuvieron que pasar por excel para procesarse: 
  a) Texto en columnas
  b) Interpolación ponderada de valores nulos. 
  c) Asignación de tipos de dato (texto, numero, fecha)
  d) Recorte de columnas con valores acumulados (no se utilizarían)
  e) Cambio de nombres de columnas para formar campos en el software QGIS
  
## 4) Idear indices 
A partir de la información obtenida ideamos y obtenemos los indices ideados. 
	a) n contratos / area por barrio --> Densidad de locaciones
	b) Normalización de densidad de locaciones normalizada --> indicador de rotatividad (o cuan dinámico es el barrio) [Xd]
	c) valor medio de contrato de alquiler / superficie media alquilada --> rentabiliddad media por m2 del barrio 
	d) Normalización de rentabilida media normalizada --> Indicador de rentabilidad [Xr]
	e) Valorar y ponderar peso de cada indicador --> 2Xr+Xd : Ponderación cruzada
	f) Normalización de ponderación cruzada --> Indicador Global
  
## 5) Simplificar geometrías 
En el programa QGIS simplificamos la geometría de los poligonos base a tolerancia de 0.0001 para aligerar movimiento y carga de datos

## 6) Unirs datos
Hacemos un Join entre las geometrías de barrios con tabla de indicadores globales tirmestrales.

## 7) Reproyectar a EPSG 4326 (Coordenadas Geográficas Mudiales s/ Datum WGS84)
Kepler.gl requiere que los Gjson que se carguen a su web estén en el dicho marco de referencia. 

## 8) Exportar capa en formato GeoJson

## 10) Cargar en Kepler base de datos Geojson
Utilizando el drag&drop provisto en el portal, podemos agregar capas a nuestro proyecto.

## 11) Generación de capas
Generamos una capa por cada trimestre (16 en total) con la siguiente configuración:
	a) Geometría --> Polígono  --> base: Geometría .json
	b) Color de relleno --> en f(x) de Distrito al que pertenece el barrio
	c) Borders --> No
	d) Height --> valor modal 15 y adjudicar en f(x) del valor del Indice del trimestre que corresponda. 
	e) Fondo--> Mute Night
  
## 12) Exportar como .html para hacer un embed para ver de un navegador web. 

## 13) Exportar como .json para BU.

## 14) Crear un embed de html para cargar el visor.

## 15) Subir a Github y publicar.

