**###Para copiar una tabla e importarla a R##**

pruebaR<-read.delim("clipboard")



**###Para graficar las curvas de respuesta de maxent en R una por una###**


Pseudoeurycea_robertsi_0_agriidrisiRRAIFA <- read.csv("E:/MODELADOMONSE/MAXENT4/MODELADO4 REPLICAS/plots/Pseudoeurycea_robertsi_0_agriidrisiRRAIFA.dat")
View(Pseudoeurycea_robertsi_0_agriidrisiRRAIFA)


ggplot(Libro1, aes(x=x, y=y)) +
geom_point(color="red") +
geom_line()+
theme_light()

ggplot(agri, aes(x=x, y=y,geom=c("smooth"), method="lm", formula= y~log10(x), level=0.95)) +
geom_point(color="red") +
theme_light()

geom=c("smooth"), method="lm", formula= y~log10(x), level=0.95



**##Mucho mejor grafica###**

qplot(x, y, data=agri, geom=c("smooth"), method="lm", formula= y~log10(x), level=0.95) +
labs(y="Logistic output (Probability of presence)", x="Agriculture")

qplot(x, y, data=oyamel, geom=c("smooth"), method="lm", formula= y~log10(x), level=0.95) +
labs(y="Logistic output (Probability of presence)", x="Abies Forest")

qplot(x, y, data=pino, geom=c("smooth"), method="lm", formula= y~log10(x), level=0.95) +
labs(y="Logistic output (Probability of presence)", x="Pinus Forest")

qplot(x, y, data=altitud, geom=c("smooth"), method="lm", formula= y~log10(x), level=0.95) +
labs(y="Logistic output (Probability of presence)", x="Altitude")

qplot(x, y, data=aspect, geom=c("smooth"), method="lm", formula= y~log10(x), level=0.95) +
geom_point(color="red") +
labs(y="Probability of presence", x="Aspect")


qplot(x, y, data=slope, geom=c("smooth"), method="lm", formula= y~log10(x), level=0.95) +
labs(y="Probability of presence", x="Slope")

qplot(x, y, data=Libro1, geom=c("smooth"), method="lm", formula= y~log10(x), level=0.95) +
labs(y="Genetic Distance", x="Geograpihc Distance")



**##para graficar varias variables en una sola grafica###**
se juntan todas las bases de datos en un solo excel

ggplot(variables, aes(x, y, color = ï..veg)) +
    geom_smooth(method = "lm", formula= y~log10(x), level=0.95) +
labs(y="Probability of presence", x="Variables")



**####Para graficar una prueba de MANTEL en R###**

modelo3 <- ggplot(Libro1, aes(x=X, y=Y, color="red")) + 
geom_point(size=3) +
geom_smooth(method = "lm", formula = y~poly(x,1), level=0.95, color="blue") +
labs(y="Geograpihc distance", x="Genetic distance")

modelo3

**###cargar una tabla copiada en el porta papeles a R###**
m1=read.delim("clipboard")
m8=read.delim("clipboard", header=TRUE)

**La base la copiamos del excel en este formato**

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

**##Heatmap con bases ya creadas###**

library(reshape2)

m8=read.delim("clipboard", header=TRUE)
m8

melted_jost <- melt(m8)
melted_jost

ggheatmap=ggplot(data = melted_jost, aes(x=X, y=variable, fill=value)) + 
geom_tile()

**###Para agregarle valores a los colores###**
ggheatmap+
geom_text(aes(X, variable, label = value), color = "black", size = 3)

**###Para seleccionar los colores deseados###**

ggheatmap=ggplot(data = melted_jost, aes(x=X, y=variable, fill=value)) + 
geom_tile(color='white')+
scale_fill_gradient2(low='dark green',mid='grey',high='dark red',midpoint=0, limit=c(-1,1),name='Jost')
ggheatmap

ggheatmap=ggplot(data = melted_jost, aes(x=X, y=variable, fill=value)) + 
geom_tile(color='white')+
scale_fill_gradientn(colours = cm.colors(7))
ggheatmap

**para mas colores checar esta pagina:** https://rpubs.com/htejero/212365