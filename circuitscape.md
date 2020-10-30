###para hacer mapas de circuitscape###


####coorrer Maxent####

###para convertir el mapa de maxent a la inversa para poder usarlo como mapa de resistencia####

arcgis
spatial analyst tools
map algebra
raster calculator
1-capa
ok


###para preparar el mapa de entrada para circuitscape###


##R##
#PARA CARGAR UN MAPA EN R EN TIFF#
#definir directorio de trabajo#
session->Set working directory->choose directory->

##cargar librer’as##
library(tiff)
library(raster)

nevado="nevado.tif"
rast=raster(nevado)
plot(rast)
writeRaster(rast, "nevadoraster.asc")

##IDRISI##
#para importar el mapa raster a idrisi##

File->import->software-specific formats->ESRI formats->Arcraster
Marcamos Arcinfo raster ascii format to idrisi
input file: cargar archivo en raster
output file: dar nombre de salida 
output reference information->plane:programfiles:buscar sistema de coordenadas del raster (lat long, UTM)
ok

#para reclasificar los pixeles o valores##

reclass (checar que estŽn marcados image y user-defined reclass
input file: cargar archivo en raster (nevadorasteridrisi)
output file: dar nombre de salida 
asignar valores:

assig a new value of/ to all values form/ to just less than
-9999                   -32768                   1
20                         1                     2
40                         2                     3
ok

###para cambiar coordenadas lat long a utm###

Data management tools
project and transformations
Project raster
checar que esté marcado en raster
input file name: cargar archivo raster idrisi
output file name: nombrar archivo de salida
reference file for output result: UTM-14N
resample type: Nearest Neighbor
background value: -9999
ok

#para exportar el mapa de idrisi a ascii##
File->import->software-specific formats->ESRI formats->Arcraster
Marcamos Idrisi to ArcInfo raster ASCII format
input file: cargar archivo en rasteridirsi
output file: dar nombre de salida 
ok



##para preparar las localidades de colecta##
tener las coordenadas en cvs delimitado por comas
ejemplo de la base de datos:
id	x	y
1	-99.95405	19.20265
2	-99.88972	19.0993



##ARCGIS##
#para convertir los puntos de colecta en shape#
File->add data->add XY data
choose a table from the map or browse for another table:elegir el archivo csv
X field: x
Y field: y
ok

#para convertir las localidades cargadas a shp#
ArcToolbox->conversion tools->to shapefile->Feature class to shapefile 
input feautres (seleccionar en la pestaña archivo csv)
output folder: carpeta para guardar
ok

##para importar un shp (puntos de colecta)a idrisi##
File->import->software-specific formats->ESRI formats->Shapeidr
Marcamos shape file to idrisi
input Shapefile:cargar capa de puntos en shape
output idrisi vector file: nombre de salida del archivo
Reference system: buscar sistema de coordenadas del shape (lat long, UTM)
ok

#para convertir de shp (vector) a raster#
rastervector
checar que esté marcado "vector to raster" y "point to raster"
vector point file: cargar archivo
image file to be updated: nombrar archivo
checar que esté marcado "change cells to record the identifiers of points"
ok
sale una ventana de initial
marcar copy spatial parameters from another image
outout image: ya está cargado
image to copy parameters from: cargar archivo (nevadoreclassidrisi)
ok

#para exportar el mapa de idrisi a ascii##
File->export->software-specific formats->ESRI formats->Arcraster
Marcamos Idrisi to ArcInfo raster ASCII format
input file: cargar archivo en rasteridirsi
output file: dar nombre de salida 
ok

###Definir proyeccion del mapa######
Data management tools
project and transformations
raster
Define proyection
Geographic Coordinates Systems
World
WGS1984
ok

###para cambiar coordenadas lat long a utm###
Data management tools
project and transformations
raster
project raster
checar que esté marcado en raster
input file name: cargar archivo raster idrisi
output file name: nombrar archivo de salida
reference file for output result: UTM-14N
resample type: Nearest Neighbor
background value: -9999
ok

#para exportar el mapa de idrisi a ascii##
File->import->software-specific formats->ESRI formats->Arcraster
Marcamos Idrisi to ArcInfo raster ASCII format
input file: cargar archivo en rasteridirsi
output file: dar nombre de salida 
ok



###############################CIRCUITSCAPE####################################

Step 1: raster
step 2: pairwise:iterate across all pairs in focal node file
Input resistance data: cargar mapa de resistencia de UTM
Pairwise mode options: cargar mapa de localidades en UTM
Output options: nombrar archivo de salida
checar que este marcado current maps y log completion times
Run




####PARA HACER LOS CORE AREAS####

### cambiar sistemas de coordenadas o definir coordenadas en arcgis###
Acrctoolbox->Data management tools->Projections and transformations->Define projection

Input: se mete la capa que se quiere definir las coordenadas

coordinates systems:se selecciona el sistema de coordenadas que tiene:Projected Coordinate Systems->UTM->WGS1984->Northern Hemisphere->Zone14N

Aceptar
Ok



###RECLASS PARA TENER LOS CORE AREAS####



#para importar el mapa raster de CircuitScape a idrisi##

File->import->software-specific formats->ESRI formats->Arcraster
Marcamos Arcinfo raster ascii format to idrisi
input file: cargar archivo en raster
output file: dar nombre de salida 
output reference information->plane:programfiles:buscar sistema de coordenadas del raster (lat long, UTM)
ok



###para reclasificar los pixeles o valores###

reclass (checar que estŽn marcados image y user-defined reclass
input file: cargar archivo en raster (nevadorasteridrisi)
output file: dar nombre de salida 
asignar valores:

assig a new value of/ to all values form/ to just less than
1                  0.06                   1
0                  0                      0.06
ok




###Para convertir rater a shape en  IDRISI (es mejor hacerlo en ARCGIS)###
Rastervector
Raster to vector
Raster to polygon
Output
Idisi to polygon file
Input:seleccionamos la capa que queremos exportar
Output idrisi polygon file: ponemos el nombre del archivo de salida

Marcamos la Casilla de exclude background polygon y le asignamos el valor "0"

Ok

ShapeIDR
Idrisi to Shapefile
Export vector collection
Input idrisi vector Collection:seleccionamos la capa que vamos a transformar
Output shape file: ponemos el hombre de salida del archivo
ok


###Para cargar la capa reclasificada de IDRISI a ARCGIS###

Convertiontools->fromraster->raster to polygon
Meto la capa, des selecciono la casilla de simplified y pongo donde guardar




###para convertir todos los poligonos en unos solo (Core Areas)###


Tabla de atributos-select by attributes (ojo: esto es en la tabla no en el arctoolbox)

gridcode=1 (esto dependiendo del valor que tengan los coreareas asignados)

Analysis tools->extract->select

Luego vuelvo a seleccionar los poligonos mas grandes (coreareas) manualmente y de nuevo:
Analysis tools->extract->select



###agregar un identificador a cada Core Area###

Open attribute table opcions->add field->ponerle nombre al identificador "identifica"-> ok -> cerrarlo
Editor->start editing->selecionamos el shape que queremos editar-> continue
Vamos a tabla de atributos y agregamos el valor a cada campo que creamos (1,2,3,4,5,6,7,8,9,10,11; dependiendo del numero de core areas que tengamos), lo cerramos y luego vamos a editor y stop editing-> save->yes



###Para correr Linkage Mapper###
Se define el directorio de trabajo
Se cargan los Core Areas en SHP
Se carga Core Area field en nuestro caso es "identifica"
Se carga el mapa de resistencia en raster (con los valores que asignamos de resistencia a cada variable del paisaje)


Cambiar el valor en cost weighted distance threshold use in truncating corridors(Se pone que tan grande quieres que sea el corridor de ancho en nuestro caso pusimos 1km que es 1000 metros por que es lo que se cree que m‡ximo puede migrar una salamandra




###para definir que un valor no exista (NO DATA Value) en ARCGIS####

Arcgis->catalog->buscamos el mapa ->no data value-> edit->agregamos el valor (-9999) ok
Si no funciona
Abrimos el txt y le agregamos NODATA_value -9999
Salvamos


####para definir el sistema de coordenadas en ARCGIS####

En arcgis->Data Management Tools->Projections and Transformations->Define Protection->seleccionamos la capa->coordinates systems:se selecciona el sistema de coordenadas que tiene:Projected Coordinate Systems->UTM->WGS1984->Northern Hemisphere->Zone14N

Aceptar
Ok



###Centrality Mapper###
Ya que se corrio lo abrimos en ARCGIS
Properties->symbology->categories->unique values->value field: CF_Central->add all Values->Aceptar

#########PARA SACAR DISTANCIA GENETICA JOST Y OTRAS DISTANCIAS GENETICAS##########
R
library(mmod)

destpair=pairwise_D(obj1, linearized = FALSE, hsht_mean = "arithmetic")


####para guardar tablas generadas por R####
se selecciona la tabla en R y se copia a un block de notas pero tiene que estar en square asi que se tiene que editar primero en excell

####para hacer el RDA#######
primero abrimos el txt de salida de circuit scape "OYAMELCIRCUITOUT_resistances"
y le quitamos el 0 que tiene al principio de la base de datos

#############RDA######################################

library(vegan)

genetic.distance=1-(as.dist(read.table(file.choose())))
cost.distance=as.dist(read.table(file.choose()))
pcoa.genetic=cmdscale(genetic.distance, k=1)
pcoa.cost=cmdscale(cost.distance, k=1)
rda.full=rda((pcoa.genetic~pcoa.cost))
head(summary(rda.full))
anova(rda.full,by="terms")
RsquareAdj(rda.full)
plot(rda.full,pch=19)
regresion=lm(pcoa.genetic~pcoa.cost)
summary(regresion)



#####################################GENETIC LANDSCAPE GIS TOOLBOX#######################


Se necesita el ascii del mapa
Las bases de datos como en el ejemplo del software "apomastus_example input"

Se carga la extensi—n a ARCGIS
Genetic tools->single Species Genetic Divergence
Input points->Se carga la base de datos en excel 97
Excel worksheet->selecionamos la pagina donde est‡n la diversidad genŽtica (Ho)
Output cell size-> se pone el valor del pixel (10 Mts)
Power->2
Variable
Number of points->11 (number de localidades)

Study area grid->garramos el ascii de nuestra zona de estudio
Standarized grid-->dejamos el nombre que tiene y solo cambiamos la ultima parte por nuestra especie
Raw divergency grid->dejamos el nombre y solo cambiamos la ultima parte por nuestra especie

Coord’nate system ponemos-> ponemos el sistema de coordenadas que definimos en el excel y con el que estamos trabajando (Proyected coordinate systems->World->WGS1984 World Mercator)