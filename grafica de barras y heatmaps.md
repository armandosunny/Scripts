**###grafica de barras#####**

`sites <-rep(c("AmanalcoA", "AguaBendita", "HuacalViejo", "AmanalcoB", "MesonViejo", "AmanalcoC", "Raices", "Lagrimas", "RanchoViejo", "ElContadero", "Carretera", "SantaCruz", "PaloSeco", "SanJuan"))`
`probertsi <-c(46, 4, 0, 22, 20, 16, 4, 19, 26, 0, 9, 9, 15, 1)`
`acephalica <-c(0, 9, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0)`
`pleprosa <-c(0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 2, 0, 5, 0)`
`ibelli <-c(2, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0)`
`values <-c(probertsi, acephalica, pleprosa, ibelli)`
`type <-c(rep("Probertsi", 14), rep("Acephalica", 14), rep("Pleprosa", 14), rep("Ibelli", 14))`
`mydata <-data.frame(sites, values, type)`

`p <-ggplot(mydata, aes(sites, values))`
`p +geom_bar(stat = "identity", aes(fill = type))+` 
`theme(axis.text.x=element_text(angle=70, size=5, vjust=0.6))`



**###HEATMAP###**

**#####cargar una tabla copiada en el porta papeles a R############**
`m1=read.delim("clipboard")`
`m8=read.delim("clipboard", header=TRUE)`

#la base la copiamos del excel en este formato

	Amanalco A	Rancho Viejo	Meson Viejo	Amanalco C	Carretera	Palo Seco	Lagrimas	Amanalco B	Santa Cruz	Agua Bendita	Raices
Amanalco A	0.00	0.04	0.01	0.01	0.04	0.09	0.06	0.05	0.09	0.06	0.18
Rancho Viejo	0.04	0.00	0.07	0.07	0.08	0.12	0.16	0.05	0.06	0.04	0.20
Meson Viejo	0.01	0.07	0.00	0.00	0.02	0.08	0.10	0.08	0.08	0.07	0.21
Amanalco C	0.01	0.07	0.00	0.00	0.01	0.05	0.10	0.04	0.07	0.04	0.14
Carretera	0.04	0.08	0.02	0.01	0.00	0.02	0.07	0.05	0.04	0.03	0.12
Palo Seco	0.09	0.12	0.08	0.05	0.02	0.00	0.10	0.09	0.07	0.06	0.09
Lagrimas	0.06	0.16	0.10	0.10	0.07	0.10	0.00	0.05	0.09	0.07	0.11
Amanalco B	0.05	0.05	0.08	0.04	0.05	0.09	0.05	0.00	0.01	-0.03	0.07
Santa Cruz	0.09	0.06	0.08	0.07	0.04	0.07	0.09	0.01	0.00	-0.03	0.09
Agua Bendita	0.06	0.04	0.07	0.04	0.03	0.06	0.07	-0.03	-0.03	0.00	0.04
Raices	0.18	0.20	0.21	0.14	0.12	0.09	0.11	0.07	0.09	0.04	0.00

**#####heatmap con bases ya creadas#####**

`library(reshape2)`

`m8=read.delim("clipboard", header=TRUE)`
`m8`

`melted_jost <- melt(m8)`
`melted_jost`

`ggheatmap=ggplot(data = melted_jost, aes(x=X, y=variable, fill=value)) +` 
`geom_tile()`

**#####para agregarle valores a los colores######**
`ggheatmap+`
`geom_text(aes(X, variable, label = value), color = "black", size = 3)`

**###para selecionar los colores deseados####**

`ggheatmap=ggplot(data = melted_jost, aes(x=X, y=variable, fill=value)) +` 
`geom_tile(color='white')+`
`scale_fill_gradient2(low='dark green',mid='grey',high='dark red',midpoint=0, limit=c(-1,1),name='Jost')`
`ggheatmap`



`ggheatmap=ggplot(data = melted_jost, aes(x=X, y=variable, fill=value)) +` 
`geom_tile(color='white')+`
`scale_fill_gradientn(colours = cm.colors(7))`
`ggheatmap`

**#para mas colores checar esta pagina:** https://rpubs.com/htejero/212365