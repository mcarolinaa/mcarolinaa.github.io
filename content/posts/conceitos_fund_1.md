---
title: "Conceitos Fundamentais em SIG - 1"
date: 2021-10-29
description: ""
tags: ["gis", "conceitos fundamentais"]
type: post
weight: 25
---

# Georreferenciamento, Pontos de Controle, CRS e ITRS

&nbsp;

+ O que é georreferenciamento

+ O que são pontos de controle

+ Conceito de CRS ou SRS

+ Conceito de ITRS

&nbsp;

## Georreferenciamento

**Georreferenciamento** se refere ao processo de associar uma imagem/mapa a um sistema
de coordenadas, ou seja, associar uma imagem a determinadas localizações num certo
espaço físico.

Esse processo pode ser feito com qualquer objeto no qual se possa associar a uma
localização geográfica.

Os produtos modernos, como imagens de satélite, já possuem relativa precisão com 
relação à sua localização. Porém outros tipos de informações, como alguns bancos
de dados, figuras, fotos podem não possuir informações referentes à localização 
geográfica. E é aí que o processo de georreferenciamento pode ajudar a trazer
uma adequada associação com o espaço geográfico a esses materiais.

## Pontos de controle

O processo de georreferenciamento costuma se dar com o uso de uma camada referência,
a qual está no sistema de coordenadas já desejado, e em geral possui a maior resolução
espacial, a camada na qual queremos fazer o processo, e também costuma-se fazer o uso
dos pontos de controle.

Os **pontos de controle** são localizações, bem conhecidas e adequadamente localizadas,
e que vão servir como um tipo de conexão entre a camada referência para alinhamento
da camada na qual se deseja fazer o georreferenciamento.

Esses pontos serão usados para o processo de transformação na camada que se deseja
trazer para o local correto. Já a conexão entre o ponto de controle da camada
referência e o ponto correspondente na camada a ser georreferenciada, chama-se
par de pontos de controle.

## SRS, CRS

O chamado **spatial reference system (SRS)** ou **coordinate reference system (CRS)**,
às vezes entendidos como coisas diferentes, são na verdade sinônimos.

Uma informação só pode ser considerada geoespacial se estiver acompanhada de uma
informação de CRS.
O software SIG usa essa informação para saber onde seu raster ou vetor está localizado,
por exemplo, além disso o software também deverá saber que método usará para
representar a superfície da Terra e a informação geoespacial.

É interessante saber que a mesma informação, vista sob diferentes CRS não apresentará
alinhamento, devido à essas diferenças de representação.

Um CRS possui como principais componentes:

+ DATUM (um modelo que tenta representar o formato da terra, sendo o mais comum
o WGS84)

+ Projeção (transformação matemática das medidas do planeta em uma superfície
plana. Pode ser feita em diferentes unidades, como as lineares, mais comumente
usadas em análises).

Há muitas projeções, e a escolha por uma deve ser feita de maneira a estar de acordo
com o objetivo de sua análise. Fatores como tolerância a distorções, a nível local,
global, localização, são alguns que devem ser considerados nessa escolha.

Uma maneira muito comum de transmitirmos as informações de CRS, estando de acordo com
a ISO 19111, é o uso do *epsg* (um tipo de sistema de sintetização e descrição dessas
informações, mas não o único).


## ITRS

O **ITRS** ou International Terrestrial Reference System é um sistema desenvolvido
para ser de fato geocêntrico. Tem origem no centro de massa de todo o planeta,
incluindo oceanos e atmosfera. Sua unidade é o metro.
Possui um *frame* derivado, International Terrestrial
Reference Frame (ITRF).

Referências:

> Obs: Os tópicos nos posts teóricos, de conceitos, foram retirados de um ótimo
material e guia (não oficial) de estudo para o *GISP Exam*,
que pode ser encontrado [aqui](https://www.gisci.org/Portals/0/PDF's/GISP%20Unofficial%20Study%20Guide%202019%20v2.pdf?ver=hWEX0NspSVz4XcX0FFf3YA%3d%3d)