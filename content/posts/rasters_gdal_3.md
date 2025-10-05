---
title: "Operações com raster GDAL e python - 3"
date: 2021-10-10
description: ""
tags: ["gis", "python", "raster", "GDAL"]
type: post
weight: 25
---

# Manipulações básicas de arquivos raster usando GDAL e python

&nbsp;

+ Dividir (ou *split*) o raster em "pedaços" igualmente espaçados

&nbsp;

O **GDAL** é uma *translator library* para dados vetoriais e rasteriais, com
modelos próprios para trabalhar esses tipos de dados.

Uma grande diversidade de pacotes e libraries em diversas linguagens são baseados
no GDAL. 
Acredito que vale muito a pena saber usá-la até para entender melhor
a estrutura dos dados no contexto de GIS.

O GDAL tem API para diversas linguagens, incluindo o python.
A API para python pode ser consultada [aqui](https://gdal.org/api/python.html)

Uma operação interessante e comum é o corte de um raster de acordo com
área desejada/pré definida.

Em algumas ocasiões, pode ser que seja necessário dividir uma área em subáreas.
Para delimitação de áreas experimentais, coleta de amostras,
entre outros.

Nesse caso, a operação a seguir pode ser bastante útil.


## Libraries

```
from osgeo import gdal
import numpy as np
import matplotlib.pyplot as plt

```
&nbsp;

No exemplo, continuo usando o mesmo raster (NDVI) usado no texto de
manipulação de rasters com GDAL - 1.

![Informações do arquivo raster](/posts/pics/ndvi.png)


## Abre e faz split do raster

```
ds = gdal.Open(p + file) 
gt = ds.GetGeoTransform() 
proj = ds.GetProject

xmin = gt[0]
ymax = gt[3]
res = gt[1]

xlen = res * ds.RasterXSize
ylen = res * ds.RasterYSize

div = 3 # split em 9 pedaços, igualmente espaçados

xsize = xlen/div
ysize = ylen/div

xsteps = [xmin + xsize * i for i in range(div + 1)]
ysteps = [ymax - ysize * i for i in range(div + 1)]

for i in range(div):
    for j in range(div):
        xmin = xsteps[i]
        xmax = xsteps[i+1]
        ymax = ysteps[j]
        ymin = ysteps[j+1]

        print('xmin: '+str(xmin))
        print('xmax: ' + str(xmax))
        print('ymin: ' + str(ymin))
        print('ymax: ' + str(ymax))
        print('\n')

        gdal.Warp('ndvi'+str(i)+str(j)+'.tif',
                  ds,
                  outputBounds = (xmin, ymin, xmax, ymax),
                  dstNodata = -9999)
        
ds = None
```

&nbsp;

## Visualiza

Para entender o que aconteceu, a imagem abaixo (vinda do QGIS) mostra
algumas das partes do raster (todas as partes constituem a totalidade
do raster original).

![Informações do arquivo raster](/posts/pics/ndvi_pieces.png)

Obs: a escala de cor foi ajustada para cada parte do raster, por isso
não "conversam" entre si.

&nbsp;

> Obs: Usei como fonte para o código o canal *Making Sense Remotely*,
> que tem ótimos tutoriais de GIS para R e python.
> Veja [aqui](https://www.youtube.com/channel/UCrWEjubnm0HenqzOQgAVScw).