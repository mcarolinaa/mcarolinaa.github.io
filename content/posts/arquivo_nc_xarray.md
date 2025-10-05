---
title: "Utilizando netCDF"
date: 2022-10-10
description: ""
tags: ["gis", "NetCDF", "python", "xarray", "shapefile"]
type: post
weight: 25
---

# Operações básicas de arquivos tipo .nc: abrir e clip usando shapefile

&nbsp;

+ Um pouco sobre netCDF

+ Libraries usadas

+ Abre arquivo .nc e clipa usando arquivo shapefile

&nbsp;

## Um pouco sobre netCDF

netCDF (Network Common Data Form) na verdade refere-se a um conjunto de libraries
e tipos de dados, que permitem o uso e manipulação de arquivos do tipo
*array-oriented*, normalmente contendo dados científicos, e multidimensionais.

> Unidata’s Network Common Data Form
(netCDF) is a set of software libraries and machine-independent data formats that support
the creation, access, and sharing of array-oriented scientific data. It is also a community
standard for sharing scientific data.
> 
> fonte: [unidata](https://www.unidata.ucar.edu/software/netcdf/) Data Services and Tools for Geoscience.

Representam estruturar (arquivos extensão .nc) usados para armazenar grandes
quantidades de informação em formato multidimensional,
e que podem ser observados de acordo com a dimensão *tempo*.

Por essas razões, esse é um tipo de arquivo comum de ser encontrado armazenando
informações ambientais, como clima (e suas diversas variáveis).

Diversas instituições adotam esse formato para disponibilizar esse tipo de dado
aos usuários, como por exemplo os dados climáticos em grade disponinilizados pela
[NASA](https://data.giss.nasa.gov/gistemp/).

&nbsp;

## Libraries

Diversas linguagens de programação conversam com esse tipo de arquivo e libraries.

No python por exemplo, há mais de uma maneira de trabalhar com arquivos .nc.
 O próprio **GDAL** permite isso, ver [aqui](https://gdal.org/drivers/raster/netcdf.html).

A library **xarray** é outra ótima opção. A library foi feita justamente para 
trabalhar com arrays multi-dimensionais, de maneira bem simplificada 
(considerando que são arquivos relativamente complexos, e que podem ter
muitas dimensões de variáveis).

A documentação é bem completa, e pode ser encontrada [aqui](https://docs.xarray.dev/en/stable/index.html).

Uma parte bem interessante da página do xarray é a seção *How do I...*,
(veja [aqui](https://docs.xarray.dev/en/stable/howdoi.html) que já traz uma série de dúvidas (que acredito serem bem comuns)
com os respectivos métodos para resolvê-las.

&nbsp;

## Abre arquivo .nc e checa informações

A seguir, passos para abrir um arquivo .nc, ajustar crs, abrir .shp, ajustar crs
e usar o arquivo vetorial para selecionar e clipar parte do arquivo .nc.

```
import numpy as np
import geopandas as gpd
import matplotlib.pyplot as plt
import rioxarray 
import xarray as xr
from shapely.geometry import mapping


# path 
p = 'path/to/directory'

# le o .nc
rain = xr.open_dataset(p + 'prec_daily_UT_Brazil_v2.1_20100101_20151231.nc', decode_coords="all")

# checar se há crs e caso não haja, informar
rain.prec.rio.crs # não há

```
&nbsp;

As principais características do arquivo .nc podem ser vistas a seguir:

![Informações do arquivo .nc](/posts/pics/infos_nc.png)

&nbsp;

Veja acima a menção às **dimensões: tempo, latitude e longitude**.

Nesse caso, tenho dados em grade, para determinado limite geográfico,
com certa duração.

**Data variables** me indica qual é a variável do .nc, nesse caso
tenho precipitação (chuva).

E os metadados do arquivo, referentes à sua origem, na seção de 
**Attributes**.

&nbsp;

## Clipa arquivo .nc usando arquivo shapefile

```
# informa crs e dimensoes espaciais
rain.rio.write_crs(
    "epsg:4326",inplace=True,
).rio.set_spatial_dims(
    x_dim="longitude", y_dim="latitude", inplace=True,
).rio.write_coordinate_system(inplace=True)

# checa visual de 1 dia
rain.prec[0].plot();

```

![Um dia de dados do arquivo .nc](/posts/pics/pais_1dia.png)

&nbsp;

```
# le shapefile p/ clip no .nc
mt = gpd.read_file(p + 'MT_UF_2021/MT_UF_2021.shp')

# vem com crs diferente, deixar td no lat/long
# mt.crs
mt.to_crs('epsg:4326', inplace=True)

# checa crs, devem ser iguais
print(mt.crs)
print(rain.rio.crs)

# clipa o .nc usando o .shp
# c/ o drop=False, apenas irá colocar Nan nas celulas que nao correspondem ao local .shp
clipped =rain.rio.clip(mt.geometry.apply(mapping), mt.crs, drop=True)

# checa visual
clipped.prec[0].plot();
```

![Clipe do arquivo .nc](/posts/pics/mt_1dia.png)

&nbsp;

E assim podemos trabalhar com o arquivo .nc delimitado para uma região de interesse,
nesse caso, dados de precipitação para o estado do Mato Grosso.

&nbsp;

Referências:

Foram usados dados de chuva em grade (Xavier et al. 2016) e arquivo shapefile do estado do
Mato Grosso, disponibilizado pelo IBGE:

+ Xavier, A.C.; King, C.W.; Scanlon, B.R. 2016. Daily gridded meteorological variables in Brazil (1980–2013). International Journal of Climatology 36: 2644-2659.

+ [Malha Municipal IBGE](https://www.ibge.gov.br/geociencias/organizacao-do-territorio/malhas-territoriais/15774-malhas.html?=&t=acesso-ao-produto)