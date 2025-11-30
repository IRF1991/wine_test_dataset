**Supuesto para el análisis:**

Se analizan un dataset con los datos de 7500 tipos de vino tinto de España recolectados usando web scraping de diferentes fuentes (desde páginas de vinos especializadas hasta supermercados).

El dataset inicial se compone de 11 columnas que muestran el lugar de producción, nombre del vino, año de la cosecha, valoración media de los clientes, número de reviews, país, región de producción, precio, tipo de vino, cuerpo y acidez.

*Fuente del dataset*: https://www.kaggle.com/datasets/fedesoriano/spanish-wine-quality-dataset

**Problemas a resolver**

- Resolver la calidad/precio para cada vino
- Averiguar qué factores influyen más en la percepción de calidad y precio por parte de los clientes
- Ver la distribución de precios en España
- Identificar las regiones de España con mejor valoración de vinos

**Pasos seguidos en la limpieza de datos**

1. Se ha cargado el dataset wines_SPA.csv

2. Se ha sacado la información necesaria para comenzar con la limpieza de duplicados y nulos del dataset, borro la columna "country", ya que el único valor es España y es una columna redundante.

3. Elimino 5452 filas duplicadas, lo que ha dejado un total de 2048 filas restantes

4. Reviso los valores únicos de las columnas numéricas para comprobar outliers y valores redundantes

5. Veo que en la columna "year" hay valores de N.V. lo que implica mezcla de añadas, así que decido convertirlo a False para facilitar la conversión de la columna a numérica más adelante.

6. Descubro una relación entre las columnas "type", "body" y "acidity" qué coinciden a lo largo del dataset, donde en todos los type nulos que quedan, también fatan body y acidity, y compruebo si todos los type tienen el mismo body y acidity, lo cuál es verídico. Por tanto utilizo la columna type para rellenar una gran parte de los nulos restantes de body y acidity.

7. Decido borrar la columna "type", ya que mezcla los tipos de vino por uva, denominación de origen y estilo. Usaré la columna "region" cómo referencia para el tipo de vino por denominación de origen.

8. Vuelvo a comprobar el mismo patrón que había con "type" y las columnas "body" y "acidity", y utilizo la columna "region" para rellenar otra parte de esos datos faltantes.

9. Mantengo los valores nulos restantes de las columnas "body" y "acidity", ya que si elimino las filas podría afectar al posterior cálculo de la calidad/precio de los vinos, y al haber únicamente 23 valores nulos restantes tampoco afectaría al resultado final.

10. Convierto la columna "year" a numérica (tipo int64) y mantengo los nulos.

11. Normalizo la columna "rating", creando una nueva columna llamada "rating_norm", esto lo hago ya que necesito calcular la calidad/precio de los vinos y los valores de rating que tengo varían muy poco, van desde 4.2 hasta 4.9, lo cuál puede dificultar el análisis, así que usaría la columna "rating_norm" que va de 0 a 1 para calcular el percentil del rating y el porcentaje final de la calidad/precio.

12. Cómo el precio de los vinos varía desde 3119 hasta 5 euros, la calidad precio da resultados muy incongruentes, así que decido dividir los precios de los vinos en una nueva columna que será "price_group", estos  precios serán:
    - high: mayor o igual de 1000 euros
    - mid: entre 1000 y 50 euros
    - low: menor o igual de 50 euros

13. Creo la columna "quality_price_percent", para ello lo que hago es utilizar la función interna *rank* de pandas para sacar el percentil de "price" y "rating_norm" agrupándolos por "price_group", y con estos percentiles calculo una fórmula viable y fiable para sacar un porcentaje de la calidad/precio de cada vino.

14. Ordeno las columnas para mejor visualización.

15. Guardo el nuevo dataset cómo wines_SPA_cleaned.csv.

## Conclusiones resultantes de las visualizaciones:

- La gran mayoría de los vinos producidos en España provienen de Ribera del Duero y La Rioja.

- El grueso de la calidad/precio de los vinos se concentra entre un 45 y un 55% de satisfacción (calidad/precio).

- La mayor parte de la oferta del mercado español se concentra en rangos de precio asequibles o de un precio moderado, con la mediana situada cerca de los vinos con precios bajos (de 0 a 50 euros). Los consumidores buscan unos precios asequibles, lo que sugiere que las estrategias comerciales deben priorizar ofertar vinos dentro de esos rangos.

- Se pueden apreciar las zonas de dispersión en los segmentos más altos, lo que puede permitir saber qué precios oscilan los vinos más premium del mercado para tenerlo en cuenta a la hora de tener una reserva de vinos de alta gama.

- En una lectura general de todas las regiones se puede deducir que los vinos con mayor calidad/precio por lo general son vinos con un precio bajo, lo que indica que el consumidor español valora especialmente el equilibrio entre un bajo coste y la satisfacción, además, al filtrar por regiones con más de 10 vinos, se sigue observando la preferencia por vinos más económicos.

- A nivel comercial, se podrían trazar estrategias de pedido a los vinos con mayor calidad/precio dentro de las regiones más valoradas o con más producción, qué tengan rangos de precios que se adecuén a lo que necesitamos, para tener más oportunidad de mercado a la vez que se pueden ahorrar costos en transporte. Esto nos ayuda a mejorar la rentabilidad y la competitivdad.

- Al ver la distribución de los factores que influyen en la percepción de la calidad/precio de los usuarios, es destacable comentar que los vinos dentro de un rango de 0 a 100 euros tienen una percepción superior a la media, lo que refuerza aún más el hecho de centrarse comercialmente en vinos de coste bajo y medio-bajo, además hay una preferencia sobre los vinos de los años de 2020 a 2024 y los vinos con un cuerpo y acidez de 2, qué es lo mínimo que tienen los vinos del dataset.

- Todos estos datos nos pueden llevar a encontrar los vinos con más preferencia en el mercado y por tanto nos aportan un valor enorme a la hora tanto de hacer marketing, cómo de ofrecer las vinos con más calidad, más competitivos y rentables.