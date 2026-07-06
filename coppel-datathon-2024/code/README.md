#LIBRERIAS
library(ggplot2)
library(dplyr)
library(lubridate)
library(car)      
library(MASS)    
install.packages("fitdistrplus")
library(fitdistrplus)  


                              #PROCESAMIENTO DE LA DATA 
data_coppel<-read.csv("C:\\Users\\fatan\\Downloads\\data_sst_reto_coppel.csv")
data_coppel <- as.data.frame(data_coppel)

sum(is.na(data_coppel))
colSums(is.na(data_coppel))
sum(is.nan(data_coppel))
colSums(is.infinite(data_coppel))


     #Time
data_coppel$tiempo_espera <- data_coppel$hora_llamado - data_coppel$hora_llegada
data_coppel$tiempo_atencion <- data_coppel$hora_salida - data_coppel$hora_llamado

data_coppel$tiempo_espera_min <- round(data_coppel$tiempo_espera * 1440, 2)
data_coppel$tiempo_atencion_min <- round(data_coppel$tiempo_atencion * 1440, 2) #Time to minutes

    #Time_mensual_semanal(Series de tiempo)
data_coppel <- data_coppel %>%
  mutate(fecha = as.Date(Fecha, format="%Y-%m-%d")) %>%
  arrange(estado, Fecha)

data_coppel$Fecha <- as.Date(data_coppel$Fecha, format = "%Y-%m-%d")
data_coppel<- data_coppel %>%
  mutate(Mes = format(Fecha, "%m"))

data_coppel$hora_llegada_24hrs <- sapply(data_coppel$hora_llegada, function(x) {
  horas <- floor(x * 24)
  minutos <- round((x * 24 - horas) * 60)  
  paste0(sprintf("%02d", horas), ":", sprintf("%02d", minutos), ":00")
})


#---------------------------------------------------------------------------------------------------
                              #ESTADISTICA DESCRIPTIVA
     #Segmento con mayor cartera de clientes                                        
table(data_coppel$Segmento)

     #Clientes por segmento(Top 10)
clientes_por_estado <- table(data_coppel$estado)
top_10_estados <- names(head(sort(clientes_por_estado, decreasing = TRUE), 10))
data_top10 <- subset(data_coppel, estado %in% top_10_estados & !is.na(Segmento))

tabla_segmentos <- table(data_top10$estado, data_top10$Segmento)
tabla_segmentos

ggplot(data_top10, aes(x = estado, fill = Segmento)) +
  geom_bar(position = "dodge") + 
  theme_minimal() +
  labs(title = "Distribución de Segmentos en Top 10 Estados",
       x = "Estado", y = "Cantidad de Clientes") +
  scale_fill_manual(values = c("skyblue", "orange", "pink")) +
  theme(axis.text.x = element_text(angle = 45, hjust = 1)) #Grafica "Clientes por segmento(Top 10)"

     #Promedios_Espera_Atencion
          #general_by_estado
promedios_generales1 <- data_coppel %>%
  group_by(estado) %>%
  summarise(
    promedio_espera_total = mean(tiempo_espera_min, na.rm = TRUE),
    promedio_atencion_total = mean(tiempo_atencion_min, na.rm = TRUE)
  )
print(promedios_generales1)

promedios_generales2 <- data_coppel %>%
  group_by(Segmento) %>%
  summarise(
    promedio_espera_total = mean(tiempo_espera_min, na.rm = TRUE),
    promedio_atencion_total = mean(tiempo_atencion_min, na.rm = TRUE)
  )
print(promedios_generales2)

          #by_Segmento&estado
promedios_generales <- data_coppel %>%
  group_by(Segmento,estado) %>%
  summarise(
    promedio_espera_min = mean(tiempo_espera_min, na.rm = TRUE),
    promedio_atencion_min = mean(tiempo_atencion_min, na.rm = TRUE)
  )
