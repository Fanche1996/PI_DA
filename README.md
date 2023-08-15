# <h1 align=center> **PROYECTO INDIVIDUAL Nº2** </h1>

# <h1 align=center>**`Cryptocurrency Market Data Analytics`**</h1>


## Seleccion de Cryptomonedas

Obtube la informacion en CoinGecko via el get /coins/markets. Y para seleccionar las 10 Cryptos que iba a tener que analisar durante todo el trabajo procedi de la siguiente manera:

1. Elimine las StableCoins. Utilice la desviacion estandar sobre los precios y aquellos que tubieron una volatilidad muy pero muy baja los descarte. Me parecio que la "StableCoins" no iban a ser relevantes para el trabajo.
2. Elimine los outliers de volatilidad alta. Queriendo mitigar riesgos.
3. Reduje el dataframe a las 30 monedas con mayor capitalizacion de mercado. Esto tambien aporta robustez a la seleccion.
4. Obtube los datos de variacion historica del precio del utlimo año para estas 30.
5. Seleccione las cinco monedas con mayor capitalizacion de mercado. **(5 SELECTAS)**
6. De las restantes tome las tres con mejor desempeño en el ultimo año. **(3 SELECTAS)**
7. Para finalizar busque las monedas "trending" con el get /search/trending. Sobre estas, tome las dos con mayor capitalizacion de mercado. **(2 SELECTAS)**

Con eso di por finalizada la seleccion de cryptos y cree el csv cryptos_elejidas.csv. Puede verse el desarrollo en EleccionDeCryptos.ipynb

## EDA

1. Recopile los datos historicos de precio, capitalizacion de mercado y volumen de transacciones de cada cryptomoneda elejida de los ultimos 5 años. Los datos de cierre de cada dia.
2. Busques los datos de variacion porcentual de precio que tuvo cada dia con respecto al anterior
3. Añadi la tendencia del precio de cada crypto. Para eso utilizare la media movil de 15 dias (Sin tener en cuenta el dato propio de la fecha para no generar tendencias engañozas). 
4. Tambien obtube la relacion precio / MA15, para saber en que posicion se encuentra el precio con la tendencia. 
4. Añadiremos el volumen dividido la capitalizacion de mercado, para saber la liquidez relativa de cada moneda.
5. Por ultimo, calcule la desviacion estandar de los porcentajes de cambio de precio, para medir la volatilidad.


- Mediante histogramas (seperados por cryptomonedas) observe los datos recopilados.
- Con un heatmap de observe la correlacion entre todas las variables elejidas para encontrar tendencias que nos pueden ser utiles.

Puede verse el desarrollo en EDA.ipynb

## Analisis

En el EDA pude observar tres datos que me interesaron, mediante el heatmap encontre pequeñas correlaciones:

1. Existe cierta correlacion entre la relacion precio/tendencia y porcentaje (Variacion diaria del precio). Las tendencias parece que continuan, y a mayor tendencia positiva, mas chances de encontrarnos con una variacion positiva en el precio.
2. Cuando la liquidez relativa medida por Volume/market_cap es alta, nos encontramos con precios mas bajos.
3. A mayor volatilidad, medida por std_porcentaje, porcentaje (Variacion diaria del precio) mas alto.

## Operacion

Teniendo en cuenta los datos del analisis nos aventuramos realizar operaciones de compra y venta de cryptos (con un backtesting) con las siguientes consignas.

- Backtesting a partir del 01/01/2023
- Price_to_MA15 pase de menor que 1 a mayor que 1 (Con el requerimiento de venir 5 dias negativos seguidos)
- Volume_to_market_cap este dentro del 0.75 mas alto de cada crypto individualmente "o" - std_porcentaje debe ser este dentro del 0.25 mas bajo de cada crypto individualmente.

  Deben cumplirse las primeras dos condiciones y alguna de las ultimas dos. En ese caso, se realiza una operacion de compra de 1000 usd que se terminara por vender a los 5 dias.

## Dashboard

El dashboard esta compuesto por tres partes principales:

1) Visualizacion de los datos.

Tenemos dos filtrados principales que van a condicionar todo el dashboard. Eleccion de cryptos que puede ser una o multiples. Y eleccion de datos por fechas. (Entre una fecha y otra de los ultimos 5 años)

- A partir de eso podemos observar un grafico en el cual podemos elejir si queremos ver el precio, el market cap o la media movil.
- Luego podemos observar las 3 metricas principales nos interesaron en el EDA: "Volatilidad", "Liquidez" y Media Movil Sobre Precio.

2) Operaciones.

La segunda parte del dashboard podemos observar las operaciones de compra que se hicieron en base a las consignas que dijimos anteriormente (Se filtran por los filtos principales).
Podemos observar en una tabla, la fecha, la moneda a que precio se opero y que cantidad se compro y luego vendio. 
Adosados al grafico observamos algunas metricas: 
- Total Invertido (que es fijo en 1000 usd)
- Fecha de inicio de inversiones y fin de inversiones
- Cantidad de transacciones
- Resultado de las transacciones

3) ## KPIs

Los KPis elejidos son sobre los resultados de las operaciones de compra/venta realizadas. 

- Tasa de retorno anual. (Se espera una tasa de mas de 75%) Mide el exito del negocio en terminos monetarios.
- Porcentaje de Operaciones Positivas (Se espera mas de 60%) Mide La cantidad de operaciones positivas, Si este numero aumenta, disminuye el factor "Suerte".
- Resultado sin Outliers / Resultado Con Outliers (Se espera al menos 100%, es decir el mismo resultado) Queremos verificar con esto que unas pocas transacciones fructiferas no esten viciando todo el modelo.


## Analisis final

Considero en lo personal que hay muchisimos factores que entran en juego para determinar el valor de un activo, y aunque hagmamos un trabajo mucho mas exahustivo no vamos a poder conseguir datos muy contundentes para la prediccion del precio y la automatizacion de la compra/venta. Dicho esto, tambien podriamos decir, que algunos de los datos observados, pueden servir de referencia para realizar este tipo de transacciones.
