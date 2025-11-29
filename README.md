**Supuesto para el análisis:**

Se analizan un dataset con los datos de 7500 tipos de vino tinto de España recolectados usando web scraping de diferentes fuentes (desde páginas de vinos especializadas hasta supermercados).

*Fuente del dataset*: https://www.kaggle.com/datasets/fedesoriano/spanish-wine-quality-dataset

**Problemas a resolver**

- Factores que influyen más en la percepción de calidad y precio por parte de los clientes
- Identificar vinos con baja valoración pero alto precio (evitar compras poco rentables)
- Segmentar clientes según preferencias y hábitos de compra para campañas de marketing

**Pasos seguidos en la limpieza de datos**

1. Se ha cargado el dataset wines_SPA.csv
2. Se ha sacado la información necesaria para comenzar con la limpieza de duplicados y nulos del dataset
3. He eliminado 5452 filas duplicadas, lo que ha dejado un total de 2048 filas restantes
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
15. Guardo el nuevo dataset cómo wines_SPA_cleaned.csv

**Visualización de datos**