print(promedios_generales)


     #Total de tiendas&cajas_by_estado&segmento

conteo_medios <- data_coppel %>%
  group_by(estado) %>%
  summarise(numero_tiendas = n_distinct(tienda),
            cajas = sum(grepl("^caja_", unique(caja)), na.rm = TRUE),
            ventanillas = sum(grepl("^ventanilla_", unique(caja)), na.rm = TRUE),
            p = sum(grepl("^p_", unique(caja)), na.rm = TRUE),
            total = cajas + ventanillas + p  # Suma total de todos los tipos de medio)
  )
print(conteo_medios)

length(unique(data_coppel$tienda))
length(unique(data_coppel$caja))

     #Estatus
table(data_coppel$status)

--------#SERIES DE TIEMPO
  
          #Tiempo_espera&atencion_Mensuales
          #_by_estado
data_ts <- data_coppel %>%
  mutate(mes = floor_date(Fecha, "month")) %>%
  group_by(estado, mes) %>%
  summarise(
    tiempo_espera = mean(tiempo_espera_min, na.rm = TRUE),
    tiempo_atencion = mean(tiempo_atencion_min, na.rm = TRUE)
  ) %>%
  ungroup()

ggplot(data_ts, aes(x = mes)) +
  geom_line(aes(y = tiempo_espera, color = "Tiempo Espera")) +
  geom_line(aes(y = tiempo_atencion, color = "Tiempo Atención")) +
  facet_wrap(~estado, scales = "free_y") +
  labs(title = "Evolución del Tiempo de Espera y Atención por Estado",
       x = "Fecha", y = "Minutos") +
  theme_minimal()

          #_by_segmento
data_ts_general <- data_coppel %>%
  mutate(mes = floor_date(fecha, "month")) %>%
  group_by(mes, Segmento) %>%
  summarise(
    tiempo_espera = mean(tiempo_espera_min, na.rm = TRUE),
    tiempo_atencion = mean(tiempo_atencion_min, na.rm = TRUE)
  ) %>%
  ungroup()

ggplot(data_ts_general, aes(x = mes)) +
  geom_line(aes(y = tiempo_espera, color = "Tiempo Espera"), size = 1) +
  geom_line(aes(y = tiempo_atencion, color = "Tiempo Atención"), size = 1) +
  scale_x_date(date_breaks = "1 months", date_labels = "%b %Y") +  
  labs(title = "Evolución del Tiempo de Espera y Atención por Segmento",
       x = "Fecha", y = "Minutos") +
  theme_minimal() +
  facet_wrap(~Segmento, scales = "free_y")+
  theme(axis.text.x = element_text(angle = 45, hjust = 1))

          #N#Clientes_mensuales
data_clientes_ts <- data_coppel %>%
  mutate(mes = floor_date(fecha, "month")) %>%
  group_by(mes,Segmento) %>%
  summarise(clientes_unicos = n_distinct(X, na.rm = TRUE)) %>%
  ungroup()

ggplot(data_clientes_ts, aes(x = mes, y = clientes_unicos)) +
  geom_line(color = "blue", size = 1) +
  scale_x_date(date_breaks = "1 month", date_labels = "%b %Y") +
  labs(title = "Evolución de Clientes Únicos por Mes",
       x = "Fecha", y = "Número de Clientes Únicos") +
  theme_minimal() +
  facet_wrap(~Segmento, scales = "free_y")+
  theme(axis.text.x = element_text(angle = 45, hjust = 1))  # Inclina etiquetas del eje X

          #Tiempo_estado_foco
data_ts <- data_coppel %>%
  filter(estado %in% estados_problema1) %>%  # Filtrar solo los estados problemáticos
  mutate(mes = floor_date(Fecha, "month")) %>%
  group_by(estado, mes) %>%
  summarise(
    tiempo_espera = mean(tiempo_espera_min, na.rm = TRUE),
    tiempo_atencion = mean(tiempo_atencion_min, na.rm = TRUE)
  ) %>%
  ungroup()

