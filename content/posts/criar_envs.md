---
title: "Environments python"
date: 2021-10-25
description: "Criar environments em python via conda e pip"
tags: ["environments", "python"]
type: post
weight: 25
---

# Criar e manejar environments python

**Objetivo:** reunir e resumir principais comandos e passos utilizandos na
criação e principais operações de um novo environment usando pyhton.

Duas maneiras são abordadas aqui: por meio do **conda e do pip**.

Conda e pip, apesar de aqui estarem sendo usados como exemplos para o mesmo
resultado final, não são ferramentas iguais.

Para entender um pouco melhor o que cada um deles representa, alguns materiais
como esse https://www.anaconda.com/blog/understanding-conda-and-pip
podem ajudar.

## 1. Usando conda

+ Instalar miniconda

Caso você ainda não tenha anaconda/miniconda, comece por esse passo.

https://docs.conda.io/en/latest/miniconda.html

+ Criar um environment

```
conda create -n env_name python=3 anaconda
```

+ Ativar / desativar o environment

```
conda activate env_name
conda deactivate
```

+ Instalar libraries dentro do environment

```
conda activate env_name
conda install library_name
```

(checar instruções de instalação da library pelo conda)

+ Listar environments disponíveis

```
conda env list
```

+ Checar todas as libraries instaladas em um environment

```
conda list
```

+ Remover um environment

```
conda remove --name env_name --all
```

+ Checar informações sobre os environments

```
conda info --envs
```

+ Setar um environment específico como disponível em seu jupyter notebook/lab

```
conda install -c anaconda ipykernel
python -m ipykernel install --user --name=env_name
```


## 2. Usando pip

+ Instalar pip

É importante se certificar de que você tenha o pip instalado, e checar qual a sua
versão

```
python3 -m pip install --user --upgrade pip
python3 -m pip --version
```

+ Criar diretório para o(s) environment(s)

Via terminal, vá até um diretório que irá conter o(s) environment(s).

```
mkdir env_name
```

+ Criar o environment

```
python3 -m venv ./env_name
```

+ Ativar / desativar o environment

Para ativar:

```
source ./env_name/bin/activate
```

E desativar:

```
deactivate
```

+ Instalar libraries dentro do environment

```
pip3 install numpy
```
(checar instruções de instalação da library pelo pip)

+ Instalar versões específicas de libraries

```
pip3 install pandas==1.1.1
```
