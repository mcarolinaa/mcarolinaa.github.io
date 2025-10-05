---
title: "Acessar arquivos via Athena e python"
date: 2021-10-26
description: "Acessar arquivos AWS usando serviço Athena e python"
tags: ["athena", "aws", "python"]
type: post
weight: 25
---

# Rodando queries no Amazon Athena por usando python

O Amazon Athena é um serviço de queries que facilita o acesso de dados no
S3 por meio do uso do SQL.

É interessante ler mais sobre as estruturas de serviços do Amazon e como
elas se conectam para que fique um pouco mais claro.

Boto3 é um grande aliado nisso para quem usará python para pegar/analisar
os dados. A sua documentação é bem completa e ajuda bastante no entendimento:

https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/athena.html

(Obs: Considerando que as credencias aws estejam já corretamente setadas na máquina)

```
import boto3
import time
```

+ Checar em qual perfil estão os dados

```
session = boto3.Session(profile_name='profile_name')
athena = session.client(service_name='athena', region_name='sa-east-1')
```

Aqui, diferença com pegar arquivos diretamente de bucket do S3.

Para lidar com problema de paginação, numa view ou tabela, por exemplo:


> query_string: a SQL-like query that Athena will execute

> client: an Athena client created with boto3

> source: https://gist.github.com/schledererj/b2e2a800998d61af2bbdd1cd50e08b76

```
def fetchall_athena(query_string, client):
    query_id = client.start_query_execution(
        QueryString=query_string,
        QueryExecutionContext={
            'Database': 'zeus'
        },
        ResultConfiguration={
            'OutputLocation': 'path_para_a_query'
        }
    )['QueryExecutionId']
    query_status = None
    while query_status == 'QUEUED' or query_status == 'RUNNING' or query_status is None:
        query_status = client.get_query_execution(QueryExecutionId=query_id)['QueryExecution']['Status']['State']
        if query_status == 'FAILED' or query_status == 'CANCELLED':
            raise Exception('Athena query with the string "{}" failed or was cancelled'.format(query_string))
        time.sleep(100)
    results_paginator = client.get_paginator('get_query_results')
    results_iter = results_paginator.paginate(
        QueryExecutionId=query_id,
        PaginationConfig={
            'PageSize': 1000
        }
    )
    results = []
    column_names = None
    for results_page in results_iter:
        for row in results_page['ResultSet']['Rows']:
            column_values = [col.get('VarCharValue', None) for col in row['Data']]
            if not column_names:
                column_names = column_values
            else:
                results.append(dict(zip(column_names, column_values)))
    return results
```


+ Get

```
query_string = 'SELECT * FROM xxxx' # coloque aqui sua query
client = athena
```

```
data_zeus = fetchall_athena(query_string=query_string, client=client)
df_data = pd.DataFrame(data_frame)
```

Export, para que não tenha que sempre fazer a query, e
compressed, porque é relativamente grande:

```
compression_opts = dict(method='zip', archive_name='df.csv')
df_data.to_csv('df.zip', index=False, compression=compression_opts)
```
