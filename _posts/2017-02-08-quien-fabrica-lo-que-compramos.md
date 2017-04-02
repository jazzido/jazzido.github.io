---
title: ¿Quién fabrica lo que compramos?
author: manuel
layout: post
permalink: /2017/02/08/quien-fabrica-lo-que-compramos
categories:
  - Uncategorized
---

<style type="text/css">
table.dataframe {
    margin-left: auto;
    margin-right: auto;
    border: 1px solid black;
    border-collapse: collapse;
}

table.dataframe tr,
table.dataframe th,
table.dataframe td
{
border: 1px solid black;
border-collapse: collapse;
margin: 1em 2em;
}
</style>

Los códigos de barras de los productos que compramos contienen información sobre la empresa que los distribuye, empaca o fabrica. En este breve ejercicio, vamos a intentar combinar una lista de productos tomada del sitio público [Precios Claros](https://www.preciosclaros.gob.ar/) con una lista de compañías compilada por el proyecto [Product Open Data](http://www.product-open-data.com/).

Los identificadores de productos en _preciosclaros.gob.ar_ son, en la mayoría de los casos, el código de barras del producto. Para extraer el campo [GCP (Global Company Prefix)](http://www.gs1.org/company-prefix) del identificador, vamos a usar la librería [`gtin`](https://pypi.python.org/pypi/gtin/0.1.4).


```python
import pandas as pd
from gtin import GTIN
```

Leemos la lista de productos, y agradecemos a nuestro anónimo amigo que se tomó el trabajo de _scrapear_ Precios Claros.


```python
df = pd.read_csv('productos_precios_claros.csv')
df.loc[:, 'uuid'] = df.uuid.apply(lambda s: s.split('producto-')[1])
```

Agregamos una columna que contendrá, si es posible, el campo GCP.


```python
def gg(gtin):
    try:
        g = GTIN(gtin)
    except:
        return None

    try:
        return g.get_gcp()
    except:
        return None

df.loc[:, 'gcp'] = df.uuid.apply(lambda u: gg(u))
```

Calculamos una agregación de la tabla anterior, calculando dos variables adicionales:

  - `marca`: un conjunto de marcas asociadas a un `GCP`
  - `product_count`: cantidad de productos asociados a un `GCP`


```python
products_by_gcp = df \
                   .groupby(['gcp'], as_index=False) \
                   .agg({
                       'marca': lambda m: set(m),
                       'uuid': 'count'
                    }) \
                   .rename(columns={'uuid': 'product_count'}) \
                   .sort_values(by='product_count', ascending=False)
```

Leemos la base de datos de GCPs obtenida de [Product Open Data](http://product-open-data.com). Lamentablemente, la última versión es de 2013 y su cobertura para fabricantes argentinos es bastante mala.


```python
gepir = pd.read_csv('/Users/manuel/Downloads/gs1_gcp.csv',
                    dtype={'GCP_CD': str, 'GLN_CD': str},
                    low_memory=False)
```

…y la combinamos con nuestra lista de GCPs (`products_by_gcp`).


```python
merged = products_by_gcp.merge(gepir, how='inner', left_on='gcp', right_on='GCP_CD')
```

Verificamos qué porcentaje de GCPs pudimos encontrar en la base de datos:


```python
n_matched_gcps = len(merged[~merged.GLN_NM.isnull()])
total_gcps = len(products_by_gcp)
print((n_matched_gcps / total_gcps) * 100)
```

    66.34615384615384


Filtramos la lista para mostrar solamente las compañías que pudimos encontrar


```python
out = merged[~merged.GLN_NM.isnull()][['gcp', 'product_count', 'marca', 'GLN_NM', 'GLN_COUNTRY_ISO_CD']]
```

Le agregamos la bandera del país para que quede más cheto.


```python
OFFSET = ord('🇦') - ord('A')
def flag(code):
    return chr(ord(code[0]) + OFFSET) + chr(ord(code[1]) + OFFSET)

out.loc[:, 'country_flag'] = out['GLN_COUNTRY_ISO_CD'].apply(flag)
```


```python
pd.set_option('display.max_rows', len(out))
out
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gcp</th>
      <th>product_count</th>
      <th>marca</th>
      <th>GLN_NM</th>
      <th>GLN_COUNTRY_ISO_CD</th>
      <th>country_flag</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>8480017</td>
      <td>1009</td>
      <td>{GROLS, CUQUE, NAN, CARO AMICI, S.OND, CROCK, ...</td>
      <td>DIA</td>
      <td>ES</td>
      <td>🇪🇸</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4005808</td>
      <td>29</td>
      <td>{NIVEA}</td>
      <td>Beiersdorf AG</td>
      <td>DE</td>
      <td>🇩🇪</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0070330</td>
      <td>18</td>
      <td>{BIC , BIC}</td>
      <td>BIC USA Inc.</td>
      <td>US</td>
      <td>🇺🇸</td>
    </tr>
    <tr>
      <th>5</th>
      <td>4052899</td>
      <td>10</td>
      <td>{OSRAM, DULUX}</td>
      <td>OSRAM GmbH CRM&amp;S MDS P-W</td>
      <td>DE</td>
      <td>🇩🇪</td>
    </tr>
    <tr>
      <th>6</th>
      <td>301426</td>
      <td>10</td>
      <td>{ZOOTH, GILLETTE, ORAL B}</td>
      <td>PROCTER ET GAMBLE FRANCE SAS</td>
      <td>FR</td>
      <td>🇫🇷</td>
    </tr>
    <tr>
      <th>9</th>
      <td>75010074</td>
      <td>7</td>
      <td>{KOLESTON, ALWAYS, PANTENE}</td>
      <td>CORPORATIVO PROCTER &amp; GAMBLE, S. DE R.L. DE C....</td>
      <td>MX</td>
      <td>🇲🇽</td>
    </tr>
    <tr>
      <th>10</th>
      <td>4008321</td>
      <td>7</td>
      <td>{OSRAM, DULUX}</td>
      <td>OSRAM GmbH</td>
      <td>DE</td>
      <td>🇩🇪</td>
    </tr>
    <tr>
      <th>12</th>
      <td>75010067</td>
      <td>6</td>
      <td>{PAMPERS, PANTENE}</td>
      <td>CORPORATIVO PROCTER &amp; GAMBLE, S. DE R.L. DE C....</td>
      <td>MX</td>
      <td>🇲🇽</td>
    </tr>
    <tr>
      <th>13</th>
      <td>7509546</td>
      <td>6</td>
      <td>{PROTEX, COLGATE, PALMOLIVE}</td>
      <td>COLGATE PALMOLIVE, S.A. DE C.V. COLGATE PALMOLIVE</td>
      <td>MX</td>
      <td>🇲🇽</td>
    </tr>
    <tr>
      <th>14</th>
      <td>0038000</td>
      <td>6</td>
      <td>{PRINGLES}</td>
      <td>Kellogg Company</td>
      <td>US</td>
      <td>🇺🇸</td>
    </tr>
    <tr>
      <th>15</th>
      <td>8413600</td>
      <td>5</td>
      <td>{VEET}</td>
      <td>RECKITT BENCKISER</td>
      <td>ES</td>
      <td>🇪🇸</td>
    </tr>
    <tr>
      <th>16</th>
      <td>0047400</td>
      <td>5</td>
      <td>{GILLETTE}</td>
      <td>Procter &amp; Gamble Company</td>
      <td>US</td>
      <td>🇺🇸</td>
    </tr>
    <tr>
      <th>17</th>
      <td>75010092</td>
      <td>5</td>
      <td>{GILLETTE, PRESTOBARBA}</td>
      <td>NEWELL RUBBERMAID DE MEXICO, S. DE R.L. DE C.V...</td>
      <td>MX</td>
      <td>🇲🇽</td>
    </tr>
    <tr>
      <th>18</th>
      <td>75010563</td>
      <td>5</td>
      <td>{PONDS, SEDAL}</td>
      <td>UNILEVER DE MEXICO, S. DE R.L. DE C.V. UNILEVER</td>
      <td>MX</td>
      <td>🇲🇽</td>
    </tr>
    <tr>
      <th>19</th>
      <td>4005900</td>
      <td>5</td>
      <td>{NIVEA}</td>
      <td>Beiersdorf AG</td>
      <td>DE</td>
      <td>🇩🇪</td>
    </tr>
    <tr>
      <th>20</th>
      <td>0041789</td>
      <td>5</td>
      <td>{MARUCHAN}</td>
      <td>Maruchan, Inc.</td>
      <td>US</td>
      <td>🇺🇸</td>
    </tr>
    <tr>
      <th>21</th>
      <td>75010592</td>
      <td>4</td>
      <td>{COFFEE MATE, NESCAFE}</td>
      <td>NESTLE MEXICO, S.A. DE C.V. NESTLE MEXICO, S.A...</td>
      <td>MX</td>
      <td>🇲🇽</td>
    </tr>
    <tr>
      <th>22</th>
      <td>0000075</td>
      <td>4</td>
      <td>{CORONA, DOVE, REXONA}</td>
      <td>ASOCIACION MEXICANA DE ESTANDARES PARA EL COME...</td>
      <td>MX</td>
      <td>🇲🇽</td>
    </tr>
    <tr>
      <th>23</th>
      <td>0041333</td>
      <td>4</td>
      <td>{DURAC}</td>
      <td>Procter &amp; Gamble Company</td>
      <td>US</td>
      <td>🇺🇸</td>
    </tr>
    <tr>
      <th>24</th>
      <td>0084773</td>
      <td>4</td>
      <td>{ROBINSON CRUSOE}</td>
      <td>Trans Antartic Trading Co Ltda</td>
      <td>CL</td>
      <td>🇨🇱</td>
    </tr>
    <tr>
      <th>25</th>
      <td>8718291</td>
      <td>3</td>
      <td>{PHILI}</td>
      <td>Philips Electronics Nederland B.V.</td>
      <td>NL</td>
      <td>🇳🇱</td>
    </tr>
    <tr>
      <th>26</th>
      <td>75010011</td>
      <td>3</td>
      <td>{HEAD &amp; SHOULDERS, PANTENE}</td>
      <td>CORPORATIVO PROCTER &amp; GAMBLE, S. DE R.L. DE C....</td>
      <td>MX</td>
      <td>🇲🇽</td>
    </tr>
    <tr>
      <th>27</th>
      <td>0040000</td>
      <td>3</td>
      <td>{M &amp; M, SKITTLES}</td>
      <td>Mars Chocolate North America LLC</td>
      <td>US</td>
      <td>🇺🇸</td>
    </tr>
    <tr>
      <th>29</th>
      <td>8710163</td>
      <td>3</td>
      <td>{PHILI}</td>
      <td>Philips Electronics Nederland B.V.</td>
      <td>NL</td>
      <td>🇳🇱</td>
    </tr>
    <tr>
      <th>30</th>
      <td>8710103</td>
      <td>3</td>
      <td>{PHILI}</td>
      <td>Philips Electronics Nederland B.V.</td>
      <td>NL</td>
      <td>🇳🇱</td>
    </tr>
    <tr>
      <th>31</th>
      <td>0000080</td>
      <td>3</td>
      <td>{KINDER}</td>
      <td>Indicod-Ecr - GS1 Italy</td>
      <td>IT</td>
      <td>🇮🇹</td>
    </tr>
    <tr>
      <th>32</th>
      <td>75010086</td>
      <td>2</td>
      <td>{BACARDI}</td>
      <td>BACARDI Y COMPAÑIA, S.A. DE C.V. BACARDI</td>
      <td>MX</td>
      <td>🇲🇽</td>
    </tr>
    <tr>
      <th>33</th>
      <td>75010012</td>
      <td>2</td>
      <td>{HEAD &amp; SHOULDERS}</td>
      <td>CORPORATIVO PROCTER &amp; GAMBLE, S. DE R.L. DE C....</td>
      <td>MX</td>
      <td>🇲🇽</td>
    </tr>
    <tr>
      <th>34</th>
      <td>75010013</td>
      <td>2</td>
      <td>{OLD SPICE, PANTENE}</td>
      <td>CORPORATIVO PROCTER &amp; GAMBLE, S. DE R.L. DE C....</td>
      <td>MX</td>
      <td>🇲🇽</td>
    </tr>
    <tr>
      <th>35</th>
      <td>9044400</td>
      <td>2</td>
      <td>{PEZ}</td>
      <td>PEZ International GmbH</td>
      <td>AT</td>
      <td>🇦🇹</td>
    </tr>
    <tr>
      <th>36</th>
      <td>8712581</td>
      <td>2</td>
      <td>{PHILI}</td>
      <td>Philips Electronics Nederland B.V.</td>
      <td>NL</td>
      <td>🇳🇱</td>
    </tr>
    <tr>
      <th>37</th>
      <td>75010864</td>
      <td>2</td>
      <td>{PRO}</td>
      <td>CORPORATIVO PROCTER &amp; GAMBLE, S. DE R.L. DE C....</td>
      <td>MX</td>
      <td>🇲🇽</td>
    </tr>
    <tr>
      <th>38</th>
      <td>7591083</td>
      <td>2</td>
      <td>{COLGATE}</td>
      <td>COLGATE-PALMOLIVE C.A</td>
      <td>VE</td>
      <td>🇻🇪</td>
    </tr>
    <tr>
      <th>39</th>
      <td>0021200</td>
      <td>2</td>
      <td>{SCOTCH BRITE}</td>
      <td>3M Company</td>
      <td>US</td>
      <td>🇺🇸</td>
    </tr>
    <tr>
      <th>41</th>
      <td>0071603</td>
      <td>2</td>
      <td>{TRIM}</td>
      <td>Pacific World Corporation</td>
      <td>US</td>
      <td>🇺🇸</td>
    </tr>
    <tr>
      <th>42</th>
      <td>08452180</td>
      <td>2</td>
      <td>{VULCA, SIN MARCA}</td>
      <td>GS1 China</td>
      <td>CN</td>
      <td>🇨🇳</td>
    </tr>
    <tr>
      <th>43</th>
      <td>0070501</td>
      <td>2</td>
      <td>{NEUTROGENA}</td>
      <td>Neutrogena Corporation</td>
      <td>US</td>
      <td>🇺🇸</td>
    </tr>
    <tr>
      <th>44</th>
      <td>0022000</td>
      <td>2</td>
      <td>{WRIGLEYS}</td>
      <td>Wm. Wrigley Jr. Company</td>
      <td>US</td>
      <td>🇺🇸</td>
    </tr>
    <tr>
      <th>45</th>
      <td>4015400</td>
      <td>2</td>
      <td>{PAMPERS}</td>
      <td>Procter &amp; Gamble GmbH Wasch- u. Reinigungsmittel</td>
      <td>DE</td>
      <td>🇩🇪</td>
    </tr>
    <tr>
      <th>46</th>
      <td>0079400</td>
      <td>2</td>
      <td>{REXONA}</td>
      <td>Unilever Home and Personal Care USA</td>
      <td>US</td>
      <td>🇺🇸</td>
    </tr>
    <tr>
      <th>47</th>
      <td>5000329</td>
      <td>2</td>
      <td>{BEEFEATER}</td>
      <td>Chivas Brothers Limited</td>
      <td>GB</td>
      <td>🇬🇧</td>
    </tr>
    <tr>
      <th>48</th>
      <td>0012800</td>
      <td>1</td>
      <td>{RAYOV}</td>
      <td>Spectrum Brands, Inc.</td>
      <td>US</td>
      <td>🇺🇸</td>
    </tr>
    <tr>
      <th>49</th>
      <td>9002490</td>
      <td>1</td>
      <td>{RED BULL}</td>
      <td>Red Bull GmbH</td>
      <td>AT</td>
      <td>🇦🇹</td>
    </tr>
    <tr>
      <th>50</th>
      <td>0079200</td>
      <td>1</td>
      <td>{NERDS}</td>
      <td>Sunmark</td>
      <td>US</td>
      <td>🇺🇸</td>
    </tr>
    <tr>
      <th>51</th>
      <td>0000040</td>
      <td>1</td>
      <td>{KINDER}</td>
      <td>Nestlé Deutschland AG</td>
      <td>DE</td>
      <td>🇩🇪</td>
    </tr>
    <tr>
      <th>52</th>
      <td>0070942</td>
      <td>1</td>
      <td>{GUM}</td>
      <td>Sunstar Americas, Inc.</td>
      <td>US</td>
      <td>🇺🇸</td>
    </tr>
    <tr>
      <th>53</th>
      <td>8412300</td>
      <td>1</td>
      <td>{NIVEA}</td>
      <td>BEIERSDORF MANUFACTURING TRES CANTO</td>
      <td>ES</td>
      <td>🇪🇸</td>
    </tr>
    <tr>
      <th>54</th>
      <td>8715200</td>
      <td>1</td>
      <td>{NIVEA}</td>
      <td>Beiersdorf N.V.</td>
      <td>NL</td>
      <td>🇳🇱</td>
    </tr>
    <tr>
      <th>55</th>
      <td>0051000</td>
      <td>1</td>
      <td>{sin marca}</td>
      <td>Campbell Soup Company</td>
      <td>US</td>
      <td>🇺🇸</td>
    </tr>
    <tr>
      <th>57</th>
      <td>0080432</td>
      <td>1</td>
      <td>{CHIVAS REGAL}</td>
      <td>Pernod Ricard USA LLC</td>
      <td>US</td>
      <td>🇺🇸</td>
    </tr>
    <tr>
      <th>58</th>
      <td>0016304</td>
      <td>1</td>
      <td>{SIN MARCA}</td>
      <td>Unofficial Guides Ltd.</td>
      <td>US</td>
      <td>🇺🇸</td>
    </tr>
    <tr>
      <th>59</th>
      <td>0013000</td>
      <td>1</td>
      <td>{HEINZ}</td>
      <td>Heinz USA</td>
      <td>US</td>
      <td>🇺🇸</td>
    </tr>
    <tr>
      <th>60</th>
      <td>0099176</td>
      <td>1</td>
      <td>{COLGATE}</td>
      <td>Colgate Palmolive (Central America) S.A.</td>
      <td>GT</td>
      <td>🇬🇹</td>
    </tr>
    <tr>
      <th>61</th>
      <td>0090159</td>
      <td>1</td>
      <td>{MAIST}</td>
      <td>May Cheong Toy Products Factory Limited</td>
      <td>HK</td>
      <td>🇭🇰</td>
    </tr>
    <tr>
      <th>62</th>
      <td>75010587</td>
      <td>1</td>
      <td>{sin marca}</td>
      <td>RECKITT BENCKISER MEXICO, S.A. DE C.V. RECKITT...</td>
      <td>MX</td>
      <td>🇲🇽</td>
    </tr>
    <tr>
      <th>63</th>
      <td>5900273</td>
      <td>1</td>
      <td>{COLGATE}</td>
      <td>Colgate-Palmolive (Poland) Sp. z o.o.</td>
      <td>PL</td>
      <td>🇵🇱</td>
    </tr>
    <tr>
      <th>64</th>
      <td>5410316</td>
      <td>1</td>
      <td>{SMIRNOFF}</td>
      <td>DIAGEO  SCOTLAND LTD</td>
      <td>GB</td>
      <td>🇬🇧</td>
    </tr>
    <tr>
      <th>65</th>
      <td>5011013</td>
      <td>1</td>
      <td>{BAILEYS}</td>
      <td>GS1 Ireland</td>
      <td>IE</td>
      <td>🇮🇪</td>
    </tr>
    <tr>
      <th>66</th>
      <td>5010103</td>
      <td>1</td>
      <td>{J&amp;B}</td>
      <td>Diageo PLC</td>
      <td>GB</td>
      <td>🇬🇧</td>
    </tr>
    <tr>
      <th>67</th>
      <td>75010080</td>
      <td>1</td>
      <td>{CHOCO KRISPIS }</td>
      <td>KELLOGG COMPANY MEXICO, S. DE R.L.DE C.V. KELLOGG</td>
      <td>MX</td>
      <td>🇲🇽</td>
    </tr>
    <tr>
      <th>68</th>
      <td>75010152</td>
      <td>1</td>
      <td>{PELIK}</td>
      <td>PELIKAN MEXICO, S.A. DE C.V. PELIKAN</td>
      <td>MX</td>
      <td>🇲🇽</td>
    </tr>
    <tr>
      <th>69</th>
      <td>75010329</td>
      <td>1</td>
      <td>{GLADE}</td>
      <td>S.C. JOHNSON AND SON S.A. DE C.V. S.C. JOHNSON...</td>
      <td>MX</td>
      <td>🇲🇽</td>
    </tr>
    <tr>
      <th>70</th>
      <td>4006584</td>
      <td>1</td>
      <td>{L.B.L}</td>
      <td>OSRAM GmbH CRM&amp;S MDS P-W</td>
      <td>DE</td>
      <td>🇩🇪</td>
    </tr>
    <tr>
      <th>71</th>
      <td>75010641</td>
      <td>1</td>
      <td>{CORONA}</td>
      <td>CERVECERIA MODELO, S. DE R.L. DE C.V. CERVECER...</td>
      <td>MX</td>
      <td>🇲🇽</td>
    </tr>
    <tr>
      <th>72</th>
      <td>4005800</td>
      <td>1</td>
      <td>{NIVEA}</td>
      <td>Beiersdorf AG</td>
      <td>DE</td>
      <td>🇩🇪</td>
    </tr>
    <tr>
      <th>73</th>
      <td>303371</td>
      <td>1</td>
      <td>{NESCAFE}</td>
      <td>NESTLE FRANCE SAS</td>
      <td>FR</td>
      <td>🇫🇷</td>
    </tr>
    <tr>
      <th>74</th>
      <td>0742832</td>
      <td>1</td>
      <td>{AOC}</td>
      <td>Tufftek</td>
      <td>US</td>
      <td>🇺🇸</td>
    </tr>
    <tr>
      <th>76</th>
      <td>0681326</td>
      <td>1</td>
      <td>{SIN MARCA}</td>
      <td>Jazwares</td>
      <td>US</td>
      <td>🇺🇸</td>
    </tr>
    <tr>
      <th>77</th>
      <td>7590002</td>
      <td>1</td>
      <td>{ALWAYS}</td>
      <td>PROCTER &amp; GAMBLE DE VENEZUELA, S.C.A.</td>
      <td>VE</td>
      <td>🇻🇪</td>
    </tr>
  </tbody>
</table>
</div>


El *notebook* completo está disponible acá: [https://gist.github.com/jazzido/35990ebf8c236d3df953eb1f0af9e39c](https://gist.github.com/jazzido/35990ebf8c236d3df953eb1f0af9e39c)
