---
title: "Operações com raster GDAL e python - 1"
date: 2021-10-01
description: ""
tags: ["gis", "python", "raster", "GDAL"]
type: post
weight: 25
---

# Manipulações básicas de arquivos raster usando GDAL e python

&nbsp;

+ Lê e plota arquivo raster usando GDAL

+ cria binary mask e exporta arquivo

&nbsp;

O **GDAL** é uma *translator library* para dados vetoriais e rasteriais, com
modelos próprios para trabalhar esses tipos de dados.

Uma grande diversidade de pacotes e libraries em diversas linguagens são baseados
no GDAL. 
Acredito que vale muito a pena saber usá-la até para entender melhor
a estrutura dos dados no contexto de GIS.

O GDAL tem API para diversas linguagens, incluindo o python.
A API para python pode ser consultada [aqui](https://gdal.org/api/python.html)

&nbsp;

## Lê e plota arquivo raster usando GDAL

```
from osgeo import gdal
import numpy as np
import matplotlib.pyplot as plt

ds = gdal.Open(p + file) # abre somente 1 arquivo raster
gt = ds.GetGeoTransform() # object 
proj = ds.GetProjection()

# para evitar os dados no data de aparecerem na visualizacao
# e atrapalhar o entendimento do conteúdo
nodata = ds.GetRasterBand(1).GetNoDataValue()
data = ds.ReadAsArray()
# importante sempre fechar o arquivo
ds = None

# aplica máscara e checa dimensões e número de bandas
data = np.ma.masked_values(data, nodata)
print(data.shape)
```

&nbsp;

```
fig, ax = plt.subplots(figsize=(5,6), constrained_layout=True)

im = ax.imshow(data, extent=extent) 
cb = fig.colorbar(im, shrink=.5)
ax.axes.xaxis.set_visible(False)
ax.axes.yaxis.set_visible(False)
cb.set_label('NDVI')
```

![Informações do arquivo raster](/posts/pics/ndvi.png)

Acima pode-se observar a distribuição de NDVI em uma área de produção
de cultura agrícola em certo período de intenso desenvolvimento 
(altos valores de NDVI).

&nbsp;

## Criar uma binary mask

Para esse exercício, suponha que quero ver a distribuição do NDVI
apenas onde o mesmo encontra-se acima do valor médio na ocasião.

É uma maneira de explorar, entender os dados que temos em mãos.

```
import matplotlib.colors

# cria a máscara com condição
binmask = np.where((data >= np.mean(data)),1,0) # tudo que for > média, entao =1
# cores p/ quando atender/não atender a condição
cmap = matplotlib.colors.ListedColormap(['red', 'green'])

# plota
fig, ax = plt.subplots(figsize=(5,6), constrained_layout=True)

im = ax.imshow(binmask, extent=extent, cmap=cmap) 
ax.axes.xaxis.set_visible(False)
ax.axes.yaxis.set_visible(False)
cb.set_label('NDVI > média')

```

Nesse caso, tudo que estava acima do valor médio de NDVI da imagem,
agora aparece na cor verde, e abaixo, na cor vermelha.


![Informações do arquivo raster](/posts/pics/ndvi_mask.png)


&nbsp;


## Exporta em disco

```
# ajusta driver p/ exportar
driver = gdal.GetDriverByName('GTiff')
driver.Register()
outds = driver.Create('path_to_binmask.tif',
                      xsize = binmask.shape[1], # number of cols
                      ysize = binmask.shape[0], # number of rows
                      bands = 1,
                      eType = gdal.GDT_Int16)
outds.SetGeoTransform(gt)
outds.SetProjection(proj)
outband = outds.GetRasterBand(1)

# escreve em disco
outband.WriteArray(binmask)
outband.SetNoDataValue(np.nan)
outband.FlushCache()

# não esquecer de fechar o dataset
outband = None
outds = None
```

&nbsp;

> Obs: Usei como fonte para o código o canal *Making Sense Remotely*,
> que tem ótimos tutoriais de GIS para R e python.
> Veja [aqui](https://www.youtube.com/channel/UCrWEjubnm0HenqzOQgAVScw).