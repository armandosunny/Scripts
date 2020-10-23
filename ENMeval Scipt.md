**###############################ENMeval Scipt################################**

#Presence database (only two columns; longitude and latitude, in that order, in csv)#
#Base de datos de presencia (solo dos columnas; longitude y latitude, en ese orden, en csv)#

`bv=read.csv("registrosprobertsi.csv")`


#Convert to data frame#
#Convertir a data frame#

`occs=as.data.frame(bv[,1:2])`
`plot(occs)`

`library(raster)`

#Environmental layers#
#Capas ambientales#

`agri=raster("agriidrisiRRAIFA.asc")`
`oyamel=raster("oyamelidrisiRRAIFA.asc")`
`pastizal=raster("pastizalidrisRRAIFA.asc")`
`pino=raster("pinoidrisiRRAIFA.asc")`
`urban=raster("urbanoidrisiRRAIFA.asc")`

`envs=stack(agri, oyamel, pastizal, pino,urban)`

#to plot all layers #
#para graficar todas las capas#
`plot(envs)`

#to plot the first layer #
#para graficar la primera capa#

`plot(envs[[1]],main=names(envs)[1])`

#to see the points on the layers #
#para ver los puntos sobre las capas#

`points(occs)`

`library(sp)`

#convert data frame of points becomes an object #
#Para que el data frame de puntos se vuelva un objeto#

`occs.sp=SpatialPoints(occs)`

`library(ENMeval)`

#Generation of 10000 points of all environmental variables#
#Generacion de 10000 puntos de todas las variables ambientales#

`bg=randomPoints(envs,n=10000)`
`plot(bg)`

#to convert to data frame#
#para convertir a data frame#

`bg=as.data.frame(bg)`

#so that the points that were generated are distributed in the study area in a homogeneous way #
#para que los puntos que se generaron se distribuyen en la zona de estudio de manera homogenia#

`plot(envs[[1]],legend=FALSE)`
`points(bg, col='red')`

##models we test##
##modelos puestos a prueba##

`eval1=ENMevaluate(occs, envs, bg, method='checkerboard2', RMvalues=seq(1,2), fc=c("L", "LQ", "H", "LQH", "LQHP", "LQHPT"),aggregation.factor = c(2, 2), algorithm='maxent.jar',parallel=TRUE)`

`blockanalisis=ENMevaluate(occs, envs, bg, method='block', RMvalues=seq(1,2), fc=c("L", "LQ", "H", "LQH", "LQHP", "LQHPT"), algorithm='maxent.jar',parallel=TRUE)`

`checkerboard1analisisb=ENMevaluate(occs, envs, bg, method='checkerboard1', RMvalues=seq(1,2), fc=c("L", "LQ", "H", "LQH", "LQHP", "LQHPT"),aggregation.factor = c(2, 2), algorithm='maxent.jar',parallel=TRUE)`

Jackknife
`Jackknife=ENMevaluate(occs, envs, bg, method='jackknife', RMvalues=seq(1,2), fc=c("L", "LQ", "H", "LQH", "LQHP", "LQHPT"), algorithm='maxent.jar',parallel=TRUE)`


#Results#
#Resultados#

#algorithm we use#
#algoritmo que usamos#

`eval1@algorithm`

#AUC and AICc#
#AUC y AICc#
`eval1@results` 

#to evaluate which is the best model considering the different RM values and the feature classes#
#para evaluar cual es el mejor modelo considerando los diferentes valores de RM y las feature classes#
`eval1@results[which(eval1@results$delta.AICc==0),]` 

#ploth the best model#
#graficamos el mejor modelo#
`plot(eval1@predictions[[which(eval1@results$delta.AICc==0)]])`

#To plot the presence points on the model#
#Para graficar los puntos de presencia sobre el modelo
`points(eval1@bg.pts, pch=3, col=eval1@bg.grp, cex=0.5)` 

#To plot the points of the environment on the model#
#Para graficar los puntos del ambiente sobre el modelo#
`points(eval1@occ.pts, pch=21, bg=eval1@occ.grp)` 

#We name the model with the best AIC#
#Le damos nombre al modelo con el mejor AIC#
`aic.opt=eval2@models[[which(eval2@results$delta.AICc==0)]]` 
`aic.opt@results`

#to see the contribution of the variables (percent contribution and permutation importance)#
#para ver la contribuciÛn de las variables (percent contribution and permutation importance)#
`var.importance(aic.opt)`



#Appears in the name of each variable, followed by the slope (the variable coefficient), then the minimum and maximum value of the variable appears. If the slope (coefficient) is 0, it means that the variable was not included in the model.#

#Aparece en nombre de cada variable, seguido de la pendiente (the variable coefficient), despuÈs aparece el valor mÌnimo y m·ximo de la variable. Si la pendiente (coeficiente) es 0, significa que la variable no se incluyÛ en el modelo.

`aic.opt@lambdas` 


#To graph the percent contribution and permutation importance#
#Para graficar el percent contribution and permutation importance#

`df=var.importance(aic.opt)` 
`barplot(df$permutation.importance, names.arg=df$variable, las=2, ylab="Permutation Importance")`
`barplot(df$percent.contribution, names.arg=df$variable, las=2, ylab="Percent Contribution")`

#To plot several models together #
#Para graficar varios modelos juntos#
`par(mfrow=c(1,2), mar=c(2,2,1,0))`
`plot(eval1@predictions[['L_2']], ylim=c(-30,20), xlim=c(-90,-40), legend=F, main='L_2 prediction')`
`plot(eval1@predictions[['LQP_1']], ylim=c(-30,20), xlim=c(-90,-40), legend=F, main='LQP_1 prediction')`#We are putting all the models that we want to graph# #Vamos poniendo todos los modelos que queremos graficar#

#To graph the response curves#
#Para graficar las curvas de respuesta#
`response(aic.opt)` 

#to save the map in logistic and not raw#
#para guardar el mapa en logistico y no en raw#
`GClog <- predict(checkerboard2analisisb@models[[4]], envs, args=c("outputformat=logistic"))`
`plot(GClog)`


#to save the map in .asc #
#para guardar el mapa en .asc#

`writeRaster(GClog,"mapalogistico.asc")`