---
title: "Interpolação de dados vetoriais por método linear"
date: 2021-10-23
description: ""
tags: ["gis", "python", "GDAL", "Interpolação", "linear"]
type: post
weight: 25
---

# Manipulações de arquivos raster usando GDAL e python

O **GDAL** é uma *translator library* para dados vetoriais e rasteriais, com
modelos próprios para trabalhar esses dados.

Uma grande diversidade de pacotes e libraries em diversas linguagens são baseados
no GDAL. Acredito que vale muito a pena saber usá-la até para entender melhor
a estrutura dos dados no contexto de GIS.

o GDAL tem API para diversas linguagens, incluindo o python.
A API para python pode ser consultada aqui:

https://gdal.org/api/python.html

```
from osgeo import gdal
from osgeo import ogr
```

Nesse post há rotina para a seguinte ação:

**Interpolação** de dados vetoriais para raster usando o método de
**Linear interpolation**.

```
# Arquivo vetorial que será o input da interpolação
pts = ogr.Open('path_to_shp_file', 0)
layer = pts.GetLayer()

for field in layer.schema:
    print(field.name) # checar field name

pts = layer = None

# da mesma maneira que feito p/ o algoritmo nn
# Linear Interpolation

lin = gdal.Grid('/home/maria.carolina/Documents/GIS_routines/lin_ndvi_th001.tif',
               '/home/maria.carolina/Documents/GIS_routines/sampled_ndvi_th001.shp',
               zfield = 'rvalue_1',
               algorithm = 'linear',
               outputBounds = [ulx, uly, lrx, lry],
               width = xsize,
               height = ysize)

lin = None
```

> Obs: Usei como fonte para o código o canal *Making Sense Remotely*,
> que tem ótimos tutoriais de GIS para R e python.
> Veja [aqui](https://www.youtube.com/channel/UCrWEjubnm0HenqzOQgAVScw).
