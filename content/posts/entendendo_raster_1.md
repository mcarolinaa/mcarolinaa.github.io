---
title: "Entendendo os dados de um raster (1)"
date: 2022-03-10
description: ""
tags: ["gis", "raster", "GDAL", "python"]
type: post
weight: 25
---

# Características básicas de um arquivo raster e como entendê-las

&nbsp;

+ Ler um raster

+ Esquema para entender algumas das informações 

+ Visualização

&nbsp;

## Ler um raster

Operações com e entre rasters são bem comuns e importantes nas análises espaciais.

No python, o uso do **GDAL e numpy** já permite a realização de muitas dessas
operações.

O uso do GDAL (Geospatial Data Abstraction Library) e entendimento de sua
documentação é mais complexo e difícil (quando
comparado a outras libraries como rasterio, que faz um ótimo trabalho usando o 
modelo de dados do GDAL, de maneira mais simplificada), especialmente no início.

Mas o uso do GDAL possui a vantagem de que (com um pouco de paciência)
acabamos compreendendo melhor o porque das coisas.
Além disso, como a maior parte dos pacotes de análises espaciais são baseados do uso do GDAL
(de maneira mais simplificada), conseguimos ter melhor entendimento sobre o que cada função
realmente faz.

Após sua instalação, podemos começar a utilizá-lo. Ao abrirmos um raster, 
 deparamos com algums informações básicas e importantes
para que possamos entender melhor o contexto daqueles dados geoespaciais.

Algumas dessas informações referem-se às **coordenadas de algum ponto inicial, 
resolução espacial, sistema de coordenadas geográficas**, etc.

&nbsp;

O código a seguir pega um raster (em formato .tif), abre, pega informações sobre
coordenadas e resolução e coloca as bandas (considerando sua ordem)
em arrays, para ser usada em histogramas, scatterplots e outros.

&nbsp;

```
import numpy as np
from osgeo import gdal
import matplotlib.pyplot as plt
import seaborn as sn

# Abre
p = '/directory/path/'
raster = gdal.Open(p + 'raster_name.tif')

# Guardando algumas informações importantes:
gt = raster.GetGeoTransform() # equivalente ao Affine do rasterio
xmin = gt[0] # x coord do canto superior esquerdo, do pixel superior esquerdo
pxw = gt[1] # resolução espacial (largura pixel) (w-e)
ymax = gt[3] # y coord do canto superior esquerdo, do pixel superior esquerdo
pxh = gt[5] # altura pixel (n-s, por isso valor negativo)
# gt[2] e gt[4] são as rotações de linha e col (geralmente = 0)

xsize = raster.RasterXSize # núm de cols
ysize = raster.RasterYSize # núm de linhas

# Infos p/ caso precise trabalhar com tamanho desse raster (ou outro igual)
# ex, refazer um raster similar
xstart = xmin + pxw/2 # w-e
ystart = ymax + pxh/2 # n-s

```

&nbsp;

## Esquema para entender algumas das informações

Inicialmente achei um pouco estranho todas essas informações, e com
frequência esquecia o que era o que.

Uma representação dessas informações, mesmo que simples,
pode auxiliar nesse processo, como a figura a seguir.

Considere que o raster em questão refere-se à área em verde na imagem 
abaixo, como sendo um talhão de uma fazenda.

![Informações do arquivo raster](/posts/pics/dados_raster.png)

Veja que uma parte das informações resultantes do método .GetGeoTransform()
refere-se não propriamente à imagem quando a observamos no QGIS, por exemplo
(que seria só a área verde), mas todo o contorno que preenche a array, 
na região em azul.

Saber informações como o tamanho do pixel (pxw, pxh), número de colunas e linhas,
e o local de inicio da "escrita" do raster é muito importante caso você tenha
que reconstruir o raster, ou criar outro à sua semelhança.

&nbsp;

## Visualização

Os dados do raster também podem ser visualizados com alguns pequenos ajustes
nessa fase exploratória inicial.

Mascarando os *nodata values* (aquilo que está dentro da array, mas não é de fato
a imagem, como no esquema acima), e plotando para checar as bandas.

Nesse caso, tenho um raster de 4 bandas.


```
nodata = raster.GetRasterBand(1).GetNoDataValue()
data = raster.ReadAsArray()
ds = None

data = np.ma.masked_values(data, nodata)

# Raster com 4 bandas (.shape[0])
ys = data.shape[1]
xs = data.shape[2]
ulx, xres, _, uly, _, yres = gt
extent = [ulx, ulx+xres*xs, uly, uly+yres*ys]

fig, ax = plt.subplots(figsize=(5,6), constrained_layout=True)

im = ax.imshow(data[0], extent=extent) # Blue band é a primeira
cb = fig.colorbar(im, shrink=.5)
ax.axes.xaxis.set_visible(False)
ax.axes.yaxis.set_visible(False)
cb.set_label('Blue band')

```

Consigo visualizar as imagens de duas das bandas:

```
fig, ax = plt.subplots(figsize=(5,6), constrained_layout=True)

im = ax.imshow(data[3], extent=extent) 
cb = fig.colorbar(im, shrink=.5)
ax.axes.xaxis.set_visible(False)
ax.axes.yaxis.set_visible(False)
cb.set_label('Near infrared band')
```

![Informações do arquivo raster](/posts/pics/blue_band.png)

![Informações do arquivo raster](/posts/pics/nir_band.png)

Outra forma interessante de se avaliar inicialmente os dados de um raster,
é pelo uso de **histogramas**.  

São ferramentas úteispara checar o intervalo dos dados de reflectância,
bem como fazer um comparativo entre bandas.

```
# Como já temos a máscara do nodata, e os dados já são array,
# apenas coloca em formato flat
blue = data[0].flatten()
green = data[1].flatten()
red = data[2].flatten()
nir = data[3].flatten()

# faz histograma de todas as bandas
plt.figure(figsize=(4,3))
sn.histplot(blue, color='blue')
sn.histplot(red, color='red')
sn.histplot(green, color='green')
sn.histplot(nir, color='purple')
plt.title('Blue, Green, Red and NIR bands')
plt.show()
```
![Informações do arquivo raster](/posts/pics/hist_all.png)

Dessa maneira, podemos começar a entender um pouquinho melhor sobre essa library
universal tão poderosa, o GDAL, e sobre nossos dados.

&nbsp;

Referências:

+ [GDAL](https://gdal.org/)

+ API GDAL para [python](https://gdal.org/api/index.html#python-api)