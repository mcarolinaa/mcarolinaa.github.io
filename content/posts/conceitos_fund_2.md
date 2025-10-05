---
title: "Conceitos Fundamentais em SIG - 2"
date: 2021-10-31
description: ""
tags: ["gis", "conceitos fundamentais"]
type: post
weight: 25
---

# Map Projection, UTM, Horizontal and Vertical Datum, NAVD88, Geodetic Datum

&nbsp;

+ O que é Map Projection

+ O que é UTM

+ O que são Horizontal e Vertical Datum

+ O que é o NAVD88

+ O que é Geodetic Datum

&nbsp;

## Map projection

Sendo o *Datum* uma ferramenta para que entendamos a localização das coordenadas
de features num elipsoide, a **map projection** ou projeção do mapa, é o processo
posterior para que possamos transferir essa localização para um mapa plano.

Ou seja, é uma maneira de renderizarmos localizações de um superfície curva da Terra
para uma superfície plana de um mapa.

## UTM

Nesse mesmo contexto, quando estamos analisando dados geoespaciais, é comum
trabalharmos com o **UTM** (Universal Transversal Mercator), um sistema de grid
de coordenadas planas para que possamos definir uma *map projection*.

É um sistema global, baseado no Transverse Mercator, e possui 60 zonas,
 as quais estão separadas por 6 graus de longitude.

UTM pode ser usado com qualquer Datum, mas os resultados variam entre os mesmos.
Seu uso é mais comum em grandes pojetos de GIS, ou seja, que abrangem grandes
áreas.

As distâncias nesse sistemas são especificadas em metros (north, east) de acordo
com uma origem.

Apesar de o termo *datum* ser usado de maneira análoga ao CRS, esses não são a mesma
coisa. O Datum é na verdade um importante componente do CRS.

Nesse sentido, é importante lembrar que *elipsoide* e *geoide* são distintas
maneiras de se modelar o formato do planeta.

O modelo elipsoide é um modelo aproximado, e mais uniforme, do planeta.
É esférico, mas de maneira imperfeita, com superfície mais suave e ininterrupta.
Possui 2 parâmetros, o raio ao Equador, e a distância do centro a um dos pólos.

O modelo geoide é mais dinâmico, e considera as ondulações naturais do planeta.
A superfície do planeta é de fato irregular, com elevações e depressões, que o modelo
tenta considerar.

## Horizontal e Vertical Datum

Considerando que elipsoide e geoide são duas maneiras de tentar descrever o formato
do planeta, é aí que entram os Datuns, horizontais ou verticais.

O **datum horizontal** usa o modelo elipsoide e associa seu centro a um ponto de
origem relativo ao centro do planeta.

O exemplo mais conhecido disso é o **WGS 84**, desenvolvido para ser de uso global,
e com boa acurácia.

Importante porque: além de ser um dos mais usados, é o datum no qual os dados
de **GPS** são referenciados.

Entendendo que o *datum horizontal* nos auxilia a determinar uma localização na
superfície terrestre, o **datum vertical** nos auxilia com as informações de
elevação.

Encontram-se em 3 grandes categorias: os baseados no *Mean Sea Level*,
os baseados em superfícies derivadas de marés *Tidal Datums*, e os tri-dimensionais,
*3-D Datums*.

Importante lembrar que ao analisarmos bancos de dados de maneira comparativa,
eles devem estar referenciados no mesmo Datum.

## NAVD88

**NAVD88** refere-se ao *North American Vertical Datum of 1988*, é o datum vertical
atual usado nos EUA, que logo será substituído por uma versão mais nova, mais focada
no uso de dados provenientes de GNSS.

## Geodetic datum

Como já citado anteriormente, os Datum têm como principal função oferecer a localização
de certo ponto na superfície terrestre (por exemplo por meio de valores de lat/lon).
Assim como os modelos elipsoide e geoide atuam na tentativa de "descrever" o formato
da Terra, sendo que a contribuição de cada (elipsoide, superfície 2D) e geoide
(valor Z) torna-se bem importante. A combinação disso pode ser entendidad como o que
representa o **geodetic datum** ou apenas **datum**.

&nbsp;

Referências:

+ [EPSG](https://epsg.io/)

+ [LearnGIS](https://vector.geospatial.science/textbook/chapter-two-navigating-our-world)

&nbsp;

> Obs: Os tópicos nos posts teóricos, de conceitos, foram retirados de um ótimo
material e guia (não oficial) de estudo para o *GISP Exam*,
que pode ser encontrado [aqui](https://www.gisci.org/Portals/0/PDF's/GISP%20Unofficial%20Study%20Guide%202019%20v2.pdf?ver=hWEX0NspSVz4XcX0FFf3YA%3d%3d)