ggplot(data_ts, aes(x = mes)) +
  geom_line(aes(y = tiempo_espera, color = "Tiempo Espera")) +
  geom_line(aes(y = tiempo_atencion, color = "Tiempo Atención")) +
  facet_wrap(~estado, scales = "free_y") +
  labs(title = "Evolución del Tiempo de Espera y Atención por Estado",
       x = "Fecha", y = "Minutos") +
  theme(axis.text.x = element_text(angle = 60, hjust = 2))+
  theme_minimal()

  
#---------------------------------------------------------------------------------------------------                          

                              #APLICACION MATEMATICA
#--------#MUESTREO
data_sin_ausentes <- data_coppel %>% 
  filter(status == "Atendido")

total_filas <- nrow(data_sin_ausentes)
muestra_coppel <- data_sin_ausentes %>%
  group_by(Segmento) %>%
  sample_frac(size = 5000 / total_filas, replace = FALSE) %>%
  ungroup()

muestra_coppel <- muestra_coppel %>%
  mutate(tiempo_espera_min = ifelse(tiempo_espera_min == 0, 0.000001, tiempo_espera_min))


#--------#AJUSTES DISTRIBUCION 


# Graficas histogramas por segmento
ggplot(data_sin_ausentes, aes(x = tiempo_espera_min)) +
  geom_histogram(bins = 30, fill = "steelblue", color = "black") +
  facet_wrap(~Segmento, scales = "free") +
  theme_minimal()


shapiro.test(muestra_coppel$tiempo_espera_min[muestra_coppel$Segmento == "retail"])
shapiro.test(muestra_coppel$tiempo_espera_min[muestra_coppel$Segmento == "afiliacion"])
shapiro.test(muestra_coppel$tiempo_espera_min[muestra_coppel$Segmento == "banco"])  # Prueba de normalidad en cada segmento

ggplot(muestra_coppel, aes(x = tiempo_espera_min)) + 
  geom_density(fill = "steelblue", alpha = 0.5) + 
  facet_wrap(~ Segmento, scales = "free") +
  theme_minimal() #Varianza en funcion de la media 

sd(muestra_coppel$tiempo_espera_min) / mean(muestra_coppel$tiempo_espera_min) #coeficiente de variación
#Dado los resultamos optamos por una regresion Gamma


--------#REGRESION GAMMA 
muestra_coppel$Segmento <- relevel(as.factor(muestra_coppel$Segmento), ref = "retail" )
muestra_coppel$estado <- relevel(as.factor(muestra_coppel$estado), ref = "Puebla" )

modelo_gamma <- glm(tiempo_espera_min ~ Segmento + estado+ hora_llegada_24hrs + Fecha , 
              family = Gamma(link="log"), data = muestra_coppel)
summary(modelo_gamma)


#Validacion

hist(residuos, main="Histograma de Residuos", xlab="Residuos de Pearson", breaks=30) # Histograma de los residuos

plot(fitted(modelo_gamma), residuos, main="Residuos vs Valores Ajustados",
     xlab="Valores Ajustados", ylab="Residuos de Pearson") # Grafica residuos vs valores ajustados
abline(h=0, col="red")

vif(modelo_gamma) # Evaluación de Colinealidad

    
#--------IMPLEMENTACION MODELO ¿Un cambio realmente impactaria?

estados_problema <- c("estadoBaja California Sur", "estadoChiapas", "estadoZacatecas", 
                      "estadoQuerétaro", "estadoTabasco", "estadoVeracruz de Ignacio de la Llave", 
                      "estadoMichoacán de Ocampo", "estadoGuerrero", "estadoBaja California") # Estados con mayor impacto en tiempos de espera 

cat("Coeficientes actuales para los estados problemáticos:\n")
print(coeficientes[estados_problema])

