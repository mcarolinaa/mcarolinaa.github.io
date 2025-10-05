---
title: "Interpolação de dados vetoriais usando Moving Average"
date: 2021-10-23
description: ""
tags: ["gis", "python", "GDAL", "Interpolação", "moving average"]
type: post
weight: 25
---

Veja aqui um exemplo de como interpolar dados vetoriais, trazendo-os para
o formato raster usando o método **Inverse Distance Weighting**,
mais conhecido como **IDW**, por meio do uso do python e GDAL.
 
&nbsp;

Apenas lembrando que:

O **GDAL** é uma *translator library* para dados vetoriais e rasteriais, com
modelos próprios para trabalhar esses tipos de dados.

Uma grande diversidade de pacotes e libraries em diversas linguagens são baseados
no GDAL. 
Acredito que vale muito a pena saber usá-la até para entender melhor
a estrutura dos dados no contexto de GIS.

O GDAL tem API para diversas linguagens, incluindo o python.
A API para python pode ser consultada [aqui](https://gdal.org/api/python.html)

&nbsp;

## Moving Average é um dos métodos mais rápidos de processamento

Além disso, é um método bastante **simples** de se entender.

A base conceitual, de que **pontos mais próximos são mais semelhantes** do que os
mais distantes entre si, se mantém.

Nesse método, trabalhamos com uma **janela de busca de valores**, e com base nesses
valores é que um **valor médio** será calculado e associado ao ponto desejado.

O GDAL, por meio do gdal.Grid, tem diversos parâmetros que podem ser usados
nesse algoritmo, sendo que eles têm o poder de **detalhar e definir a janela** de
busca de valores (radius1, radius2, angle, max_points...).

&nbsp;

Continuo usando os mesmos pontos extraídos (100 no total), de maneira aleatória 
de uma imagem de NDVI, e usado nos exemplos anteriores de interpolação.

Os pontos estão dispostos da seguinte maneira:

![Pontos amostrados](/posts/pics/sample_ndvi.png)

É claro que com essa distribuição heterogênea não teremos uma boa qualidade
do produto final, independente do método. Mas seguindo com o objetivo de
exemplificar as metodologias e compará-las.


## Libraries

```
from osgeo import gdal
from osgeo import ogr
```

&nbsp;

## Dados vetoriais e ajustes

```
# Moving average
# algoritmo c/ parametros referentes à elipse de busca
# (a janela de busca é uma elipse)
# faz média dos pontos dentro da elipse de busca

# Arquivo vetorial que será o input da interpolação
p = '/Users/mariacsandrea/Dropbox/Mac/Documents/personal_page/notebooks_scripts/'
pts = ogr.Open(p + 'sampled_ndvi.shp', 0)
layer = pts.GetLayer()

for field in layer.schema:
    print(field.name) # checar
```

Lembrando de checar o nome da coluna de atributos do arquivo vetorial.

Outro passo importante é ter as informações sobre o raster que será criado,
e nesse caso, eu venho retirando informações do raster original de NDVI
do qual retirei as amostras aleatórias.

```
# Importante: Todos os inputs devem ter a mesma projeção!
# Caso eu tivesse, por exemplo, o vetor em epsg 4326 e precisasse
# mudar para o epsg do raster:
# ds = gdal.Open('path_to_model_raster')
# dsReprj = gdal.Warp('path_to_reproj_raster',
#                     ds,
#                     dstSRS = 'EPSG:4326')
# ds = dsReprj = None
# Além disso, uso informações do raster original (de onde tirei os pontos amostrais)
# para criar o arquivo interpolado

# Continuando, após de acertar projeções 
ndvi = gdal.Open(p + 'mergedNDVI2.tif')
gt = ndvi.GetGeoTransform()

ulx = gt[0] # upper left corner
uly = gt[3]
res = gt[1]

xsize = ndvi.RasterXSize
ysize = ndvi.RasterYSize

# (para calcular os boundaries do raster)
lrx = ulx + xsize * res # lower right x
lry = uly - ysize * res # lower right y

ndvi = None
pts = layer = None
```

## Algoritmo Moving Average

```
# da mesma maneira que feito p/ o algoritmo nn, idw
# exporta, checa o raster e vê se os parâmetros estão aceitáveis
# e refaz se necessário
ma = gdal.Grid(p + 'ndvi_moving_avg',
               p + 'sampled_ndvi.shp',
               zfield = 'VALUE',
               algorithm = 'average:radius1=500:radius2=800:angle=20',
               outputBounds = [ulx, uly, lrx, lry],
               width = xsize,
               height = ysize)
ma = None
```

&nbsp;

E agora podemos ver o resultado da interpolação, lembrando que para chegar
nos limites exatos do formato do raster original deverá ser feita
a operação de *clip*, utilizando um shape.

Nota-se que o resultado requer ajustes, e a escolha dos parâmetros
que definem a janela (elipse) de busca devem ser avaliados com mais
atenção.

![Moving](/posts/pics/moving_avg_ndvi.png)


&nbsp;

> Obs: Usei como fonte para o código o canal *Making Sense Remotely*,
> que tem ótimos tutoriais de GIS para R e python.
> Veja [aqui](https://www.youtube.com/channel/UCrWEjubnm0HenqzOQgAVScw).

