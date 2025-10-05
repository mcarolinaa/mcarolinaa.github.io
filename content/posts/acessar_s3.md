---
title: "Acessar arquivos S3 via python"
date: 2021-10-26
description: "Acessar arquivos armazenados no AWS S3 via python"
tags: ["s3", "python"]
type: post
weight: 25
---

# Acessando arquivos no Amazon S3

Objetivo: Acessar arquivos contidos no serviço de armazenamento da Amazon
Web Services via python.

Sobre o serviço:

https://aws.amazon.com/pt/s3/

A rotina abaixo contempla uma situação de identificar e avaliar alguns arquivos
que você possa ter no S3. 

Considera-se que as credenciais de acesso já estejam corretamente setadas
na sua máquina.

```
from osgeo import gdal
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import boto3
import io
```

## 1. Acessar bucket do S3 por meio do boto3 client

```
session = boto3.Session(profile_name='default')
s3_client = session.client('s3')
s3_bucket_name = 'nome_do_bucket'
s3 = boto3.resource('s3')
my_bucket = s3.Bucket(s3_bucket_name)
```

## 2.1 Listar todos os arquivos, sem filtros

```
bucket_list = []
for file in my_bucket.objects.filter(Prefix='path_do_bucket_e_arquivos'):
    file_name = file.key
    bucket_list.append(file.key)  # lista arquivos com nomes e paths
```

Em outro cenário, no qual tenho diferentes tipos de arquivos
e gostaria de separá-los:

## 2.2 Listar arquivos e filtrar (Tenho .csv e .tif)

```
bucket_csv = []
bucket_tif = []
for file in my_bucket.objects.filter(Prefix='path_do_bucket_e_arquivos'):
    file_name = file.key
    if file_name.split("/")[-1].endswith(".csv"):
        bucket_csv.append(file.key)  # listar com nomes de arquivos e paths
    else:
        bucket_tif.append(file.key)
```

### .csv

### 2.2.1. Lê os arquivos do s3 --> lista de data frames

```
list_dfs = []
for file in bucket_csv:
    obj = s3.Object(s3_bucket_name, file)
    data = obj.get()['Body'].read()
    list_dfs.append(pd.read_csv(io.BytesIO(data)))
```

### .tif

Nesse caso, tenho apenas 1 raster, vou apenas abrir e plotar

### 2.2.2 usando vsis3_streaming/ (file system handler)

```
ds = gdal.Open(str('/vsis3_streaming/' + s3_bucket_name + '/' + bucket_tif[0]))
array = ds.GetRasterBand(1).ReadAsArray()
plt.imshow(array)
plt.colorbar() # add a color scale
plt.title('Título do raster')
ds = None
```
