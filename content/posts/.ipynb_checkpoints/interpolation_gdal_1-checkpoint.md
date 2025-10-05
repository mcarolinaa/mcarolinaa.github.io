

title: "Interpolação com GDAL e python - 1"
date: 2021-10-20
description: ""
tags: ["gis", "python", "gdal", "Interpolação", "nearest neighbor"]
type: post
weight: 25
---

# Manipulações de arquivos raster usando GDAL e python

&nbsp;

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
**Nearest neighbour**.


```
# Nearest neighbor
# atribui o valor do ponto mais próximo na elipse de busca
# janela de busca em gridding algorithms
# descrito por 3 parametros
# 1-radius1(x); 2-radius2(y); 3-angle
# (para maior detalhamento, checar página do GDAL)

# Arquivo vetorial que será o input da interpolação
pts = ogr.Open('path_to_shp_file', 0)
layer = pts.GetLayer()

for field in layer.schema:
    print(field.name) # checar

pts = layer = None

# algoritmo nn
nn = gdal.Grid('path_to_raster_interpolado',
               'path_to_shp_file',
               zfield = 'rvalue_1', # checa o nome em seu arquivo vetorial
               algorithm = 'nearest')

nn = None # não esquecer de fechar
```

Ao checar o raster, verá que provavelmente está distante da realidade:
os boundaries provavelmente não estão bons.

Eles foram baseados apenas nos pontos do .shp. Para corrigir,
use o argumento outputBound e width e height para atingir uma resolução adequada.

Aqui, checo um raster modelo, e acesso objeto Transform para checar e utilizar
novamente.

```
# Mas antes, importante: Todos os inputs devem ter a mesma projeção!
# Aqui, meu .shp tem uma proj (epsg 4326), e o raster que uso de modelo tem outra
# (epsg 7321)
# deixá-los na mesma:
ds = gdal.Open('path_to_model_raster')
dsReprj = gdal.Warp('path_to_reproj_raster',
                    ds,
                    dstSRS = 'EPSG:4326')
ds = dsReprj = None

# continuando, o mesmo raciocínio, depois de acertar projeções e ver boundaries ok
pts = ogr.Open('path_to_shp_file', 0)
layer = pts.GetLayer()

for field in layer.schema:
    print(field.name)

ndvi = gdal.Open('path_to_reproj_raster')
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

# e faz a interpolação por nn novamente
nn = gdal.Grid('path_to_raster_interpolado_2',
               'path_to_shp_file',
               zfield = 'rvalue_1',
               algorithm = 'nearest',
               outputBounds = [ulx, uly, lrx, lry],
               width = xsize,
               height = ysize)

nn = None
```


Obs: Usei como fonte o canal *Making Sense Remotely*, que tem ótimos tutoriais
de GIS para R e python.
