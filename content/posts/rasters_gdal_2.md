---
title: "Operações com raster GDAL e python - 2"
date: 2021-10-05
description: ""
tags: ["gis", "python", "raster", "GDAL"]
type: post
weight: 25
---

# Manipulações básicas de arquivos raster usando GDAL e python

&nbsp;

+ Reprojetar raster;

+ Fazer resample de raster;

+ Clipar raster

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

**Resampling** é uma operação na qual pegamos os dados (valores) do raster, tiramos
do seu grid original, e colocamos em um novo grid 
(outro tamanho de pixel: diferentes números de linhas e colunas).

A **Reprojeção** acaba sendo algo semelhante, pois conta com um resampling feito
em um grid de diferente CRS do raster original.

Já a operação conhecida como **clip** é apenas cortar o raster usando, em geral,
um arquivo vetorial (como shape por exemplo) como referência.

&nbsp;

## Reprojetar

```
from osgeo import gdal
import numpy as np

ds = gdal.Open('path_to_raster')
# (apenas p/ checar:)
print(ds.GetGeoTransform()) # upper left corner, spatial resolution
print(ds.GetProjection()) # proj system

# Usar o método gdal.Warp, bem versátil p/ várias dessas operações
dsReprj = gdal.Warp('nameRasterOutput.tif',
                    ds,
                    dstSRS = 'EPSG:4326') # epsg p/ o qual quero mudar
```

&nbsp;

## Resample

Nesse caso, tenho um raster de resolução espacial = 3m, e quero
fazer um resampling para que ele passe a ter resolução espacial = 10m.

```
dsRes = gdal.Warp('ndviRes.tif',
                  ds,
                  xRes = 10, # modificando numero de linhas e colunas
                  yRes = 10,
                  resampleAlg = 'bilinear') # Metodo. default: nearest neighbour

```

&nbsp;

## Clip

Esse é um tipo de atividade bastante comum quando trabalhamos com arquivos
rasteriais e vetorais. Para arquivos rasteriais, é comum usarmos um
arquivo tipo shapefile para delimitar e recortar nossa área de interesse
em um raster (ex: imagem de satélite) maior.

```
dsClip = gdal.Warp('ndviClip.tif', # arquivo que será criado
                   ds, # o raster que foi aberto anteriormente
                   cutlineDSName = 'path_to_vector_file', # meu shapefile de referencia
                   cropToCutline = True, # clip to the extent of the vector file
                   dstNodata = np.nan)

# fechar
ds = dsClip = dsRes = dsReprj = None
```
&nbsp;

> Obs: Usei como fonte para o código o canal *Making Sense Remotely*,
> que tem ótimos tutoriais de GIS para R e python.
> Veja [aqui](https://www.youtube.com/channel/UCrWEjubnm0HenqzOQgAVScw).