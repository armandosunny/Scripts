**#####Comparacion de nichos#####**

`library(ecospat)` #comparacion de nichos en espacio ambiental - Broenimann et al. 
`library(raster)`  #manipular rasters
`library(SDMTools)`# varios analisis relacionados con modelos de nicho incluyendo comparacion de nichos de Warren 
`library(dismo)` # dismo = distribution modeling
`library(grDevices)` #para otro tipo de plots
`library(gtools)` #para importar todos los .asc
`library(rgdal)`

**####para montar un raster (cualquier formato ascii, bil, tif, ...) la funcion es 'raster'####**


#para calcular sobrelape de nicho segun Warren en espacio geografico:
#paquete SDMTools
#Istat(x, y, old = FALSE)

`Iobs <- Istat(meta,pata,old=FALSE)` 

############################
#aa = robertsi
#ad = leprosa

`aa <- read.csv('registrosprobertsi.csv', header=TRUE)`
`ad <- read.csv('registrospleprosa.csv', header=TRUE)`

**#variables climaticas**

`setwd("C:/Users/Sunny/Desktop/modelado sceloporus/sobrelapamiento y niche breath/2016 eje/eje16")`
`list.asc=mixedsort(list.files(pattern = ".asc"))`
`varclim=stack(list.asc)`

**#generar una tabla que para cada pixel (coordenada x, y) me diga el valor de todas las variables ambientales**
`climpunto <- rasterToPoints(varclim[[1]], fun=NULL, spatial=TRUE)`
**#extraemos datos ambientales para cada capa en varclim de cada coordenada en climpunto**
`clim <- extract(varclim, climpunto)`
**#formateamos clim para que sea una tabla normal con dos primeras columnas x y y**
`clim <- data.frame(coordinates(climpunto),clim)`

**#quiero anular puntos que esten a menos de 5 km de distancia**

#Si 1 grado = 111 km , entonces 5 km = 0.045

#Para 1km = 0.009

**###PARA QUE FUNCIONE LA BASE DE DATOS QUE CARGAMOS TIENE QUE TENER (X, Y) EN VEZ DE (LAT, LONG) EN LA SEGUNDA Y TERCER FILA###**
``occ.sp1 <- ecospat.occ.desaggregation(xy=aa, min.dist=0.009, by=NULL)`
occ.sp2 <- ecospat.occ.desaggregation(xy=ad, min.dist=0.009, by=NULL)` 

**#Añadir variables climaticas a datos**
`occ_sp1<-na.exclude(ecospat.sample.envar(dfsp=occ.sp1,colspxy=1:2, colspkept=1:2,dfvar=clim, colvarxy=1:2,colvar="all",resolution=0.1))`

`occ_sp2<-na.exclude(ecospat.sample.envar(dfsp=occ.sp2,colspxy=1:2, colspkept=1:2,dfvar=clim, colvarxy=1:2,colvar="all",resolution=0.1))`

#lista de especies

`sp.list<- c('robertsi','cephalica')`

#numero de ocurrencias por especie

`sp.nbocc<-c(30,46)`

#Que variables incluir en los analisis?

`Xvar<-c(1:14)`
`nvar<-length(Xvar)`

#numero de iteraciones
`iterations<-10`

#resolucion de espacio ambiental
`R=100`

**`########################################################################`**
**`############################ PCA-ENVIRONMENT ###########################`**
**`########################################################################`**

#set de datos para el PCA que incluya todos los sitios para el area de estudio y para todas las ocurrencias de las especies

`data<-rbind(clim[,1:14],occ_sp1[,1:14],occ_sp2[,1:14])`
`#primero clim, despues robertsi y despues leprosa`

#vector de peso 0 para las ocurrencias y 1 para todos los sitios del area de estudio
`w<-c(rep(1,nrow(clim)),rep(0,nrow(occ_sp1)),rep(0,nrow(occ_sp2)))`

`head(data)`
`dim(data)`

#El PCA se realiza con todos los datos del area de estudio

#Las ocurrencias no son usadas para la calibracion del PCA pero sus coordenadas en los PCA son calculadas

`pca.cal <-dudi.pca(data, row.w = w, center = T, scale = T, scannf = F, nf = 2)`

#filas en que estan los datos de clim y de cada una de las especies:
`row.clim <- 1:nrow(clim)`
`row.sp1 <-  (1+nrow(clim)):(nrow(clim) + nrow(occ_sp1))`
`row.sp2 <-  (1 + nrow(clim) + nrow(occ_sp1)) : (nrow(clim) + nrow(occ_sp1) + nrow(occ_sp2))`

#las coordenadas en cada uno de los ejes del PCA para los datos de toda el area de estudio y de cada una de las especies

  ``scores.clim <- pca.cal$li[row.clim,] #coordenadas para todas las celdas`
  `scores.sp1  <- pca.cal$li[row.sp1,]   #coordenadas para sp1`
  `scores.sp2  <- pca.cal$li[row.sp2,]`   #coordenadas para sp2`

#Contribucion de cada variable a cada componente del PCA
`ecospat.plot.contrib(contrib=pca.cal$co, eigen=pca.cal$eig)`
#Metricos de sobrelape de nicho observado (D - el metrico de Schoener e I - el metrico de Warren)



**########################################################################**
**########### SUPERFICIE DE DENSIDAD DE OCURRENCIAS ######################**
**########################################################################**

#Generar superficies de densidad de ocurrencias en espacio ambiental (maximos dos ejes)
`z1<-ecospat.grid.clim.dyn (scores.clim, scores.clim, scores.sp1, R=100)`
`z2<-ecospat.grid.clim.dyn (scores.clim, scores.clim, scores.sp2, R=100)`

`ecospat.niche.overlap (z1, z2, cor=TRUE)`

#Grafico de densidad de ocurrencias en espacio ambiental
`ecospat.plot.niche (z1, title="robertsi", name.axis1="PC1", name.axis2="PC2", cor=F)`
`ecospat.plot.niche (z2, title="cephalica", name.axis1="PC1", name.axis2="PC2", cor=F)`

**###############################################################  
#####PARA GRAFICAR VARIOS NICHOS EN UN SOLO GRAFICO############**
**###############################################################**

#test of niche equivalency and similarity

`sim1<-ecospat.niche.similarity.test(z116,z216,rep=100,alternative = "greater",rand.type = 1)` #niches randomly shifted in both area
`sim2<-ecospat.niche.similarity.test(z116,z216,rep=100,alternative = "greater",rand.type = 2)` #niche randomly shifted only in invaded area



#niche tests plots

`ecospat.plot.overlap.test(equ,"D","Equivalency")`
`ecospat.plot.overlap.test(sim1,"D","Similarity 1<->2")` #niches randomly shifted in both areas
`ecospat.plot.overlap.test(sim2,"D","Similarity 1->2")` #niche randomly shifted only in invaded area

`ecospat.plot.niche.dyn(z116,z216,quant=0.5,title="bellii vs leprosa",name.axis1="PC1",name.axis2="PC2",interest=1)`
`ecospat.plot.niche.dyn(z116,z216,quant=0.5,title="bellii vs leprosa",name.axis1="PC1",name.axis2="PC2",interest=2)`