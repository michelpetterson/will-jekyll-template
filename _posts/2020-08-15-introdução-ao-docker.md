---
layout: post
title: "Introdução ao Docker"
date: 2020-08-15 17:01:01
image: '/assets/img/'
description:
tags: 
- docker
categories: 
- Docker
twitter_text:
exercise: 'https://docs.google.com/forms/d/e/1FAIpQLSdvrL2b21MjtsU_dDpo-irGSfZY3KnHVS1RCf9fG6VUZpvTeQ/viewform'
lab: 'https://docs.google.com/forms/d/e/1FAIpQLScgOiwn8BlFgSFhH4NGnc-FHFn961s6hFeRJga5JHXE4xdcPQ/viewform'
presentation: '/assets/pp/Docker_Dia01.ppsx'
---

Nessa aula vamos conhecer um pouco da história do Docker, efetuar sua instalação, conhecer os componentes rodando por trás do serviço e executar alguns comandos básicos iniciais.
{: .text-justify}

## O Docker Engine e seus componentes

O docker é um sistema que funciona utilizando o modelo de comunicação "cliente-servidor". Como principais componentes de sua "engine", podemos citar o seguinte:

- O servidor possui um processo chamado "dockerd" que deve rodar em tempo integral para o funcionamento do docker.
- O daemon "dockerd" é responsável por criar os objetos do docker (imagens, contêineres, redes, volumes e outros).
- Possui uma API de comunição que utiliza a arquitetura REST. Ela é essencial para a comunicação do cliente com o servidor.
- Possui uma interface de linha de comando (CLI) que é utilizado com o binário "docker" para que o cliente se comunique com o servidor docker (dockerd).

Abaixo uma representação gráfica dos componentes que foram mencionados acima:
![Docker Engine Componentes](/assets/img/docker-engine-components.png)

## A arquitetura do Docker 

Como foi dito na sessão anterior, o docker foi desenvolvido sob o modelo de rede "cliente-servidor" e nesse caso o cliente (comando docker) conecta utilizando a API REST ao servidor que é responsável por gerenciar todas as atividades dentro do serviço.
Na imagem abaixo podemos visualizar o fluxo de execução de algumas atividades dentro do docker:
![Docker Architecture](/assets/img/docker-arch.png)
Algumas das caracteristicas são:

- O cliente (binário docker) e o servidor (processo dockerd) podem rodar no mesmo servidor.
- O cliente conecta no servidor através de soquetes do linux ou através da rede. Pode se conectar em um ou mais servidores.
- O daemon "dockerd" pode se comunicar com outros daemons para gerenciar serviços.

## Visão geral de objetos

Abaixo será apresentado uma breve conceituação de alguns dos objetos mais importantes utilizados pelo docker:

#### Imagens
Uma imagem funciona como uma espécie de template "somente leitura" que contém as instruções, binários e bibliotecas para criar e executar um contêiner.
Qualquer imagem pode ser customizada utilizando um arquivo de configuração chamado "Dockerfile", cada instrução escrita nesse arquivo implementa uma nova camada para gerar uma nova imagem.  
#### Contêineres
Um contêiner é uma imagem quando está em execução e utilização. Podemos criar, iniciar, parar e até mesmo deletar um contêiner.
É possível anexar um contêiner a uma ou mais redes, anexar um armazenamento e até mesmo criar uma imagem baseado no seu estado atual.

## Instalação do Docker 

O docker está disponível para diversas plataformas tais como: Windows, Linux e Mac.
Ele pode ser instalado através de métodos diferentes, utilizando o repositório e gerenciador de pacotes de cada distribuição, contudo iremos utilizar o mais recomendado:

Antes de iniciarmos devemos verificar se já existe alguma versão do docker instalada no sistema, e caso tenha é necessário removê-la para evitar conflitos:

Debian:
{% highlight bash %}
server# apt-get -y remove docker*
{% endhighlight %}

CentOS:
{% highlight bash %}
server# yum -y remove docker*
{% endhighlight %}

Depois baixe e instale o docker:
Deve ser executado utilizando usuário root

{% highlight bash %}
server# curl -fsSL get.docker.io | bash
{% endhighlight %}

Para não precisar utilizar o usuário root para utilizar o docker, podemos adicionar um usuário comum da seguinte forma:

{% highlight bash %}
server# usermod -a -G docker $USER
{% endhighlight %}

## Alguns comandos básicos

Depois de instalado, podemos testar se a comunicação do cliente com o servidor está funcionando normal:

{% highlight bash %}
server# docker version
{% endhighlight %}

Para exibir mais informações sobre o servidor docker:

{% highlight bash %}
server# docker info
{% endhighlight %}


