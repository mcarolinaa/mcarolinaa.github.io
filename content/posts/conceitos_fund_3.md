---
title: "Conceitos Fundamentais em SIG - 3"
date: 2021-11-02
description: ""
tags: ["gis", "conceitos fundamentais"]
type: post
weight: 25
---

# WGS 84 e SRID integer

&nbsp;

+ O que é o WGS 84

+ O que é SRID integer

&nbsp;


## WGS 84

**WGS 84** refere-se ao *World Geodetic System*.
Como já apontado nos tópicos
anteriores, para identificarmos uma localização no planeta, precisamos inicialmente
do datum, que irá "descrever" o formato do planeta, e um sistema de projeção,
que irá transformar as medidas em uma superfície.

O WGS 84 foi criado para ser um sistema de posicionamento global, usando
o sistema de coordenadas geográficas (lat/lon) para localização.

O WGS84, que é um datum, mas também pode atuar como o tipo de sistema de
coordenada geográfica, é muito conhecido também por ser o usado pelo **GPS**.

Por representar essas informações, o WGS 84 compreende os seguinte elementos:
elipsoide, datum horizontal e vertical, e sistema de coordenadas.

O WGS 84 é um sistema geodético, assim como os chamados NAD 27 e NAD 83 (voltados
à América do Norte).

&nbsp;

## SRID integer

O **SRID** é o chamado *Spatial Reference IDentifier*, ou identificador espacial
de referência, e é basicamente um identificador para um conjunto de informações
relacionadas a CRS e projeção.

Não existe um único identificador, mas provavelmente o mais conhecido é o
**epsg**.

Um exemplo utilizando os 2 últimos conceitos é o de que o epsg 4326 refere-se
ao *WGS 84 - WGS84 - World Geodetic System 1984, used in GPS*, conforme
pode ser pesquisado [aqui](https://epsg.io/?q=4326)

O epsg está bem presente no dia-dia de quem trabalha com produtos geoespaciais
(utilizando ou criando).

Por exemplo, trabalhar com imagens de satélite de empresa que fornece os 
produtos para seu local, e os mesmos são entregues no **epsg 32721**.
Esse epsg refere-se ao Geodetic CRS WGS 84 / UTM zone 21S.
Se você procurar na página do [epsg.io](https://epsg.io/32721), verá inclusive a representação
da "faixa" do planeta a que ela se refere.


&nbsp;

Referências:

+ [GIS Geography](https://gisgeography.com/wgs84-world-geodetic-system)

> Obs: Os tópicos nos posts teóricos, de conceitos, foram retirados de um ótimo
material e guia (não oficial) de estudo para o *GISP Exam*,
que pode ser encontrado [aqui](https://www.gisci.org/Portals/0/PDF's/GISP%20Unofficial%20Study%20Guide%202019%20v2.pdf?ver=hWEX0NspSVz4XcX0FFf3YA%3d%3d)


