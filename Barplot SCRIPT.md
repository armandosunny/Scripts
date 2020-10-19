`df <- data.frame(`
  `group = c("Octavio Monroy Vilchis", "Armando Sunny", "Victor Fajardo"),`
  `value = c(4, 3, 2)`
  `)`
`head(df)`

`library(ggplot2)`

# Barplot
`bp<- ggplot(df, aes(x="", y=value, fill=group))+`
`geom_bar(width = 1, stat = "identity")`
`bp`


# pieplot

`pie <- bp + coord_polar("y", start=0)+`
`geom_bar(stat="identity", width=1, color="white") +`
`coord_polar("y", start=0) +`
`theme_void()`

`pie`


# Create a basic bar
`pie = ggplot(df, aes(x="", y=value, fill=group)) + geom_bar(stat="identity", width=1)`

# Convert to pie (polar coordinates) and add labels
`pie = pie + coord_polar("y", start=0) + geom_text(aes(label = paste0(round(value*1))), position = position_stack(vjust = 0.5))`

# Add color scale (hex colors)
`pie = pie + scale_fill_hue()`

# Remove labels and add title
`pie = pie + labs(x = NULL, y = NULL, fill = NULL, title = "Artículos por autor`
`Ambystoma leorae")`

# Tidy up the theme
`pie = pie + theme_classic() + theme(axis.line = element_blank(),`
          `axis.text = element_blank(),`
          `axis.ticks = element_blank(),`
          `plot.title = element_text(hjust = 0.5))`

`pie`

**###grafica de barras super bien con titulos y en cursivas SUNNY###**

`df <- data.frame(`
  `group = c("1. Octavio Monroy-Vilchis", "2. Armando Sunny", "3. Victor Fajardo", "4. Martha Zarco-González", "5. Julio Lemos-Espinal"),`
  `value = c(4, 3, 2, 2, 1)`
  `)`
`head(df)`

`library(ggplot2)`

# Create a basic bar
`pie = ggplot(df, aes(x="", y=value, fill=group)) + geom_bar(stat="identity", width=1)`

# Convert to pie (polar coordinates) and add labels
`pie = pie + coord_polar("y", start=0) + geom_text(aes(label = paste0(round(value*1))), position = position_stack(vjust = 0.5))`

# Add color scale (hex colors)
`pie = pie + scale_fill_hue()`

# Remove labels and add title
`pie = pie + l`abs(x = NULL, y = NULL, fill = NULL, title = "Artículos por autor",`
              `subtitle = "Ambystoma leorae",`
              caption = "Data source: Web of Science")`

# Tidy up the theme
`pie = pie + theme_classic() + theme(axis.line = element_blank(),`
          `axis.text = element_blank(),`
          `axis.ticks = element_blank(),`
          `plot.title = element_text(hjust = 0.5))+`

  `theme(`
    `plot.title = element_text(hjust = 0.5, size = 14),    # Center title position and size`
    `plot.subtitle = element_text(hjust = 0.5, face = "italic"),            # Center subtitle`
    `plot.caption = element_text(hjust = 0, face = "italic")# move caption to the left`
  `)`



`pie + theme(legend.text = element_text(size=13, face="bold"))+` 
`theme(legend.position="bottom")`