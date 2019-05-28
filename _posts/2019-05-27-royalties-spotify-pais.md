---
title: Cómo son los royalties que paga Spotify en cada país?
author: manuel
layout: post
permalink: /2019/05/27/royalties-spotify-pais
categories:
  - Uncategorized
---

Lo siguiente es un ejercicio de análisis de un dataset de reproducciones emitido por el servicio [Distrokid](http://distrokid.com), para un artista argentino con 2.5 millones de reproducciones en los últimos 9 meses, en el servicio de streaming [Spotify](http://spotify.com).



```python
import pandas as pd
import altair as alt
import numpy as np

```


```python
data = pd.read_csv('DistroKid_1558988581759_SoyRadaandtheColibriquis.tsv', sep='\t', encoding='iso-8859-1')
sales = data[(data.Artist == 'Soy Rada and the Colibriquis') & (data.Store == 'Spotify')]
sales.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Reporting Date</th>
      <th>Sale Month</th>
      <th>Store</th>
      <th>Artist</th>
      <th>Title</th>
      <th>ISRC</th>
      <th>UPC</th>
      <th>Quantity</th>
      <th>Team Percentage</th>
      <th>Song/Album</th>
      <th>Customer Paid</th>
      <th>Customer Currency</th>
      <th>Country of Sale</th>
      <th>Songwriter Royalties Withheld</th>
      <th>Earnings (USD)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>653</th>
      <td>2018-09-22</td>
      <td>2018-07</td>
      <td>Spotify</td>
      <td>Soy Rada and the Colibriquis</td>
      <td>Avisame Cuando Llegues</td>
      <td>QZDA61809768</td>
      <td>NaN</td>
      <td>20</td>
      <td>100</td>
      <td>Song</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>IL</td>
      <td>0</td>
      <td>0.077258</td>
    </tr>
    <tr>
      <th>654</th>
      <td>2018-09-22</td>
      <td>2018-07</td>
      <td>Spotify</td>
      <td>Soy Rada and the Colibriquis</td>
      <td>Avisame Cuando Llegues</td>
      <td>QZDA61809768</td>
      <td>NaN</td>
      <td>20</td>
      <td>100</td>
      <td>Song</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>PT</td>
      <td>0</td>
      <td>0.078115</td>
    </tr>
    <tr>
      <th>655</th>
      <td>2018-09-22</td>
      <td>2018-07</td>
      <td>Spotify</td>
      <td>Soy Rada and the Colibriquis</td>
      <td>Avisame Cuando Llegues</td>
      <td>QZDA61809768</td>
      <td>NaN</td>
      <td>2226</td>
      <td>100</td>
      <td>Song</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>AR</td>
      <td>0</td>
      <td>1.078313</td>
    </tr>
    <tr>
      <th>656</th>
      <td>2018-09-22</td>
      <td>2018-07</td>
      <td>Spotify</td>
      <td>Soy Rada and the Colibriquis</td>
      <td>Avisame Cuando Llegues</td>
      <td>QZDA61809768</td>
      <td>NaN</td>
      <td>91</td>
      <td>100</td>
      <td>Song</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>IT</td>
      <td>0</td>
      <td>0.040131</td>
    </tr>
    <tr>
      <th>657</th>
      <td>2018-09-22</td>
      <td>2018-07</td>
      <td>Spotify</td>
      <td>Soy Rada and the Colibriquis</td>
      <td>Avisame Cuando Llegues</td>
      <td>QZDA61809768</td>
      <td>NaN</td>
      <td>11</td>
      <td>100</td>
      <td>Song</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>MX</td>
      <td>0</td>
      <td>0.027201</td>
    </tr>
  </tbody>
</table>
</div>



Las columnas que nos interesan son:

  - `Store`: sólo vamos a considerar _Spotify_
  - `Quantity`: la cantidad de streams que se reportan en esta fila
  - `Country of Sale`: El país en el que fue escuchado el stream
  - `Earnings (USD)`: lo pagado para la cantidad de streams reportada en esta fila. Asumimos que el pago por un stream es `Earnings (USD)`/`Quantity`

## Ingreso mensual


```python
sales_per_month=sales.groupby(['Sale Month'])[['Quantity', 'Earnings (USD)']].sum().reset_index()

alt.Chart(sales_per_month).mark_bar().encode(
    x='Sale Month:O',
    y='Earnings (USD):Q',
    tooltip='Quantity:Q',
).properties(width=500, title="Ingreso (USD) por mes")\
 .configure(background='#FFFFFF')
```




![png](/wp-content/uploads/2019/05/output_5_0.png)



Los meses de aumento en el ingreso corresponden a lanzamientos.

## ¿En qué países está la audiencia?

Por supuesto, nos interesa saber a dónde está la mayor cantidad de escuchas de nuestros tracks


```python
by_country = sales.groupby('Country of Sale').sum().reset_index()
top_10_countries = by_country.sort_values('Quantity', ascending=False).head(10).reset_index()
top_10_countries[['Country of Sale', 'Quantity']]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country of Sale</th>
      <th>Quantity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>AR</td>
      <td>1917972</td>
    </tr>
    <tr>
      <th>1</th>
      <td>CL</td>
      <td>199124</td>
    </tr>
    <tr>
      <th>2</th>
      <td>UY</td>
      <td>98114</td>
    </tr>
    <tr>
      <th>3</th>
      <td>US</td>
      <td>37505</td>
    </tr>
    <tr>
      <th>4</th>
      <td>PE</td>
      <td>31865</td>
    </tr>
    <tr>
      <th>5</th>
      <td>MX</td>
      <td>30346</td>
    </tr>
    <tr>
      <th>6</th>
      <td>CO</td>
      <td>14393</td>
    </tr>
    <tr>
      <th>7</th>
      <td>ES</td>
      <td>12948</td>
    </tr>
    <tr>
      <th>8</th>
      <td>PY</td>
      <td>10372</td>
    </tr>
    <tr>
      <th>9</th>
      <td>EC</td>
      <td>9856</td>
    </tr>
  </tbody>
</table>
</div>



Vemos que Argentina es el país principal, muy por encima del resto de los países: un orden de magnitud superior al segundo puesto. Graficamos esta distribución en un gráfico con escala logarítmica en el eje vertical.

(Ya que estamos,agregamos las banderas de los países)


```python
OFFSET = ord('🇦') - ord('A')
def flag(code):
    """ Retorna el emoji de la bandera de un país, dado su código ISO-3166 alpha-2"""
    return chr(ord(code[0]) + OFFSET) + chr(ord(code[1]) + OFFSET)

top_quantity = by_country.sort_values('Quantity', ascending=False).reset_index()
top_quantity.loc[:, 'country_name_and_flag'] = top_quantity['Country of Sale'].apply(lambda c: flag(c) + c)
```


```python
alt.Chart(top_quantity.sort_values('Quantity', ascending=False)).mark_bar().encode(
    x=alt.X('country_name_and_flag:N', sort=list(top_quantity['country_name_and_flag'])),
    y=alt.Y('Quantity:Q',scale=alt.Scale(type='log')),
).properties(
    title='Number of streams per country (log scale)',    
)\
 .configure(background='#FFFFFF')
```




![png](/wp-content/uploads/2019/05/output_11_0.png)



## Promedio de pago _por stream_ en cada país

Intentaremos, de manera muy poco rigurosa, estudiar la relación entre el importe promedio que Spotify paga al artista por cada reproducción, y el costo de la suscripción mensual en cada país. 

Comenzamos calculando el valor promedio de cada reproducción (_stream_)


```python
metrics = by_country[['Country of Sale', 'Quantity', 'Earnings (USD)']].sort_values('Quantity', ascending=False)
metrics = metrics.assign(per_stream_avg=metrics['Earnings (USD)']/metrics['Quantity'])
metrics[['Country of Sale', 'Quantity', 'per_stream_avg']].head(10)

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country of Sale</th>
      <th>Quantity</th>
      <th>per_stream_avg</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>AR</td>
      <td>1917972</td>
      <td>0.000965</td>
    </tr>
    <tr>
      <th>11</th>
      <td>CL</td>
      <td>199124</td>
      <td>0.001405</td>
    </tr>
    <tr>
      <th>64</th>
      <td>UY</td>
      <td>98114</td>
      <td>0.002186</td>
    </tr>
    <tr>
      <th>63</th>
      <td>US</td>
      <td>37505</td>
      <td>0.002802</td>
    </tr>
    <tr>
      <th>50</th>
      <td>PE</td>
      <td>31865</td>
      <td>0.001511</td>
    </tr>
    <tr>
      <th>43</th>
      <td>MX</td>
      <td>30346</td>
      <td>0.001261</td>
    </tr>
    <tr>
      <th>12</th>
      <td>CO</td>
      <td>14393</td>
      <td>0.001187</td>
    </tr>
    <tr>
      <th>22</th>
      <td>ES</td>
      <td>12948</td>
      <td>0.002075</td>
    </tr>
    <tr>
      <th>54</th>
      <td>PY</td>
      <td>10372</td>
      <td>0.001203</td>
    </tr>
    <tr>
      <th>19</th>
      <td>EC</td>
      <td>9856</td>
      <td>0.001744</td>
    </tr>
  </tbody>
</table>
</div>



Graficamos el promedio por reproducción para cada país


```python
top_per_stream_avg = metrics.sort_values('per_stream_avg', ascending=False)
top_per_stream_avg.loc[:, 'country_name_and_flag'] = top_per_stream_avg['Country of Sale'].apply(lambda c: flag(c) + c)

alt.Chart(top_per_stream_avg).mark_bar().encode(
    x=alt.X('country_name_and_flag:N', type='nominal', sort=list(top_per_stream_avg['country_name_and_flag'])),
    y='per_stream_avg:Q',
    tooltip=['per_stream_avg:Q']
).properties(
    title='Per-stream payout average by Country\n(from a Spotify royalties report with %.2fM plays)' % (metrics.Quantity.sum() / 1e6)
)\
 .configure(background='#FFFFFF')
```




![png](/wp-content/uploads/2019/05/output_15_0.png)



Suiza (CH) es el país que "mejor" paga por cada reproducción ($0.005 USD). Vemos también que los países en el tope del ranking pertecen al hemisferio norte. Gana peso nuestra hipótesis de que el _per-stream average payout_ está relacionado con el costo de la suscripción al servicio Spotify

## Costo mensual de Spotify en cada país

Necesitamos, por supuesto, una base de datos que contenga el costo de la suscripción a Spotify en cada país. Pensé en _scrapearlo_, pero por suerte un señor danés llamado Matias Singer construyó un [Spotify International Pricing Index](http://mts.io/2014/05/07/spotify-pricing-index/) que contiene la información que necesitamos.

Los datos en la versión publicada están muy desactualizados, pero el buen Matias publicó el [código fuente del scraper](https://github.com/matiassingers/spotify-pricing). Luego de unas modificaciones triviales, ejecuté ese código y obtuve la data que necesitaba.


```python
spotify_monthly = pd.read_json('./spotify-pricing.jsonlines', lines=True)
spotify_monthly = spotify_monthly[spotify_monthly.convertedPrice > 0]
spotify_monthly.loc[:, 'country_upper']  = spotify_monthly.rel.str.upper()
spotify_monthly.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>convertedPrice</th>
      <th>countryCode</th>
      <th>currency</th>
      <th>demonym</th>
      <th>internationalName</th>
      <th>link</th>
      <th>originalCurrency</th>
      <th>originalPrice</th>
      <th>originalRel</th>
      <th>price</th>
      <th>region</th>
      <th>rel</th>
      <th>subRegion</th>
      <th>title</th>
      <th>country_upper</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>4.990000</td>
      <td>DZA</td>
      <td>DZD</td>
      <td>Algerian</td>
      <td>Algeria</td>
      <td>/dz-fr/premium/?checkout=false</td>
      <td>DZD</td>
      <td>USD 4.99/mois</td>
      <td>dz-fr</td>
      <td>4.99</td>
      <td>Africa</td>
      <td>dz</td>
      <td>Northern Africa</td>
      <td>Algérie (français)</td>
      <td>DZ</td>
    </tr>
    <tr>
      <th>1</th>
      <td>7.506266</td>
      <td>CAN</td>
      <td>CAD</td>
      <td>Canadian</td>
      <td>Canada</td>
      <td>/ca-fr/premium/?checkout=false</td>
      <td>CAD</td>
      <td>9,99 $ CAD/mois</td>
      <td>ca-fr</td>
      <td>9.99</td>
      <td>Americas</td>
      <td>ca</td>
      <td>Northern America</td>
      <td>Canada (français)</td>
      <td>CA</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5.990000</td>
      <td>GTM</td>
      <td>GTQ</td>
      <td>Guatemalan</td>
      <td>Guatemala</td>
      <td>/gt/premium/?checkout=false</td>
      <td>GTQ</td>
      <td>5.99 USD al mes</td>
      <td>gt</td>
      <td>5.99</td>
      <td>Americas</td>
      <td>gt</td>
      <td>Central America</td>
      <td>Guatemala</td>
      <td>GT</td>
    </tr>
    <tr>
      <th>3</th>
      <td>5.990000</td>
      <td>ECU</td>
      <td>USD</td>
      <td>Ecuadorean</td>
      <td>Ecuador</td>
      <td>/ec/premium/?checkout=false</td>
      <td>USD</td>
      <td>5.99 USD al mes</td>
      <td>ec</td>
      <td>5.99</td>
      <td>Americas</td>
      <td>ec</td>
      <td>South America</td>
      <td>Ecuador</td>
      <td>EC</td>
    </tr>
    <tr>
      <th>4</th>
      <td>7.506266</td>
      <td>CAN</td>
      <td>CAD</td>
      <td>Canadian</td>
      <td>Canada</td>
      <td>/ca-en/premium/?checkout=false</td>
      <td>CAD</td>
      <td>$9.99 CAD / month</td>
      <td>ca-en</td>
      <td>9.99</td>
      <td>Americas</td>
      <td>ca</td>
      <td>Northern America</td>
      <td>Canada (English)</td>
      <td>CA</td>
    </tr>
  </tbody>
</table>
</div>



Vamos a visualizar en un [scatterplot](https://en.wikipedia.org/wiki/Scatter_plot) las dos variables que nos interesan, valor promedio por reproducción y costo mensual del servicio.

El eje horizontal codificará el _per-stream average payout_, mientras que el vertical el costo mensual de la suscripción. El color de cada punto corresponde a la región del país, mientras que el tamaño codifica la cantidad de reproducciones.

Combinamos (`merge`) nuestro dataset de pago promedio por reproducción con el dataset obtenido por el scrapper, y construímos el _scatterplot_.


```python
stream_avg_monthly_scatter = top_per_stream_avg.merge(spotify_monthly, left_on='Country of Sale', right_on='country_upper')
stream_avg_monthly_scatter.loc[:, 'convertedPriceFit'] = pd.Series(np.poly1d(fit)(stream_avg_monthly_scatter.per_stream_avg))

scatter = alt.Chart(stream_avg_monthly_scatter).mark_circle().encode(
    x='per_stream_avg:Q',
    y='convertedPrice:Q',
    tooltip=['internationalName:N', 'convertedPrice:Q', 'per_stream_avg:Q', 'Quantity:Q'],
    color='region:N',
    size=alt.Size('Quantity:Q', scale=alt.Scale(range=(50,1000))),
).properties(
title="Per-stream AVG payout VS Spotify monthly price by country (R²=%.2f)" % fit_rsq
)\
 .configure(background='#FFFFFF')
scatter
```




![png](/wp-content/uploads/2019/05/output_21_0.png)



Vemos un atisbo de correlación lineal entre las dos variables. También vemos agrupaciones "horizontales", que corresponden a importes como $5.99 ([psychological pricing](https://en.wikipedia.org/wiki/Psychological_pricing)). Calculamos un _fit_ lineal y también lo graficamos. También vamos a agregar líneas horizontales para enfatizar los países que comparten importes.


```python
fit = np.polyfit(stream_avg_monthly_scatter.per_stream_avg, stream_avg_monthly_scatter.convertedPrice, 1)

fit_rsq = np.corrcoef(stream_avg_monthly_scatter.per_stream_avg, stream_avg_monthly_scatter.convertedPrice)[0,1]**2
scatter = alt.Chart(stream_avg_monthly_scatter).mark_circle().encode(
    x='per_stream_avg:Q',
    y='convertedPrice:Q',
    tooltip=['internationalName:N', 'convertedPrice:Q', 'per_stream_avg:Q', 'Quantity:Q'],
    color='region:N',
    size=alt.Size('Quantity:Q', scale=alt.Scale(range=(50,1000))),
).properties(
title="Per-stream AVG payout VS Spotify monthly price by country (R²=%.2f)" % fit_rsq
)

linear_fit = alt.Chart(stream_avg_monthly_scatter).mark_line().encode(
    x='per_stream_avg:Q',
    y='convertedPriceFit:Q'
)


# add fixed price points
fixed_price_points = pd.DataFrame(
    [
        {'text': '9.99€', 'rule': 11.20},
        {'text': '6.99€', 'rule': 7.83},
        {'text': '$5.99', 'rule': 5.99}
    ]
)

fixed_price_rules = alt.Chart(fixed_price_points).mark_rule(strokeDash=[1,1]).encode(
    y='rule:Q',
)

fixed_price_text = alt.Chart(fixed_price_points).mark_text(align='left', dy=-5, dx=5).encode(
    text='text:N',
    y='rule:Q',
    x=alt.X(value=0)
)

(scatter + linear_fit + fixed_price_rules + fixed_price_text).configure(background='#FFFFFF')

```




![png](/wp-content/uploads/2019/05/output_23_0.png)


## Qué averiguamos?

El resultado de este ejercicio informal y superficial sugiere que hay una relación entre el costo de la suscripción mensual al servicio Spotify, y lo que pagan a los artistas en concepto de _royalties_. También, el dato curioso de que Argentina es el país con el costo de suscripción más barato del mundo. 

Si la relación que investigamos existe en efecto, podemos decir que pegar un _hit_ en Argentina es un mal negocio en comparación al resto de los países.
