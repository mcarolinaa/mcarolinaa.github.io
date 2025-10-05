---
title: "Operações com raster GDAL e python - 4"
date: 2021-10-12
description: ""
tags: ["gis", "python", "raster", "GDAL"]
type: post
weight: 25
---

# Manipulações básicas de arquivos raster usando GDAL e python

&nbsp;

+ Merge (ou 'juntar') os pedaços de um raster

&nbsp;

O **GDAL** é uma *translator library* para dados vetoriais e rasteriais, com
modelos próprios para trabalhar esses tipos de dados.

Uma grande diversidade de pacotes e libraries em diversas linguagens são baseados
no GDAL. 
Acredito que vale muito a pena saber usá-la até para entender melhor
a estrutura dos dados no contexto de GIS.

O GDAL tem API para diversas linguagens, incluindo o python.
A API para python pode ser consultada [aqui](https://gdal.org/api/python.html)

Conforme foi feito anteriormente (Manipulação de rasters usando GDAL
e python - 3), em que um raster foi dividido em partes, agora será
feito o caminho inverso, portanto, 'juntando' novamente os pedaços
do raster.


## Libraries

```
from osgeo import gdal
import numpy as np
import matplotlib.pyplot as plt
import glob
import subprocess
```
&nbsp;

No exemplo, continuo usando o mesmo raster (NDVI), que foi didivido
na manipulação de rasters com GDAL - 3.

![Informações do arquivo raster](/posts/pics/ndvi_pieces.png)

(Obs: Apenas algumas partes do raster total são mostradas na Figura acima,
para fins de entendimento que o arquivo foi dividido em partes (nesse caso, 9).

&nbsp;

## Abre as partes e faz merge dos rasters

```
# apontar para a lista de rasters que quer o merge
ndviList = glob.glob('ndvi[0-9][0-9].tif') # ex: rasters de ndvi
print(ndviList)
```

&nbsp;

A seguir, 2 maneiras de fazer o processo.

### 1. Usar subprocess

É praticamente a mesma coisa que faríamos ao usar o GDAL pelo terminal.

Nesse caso, importante checar a documentação para ficar bem claro quais são
e como devem ser chamados os argumentos.

```
# Jeito 1: Usando subprocess
# (tipo usando o terminal)

cmd = 'gdal_merge.py -ps 3 -3 -o mergedNDVI1.tif'
subprocess.call(cmd.split()+ndviList)
```
&nbsp;

### 2. Criar um *virtual raster* e transformá-lo em GeoTiff
(essa maneira é melhor para arquivos maiores)

```
# Jeito 2: Cria um virtual raster, e transforma em GeoTiff
# (melhor p/ arquivos maiores)

# O virtual raster é mais fácil de ser processado
vrt = gdal.BuildVRT('mergedNDVI2.vrt',
                    ndviList)
# isso acima é um xml, que indica ao GDAL onde os arquivos estão
# e a seguir, cria um GeoTiff a partir disso
gdal.Translate('mergedNDVI2.tif',
               vrt,
               xRes = 3,
               yRes = -3)
vrt = None # nao esquece de fechar
```

&nbsp;

Em ambas as situações, tenho como resultado final exatamente o mesmo
produto, o raster original, utilizado para todas as manipulações
feitas nos últimos posts.

![Informações do arquivo raster](/posts/pics/merged_ndvi.png)

&nbsp;

> Obs: Usei como fonte para o código o canal *Making Sense Remotely*,
> que tem ótimos tutoriais de GIS para R e python.
> Veja [aqui](https://www.youtube.com/channel/UCrWEjubnm0HenqzOQgAVScw).