coeficientes <- coef(modelo_gamma) #coeficientes del modelo

   #Reducccion de los coeficientes logarítmicos en un 35% (ajustando solo los coeficientes)
reduccion <- 0.30
coeficientes_optimizados <- coeficientes
coeficientes_optimizados[estados_problema] <- coeficientes[estados_problema] * (1 - reduccion)

cat("Coeficientes modificados (después de la optimización, en escala logarítmica):\n")
print(coeficientes_optimizados[estados_problema])

  #Prediccion tiempos de espera actuales (usando el modelo original)
tiempo_espera_actual <- predict(modelo_gamma, type = "response")

  #Modelo con los coeficientes optimizados en escala logarítmica
nuevo_modelo <- modelo_gamma
nuevo_modelo$coefficients[estados_problema] <- coeficientes_optimizados[estados_problema]

  #Predeccion tiempos de espera con nuevo modelo (usando la escala logarítmica)
tiempo_espera_nuevo <- predict(nuevo_modelo, newdata = muestra_coppel, type = "response")

 #Calculo reducción promedio en los tiempos de espera
reduccion_promedio <- mean(tiempo_espera_actual - tiempo_espera_nuevo)
porcentaje_reduccion <- mean((tiempo_espera_actual - tiempo_espera_nuevo) / tiempo_espera_actual) * 100

 #Resultados
cat("Reducción promedio en minutos:", reduccion_promedio, "\n")
cat("Porcentaje de mejora en tiempos de espera:", porcentaje_reduccion, "%\n")

  #Grafico
resultados <- data.frame(
  Estado = factor(estados_foco),
  Tiempo_espera_original = tiempo_espera_promedio,
  Tiempo_espera_proyectado = proyeccion_tiempos_espera
)

ggplot(resultados, aes(x = Estado)) +
  geom_bar(aes(y = Tiempo_espera_original), stat = "identity", fill = "red", alpha = 0.6) +
  geom_bar(aes(y = Tiempo_espera_proyectado), stat = "identity", fill = "green", alpha = 0.6) +
  labs(title = "Impacto de los Recursos en los Tiempos de Espera por Estado",
       x = "Estado", y = "Tiempo de Espera (minutos)") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))


#--------METRICAS DE APOYO
data_problema <- data_sin_ausentes %>%
  filter(estado %in% c("Baja California Sur", "Chiapas", "Zacatecas", 
                       "Querétaro", "Tabasco", "Veracruz de Ignacio de la Llave", 
                       "Michoacán de Ocampo", "Guerrero", "Baja California"))
data_problema

metricas_estado <- data_problema %>%
  group_by(estado) %>%
  summarise(
    clientes_atendidos = n(),  # Número de clientes atendidos
    tiempo_espera_promedio = mean(tiempo_espera_min, na.rm = TRUE),  # Promedio de espera
    tiempo_espera_sd = sd(tiempo_espera_min, na.rm = TRUE),  # Variabilidad en espera
    tiempo_atencion_promedio = mean(tiempo_atencion_min, na.rm = TRUE)  # Promedio de atención
  ) %>%
  arrange(desc(tiempo_espera_promedio))  # Ordenar por mayor tiempo de espera

print(metricas_estado)


# Clasificación de estados problemáticos
clasificacion <- metricas_estado %>%
  mutate(
    problema_principal = case_when(
      clientes_atendidos < quantile(clientes_atendidos, 0.25) ~ "Falta de recursos",
      tiempo_espera_promedio > quantile(tiempo_espera_promedio, 0.75) ~ "Alta variabilidad en los tiempos de espera",
      tiempo_atencion_promedio > quantile(tiempo_atencion_promedio, 0.75) ~ "Mayor carga operativa",
      tiempo_espera_sd > quantile(tiempo_espera_sd, 0.75) ~ "Deficiencia en el personal",
      TRUE ~ "Sin problema crítico"
    )
  )
print(clasificacion)

