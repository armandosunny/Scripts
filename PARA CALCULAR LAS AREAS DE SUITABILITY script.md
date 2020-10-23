**###PARA CALCULAR LAS AREAS DE SUITABILITY EN KM2###**

`aneus50=raster("Sceloporus_aeneus_avg2050surban.asc")`
`m.df <- as.data.frame(aneus50, xy=TRUE)`
 `m.df1 <- na.omit(m.df)`
`m.df2 <- m.df1[m.df1$Sceloporus_aeneus_avg2050surban> 0.8,] ###EL 0.8 ES EL AREA DE SUITABILITY A PARTIR DE QUE QUEREMOS CALCULAR EL AREA######keep only cells that that have a suitability score above 0.5 (scores range from 0 to 1)`
`m.raster <- rasterFromXYZ(m.df2)`
`cell_size<-area(m.raster, na.rm=TRUE, weights=FALSE)`
`cell_size1<-cell_size[!is.na(cell_size)]`
`raster_area_aneus50<-length(cell_size1)*median(cell_size1)`
`raster_area_aneus50`