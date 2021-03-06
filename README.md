# Dia_72_27mayo

```{r}
"Protocolo:

1. Daniel felipe Villa Rengifo

2. Lenguaje: R

3. Tema: normalizacion y  creación de variables ficticias

4. Fuentes:
   https://rpubs.com/ydmarinb/429761"

```


```{r}
housing <- read.csv("BostonHousing.csv")
# Usamos la funcion "scale" (nota: esa función solo funciona para variables numericas)

housing.z <- scale(housing, center = TRUE, scale = TRUE)
# la funcion scale tiene dos argumentos, center y scale
# center = TRUE lo que hace es habilitar la resta de la media 
# scale = TRUE lo que hace es habilitar la division por la divicion tipica (sd)

#NOTA: CON ESTOS PARAMETROS SE PUEDE HACER COMBINACIONES

# solo ajustar el promedio 

housing.mean <- scale(housing, center = TRUE, scale = FALSE)


# Solo dividir por la desviacion estandar 

housing.sd <- scale(housing, center = FALSE, scale = TRUE)

# Queda igual, si no resta la media y no divide

housing.none <- scale(housing, center = FALSE, scale = FALSE)
```



```{r}

# la siguinete funcion sirve para normalizar una multitud de variables

scale.many = function(dataframe, cols){
  "Normaliza las variables, para devolver un dataframe modificado"
  # Nombre de las columnas del dataframe
  names <- names(dataframe)
  #Recorro las columnas 
  for(col in cols){
    #Concateno el nuevo nommbre de la variable 
    name <- paste(names[col], "z", sep = ".")
    #Aplico la fucnion scale a las variables 
    dataframe[name] <- scale(dataframe[,col])
  }
  
  #concateno un mensaje de información 
  cat(paste("Hemos normalizado ", length(cols), " variable(s)"))
  # retorno el data frame normalizado
  return(dataframe)
}

#pruebo la función 

housing <- scale.many(housing, c(1, 3, 5:8))
```

# Categorizando informacion numerica 

```{r}
# lectura de la base datos es pequeña solo a modo de ejemplo 
# se puede scalalar con la misma logica 
students <- read.csv("data-conversion.csv")

# Vamos a categorizar los ingresos de la siguinete forma 

# el vector tiene tres categorias 


# primera categoria es [-infito, 10000]
# segunda categoria es [10000, 31000]
# tercera categoria es [31000, infin]


bp <- c(-Inf, 10000, 31000, Inf)

# creamos los nombre de las categorias 


names <- c("Low", "Average", "High")


# Nueva variable dentro del dataframe que se llama income.cat
# uso la funcion cut(esta funcion corta el dataframe)
# la funcion cut utiliza los rangos definidos por el parametro breaks para inferir los trozos que intetamos catalogar 

# labels nombra los intervalos que categorizo (se convierten en un dato de la fila o registro)

students$Income.cat <- cut(students$Income, breaks = bp, labels = names)




# se puede hacer directame dentro de la funcion breaks indica las particones de los intervalos y labels lo nombres 
students$Income.cat3 <- cut(students$Income, 
                            breaks = 4, 
                           labels = c("Level 1", "Level 2", 
                                      "Level 3", "Level 4")                          )

```
 

# creando variables ficticias o ingles (dummy variables)

```{r}
#Caragamos los datos 
students2 <- read.csv("data-conversion.csv")

# Intalamos un paquete que se usa para realizar el trabajo 

#install.packages("dummies")
library(dummies)

#Convertimos en data.frame (una forma más entendible para la visulización) por medio de las variables indicadoras

students2.dummy <- dummy.data.frame(students, sep = ".")

# Sacamos los nombres de las columnas del data.frame anterior
names(students2.dummy)

#Con la función dummy sacamos una matriz indicadora
dummy(students2$State, sep=".")

# Sacamos un data.frame de students2, para el estado (anologo con departamento colombiano) y el genero:
dummy.data.frame(students2, names = c("State", "Gender"), sep = ".")
```