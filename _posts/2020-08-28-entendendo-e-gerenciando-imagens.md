---
layout: post
title: "Entendendo e gerenciando imagens"
date: 2020-08-15 16:23:16
image: '/assets/img/'
description:
tags:
- docker
categories:
- Docker
twitter_text:
exercise: https://docs.google.com/forms/d/e/1FAIpQLSeDMUn8AZSZHnAPLIa1aBSRON4UPYpbGbVoxJIrKw-D8oM8qw/viewform
lab: 'https://docs.google.com/forms/d/e/1FAIpQLSez6_WEKN-2lTtoPjovBx1kSJCCwxyHccjXVbYtB807tZjczA/viewform'
presentation: '/assets/pp/Docker_Dia04.ppsx'
---

Agora veremos o que é uma imagem no docker, como ela está estrutuda, entender o conceito de camadas, conhecer sobre o cache de imagens local, sobre o repositório online, sobre como identificar uma imagem official, conhecer e executar comandos para gerenciamento de imagens, renomear e colocar tags nas imagens, bem como construir um repositório de imagens local.
{: .text-justify}


## O que é uma imagem ?

Uma imagem é um conjunto de binários e suas dependências necessárias para executar uma determinada aplicação e seus metadados.
Segundo o conceito official do docker: "An image is an ordered collection of root filesystem changes and the corresponding execution parameters for use whithin a container runtime."
Uma imagem não é um sistema operacional completo, não possui kernel, não possui módulos e não dá boot como um sistema operacional.
{: .text-justify}


## Repositório local e remoto

Quando falamos de repositório remoto o docker utiliza um site chamado "hub.docker.com", onde é possível enviar imagens personalizadas para serem armazenadas.
O docker hub possui quatro planos de acesso, o plano **PRO**, **FREE**, **TEAM** e **LARGE**.
{: .text-justify}

Para mais informações sobre os planos as caracterísicas de cada plano, veja: [Docker Hub Plan](https://www.docker.com/pricing "Docker Hub Plan")

É possível também utilizar um repositório local para armazenar nossas imagens, para isso o docker conta com um serviço chamado Registry.
Com ele é possível guardar nossas imagens em um ambiente local para que possamos usar da forma que desejarmos.

#### Recomendações

* Sempre preferir imagens oficiais
* Optar por imagens com boa reputação (muitas estrelas)

## Visão estrutural de uma imagem

Uma imagem no docker é formada por uma ou mais camadas, essas camadas são formadas por um sistema de arquivos chamados UnionFS.
A característica do UnionFS de trabalhar com arquivos e diretórios de sistemas de arquivos diferentes, abstraindo para o usuário essas diferenças é que faz dele o sistema de arquivos utilizado pelo docker.
{: .text-justify}

Algumas características das imagens são:

* Toda imagem começa de uma camada incial conhecida como scratch.
* Uma modificação feita em uma imagem pode alterar ou não o seu tamanho.
* Cada camada possui um hash de identificação único.
* O docker utiliza um cache local para deduplicar o tamanho das imagens.
* Todas as camadas que compõem uma imagem são "somente leitura (RO).
* Ao executar um contêiner o docker cria uma camada que fica acima das camadas da imagem e que possui propriedade de escrita e leitura (RW), também chamada de camada do contêiner.


Abaixo podemos ver na ilustração um exemplo de uma imagem do docker composta de várias camadas:

![Docker image](/assets/img/into-docker-image.png)


Quando o docker precisa acessar algum arquivo que está nas camadas inferiores (camadas RO) é utilizado um procedimento chamado CoW (Copy-On-Write). Esse processo consiste em fazer uma cópia do arquivo que precisa ser alterado, executar a modificação e armazenar na camada RW (camada de contêiner).
{: .text-justify}


## Comandos para gerenciamento

Nessa sessão podemos ver alguns dos comandos básicos para fazer o gerenciamento de imagens no Docker.

O comando abaixo é utilizado para baixar uma imagem do repositório docker ([hub.docker.com](hub.docker.com "Docker Hub")) ou de algum repositório próprio para o cache local.
{: .text-justify}

{% highlight bash %}
server# docker image pull <repo:tag>
{% endhighlight %}

* Para fazer upload de uma imagem:

{% highlight bash %}
server# docker image push <repo:tag>
{% endhighlight %}

* Para listar as imagens que encontram-se no cache local, o comando de ser:


{% highlight bash %}
server# docker image ls
{% endhighlight %}

* Em caso de necessitar verificar o histórico de uma determinada imagem, pode ser feito através do comando abaixo:

{% highlight bash %}
server# docker image history <repo:tag ou id da imagem>
{% endhighlight %}
<em><strong>Se não especificado a tag, será baixada a última imagem disponível.</strong></em>

* Para remover uma imagem:

{% highlight bash %}
server# docker image rm <repo:tag ou id da imagem>
{% endhighlight %}
<em><strong>Se algum contêiner tiver utilizando a imagem, a flag -f pode ser utilizada para forçar a remoção da imagem junto com o contêiner.</strong></em>

* Para mostrar as configurações e parâmetros que estão associadas a determinada imagem, comando é o seguinte:

{% highlight bash %}
server# docker image inspect <repo:tag ou id da imagem>
{% endhighlight %}

