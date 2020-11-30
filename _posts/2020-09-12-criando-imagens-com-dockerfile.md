---
layout: post
title: "Criando imagens com Dockerfile"
date: 2020-08-15 16:22:01
image: '/assets/img/'
description:
tags: 
- docker
categories: 
- Docker
twitter_text:
exercise: 'https://docs.google.com/forms/d/e/1FAIpQLSfY2sYg-gBOtUKQ26RZM3J9L7enPtcs8tcGhRlY4_ZUV9286g/viewform'
lab: 'https://docs.google.com/forms/d/e/1FAIpQLSeYNj2LgVu8N0u7wgwDqlj_S8s9prZJ8TBffbWUXhx5Zbb5KQ/viewform'
---

Agora vamos entender como criar uma imagem personalizada no docker utilizando o dockerfile, entender os parâmetros básicos para colocar no arquivo, conhecer os comandos e parâmetrospara criar a imagem, renomear, colocar tag e enviar para o docker hub.
{: .text-justify}


## Entendendo o Dockerfile

O Dockerfile é um arquivo usado pelo Docker para a contrução de imagens personalizadas. É possível definir várias instruções para a construção da image como: definir variáveis para sem usadas dentro do contêiner, instalar pacotes dentro da imagem, copiar arquivos para dentro da imagem, definir o usuário que irá executar a aplicação, definir a porta que será exposta para fora do contêiner e muitas outras coisas.
{: .text-justify}


<em>Abaixo pode está um exemplo de dockerfile para contrução de uma imagem personalizada do apache.</em>

{% highlight bash %}
# Apache2 in Debian Stretch

FROM "debian:squeeze"

COPY ./sourceconf/sources.list.squeeze /etc/apt/

WORKDIR /etc/apache2

RUN apt-get update && \ 
    apt-get -y --force-yes install apache2 && \
    apt-get clean 

ADD ./sourceconf/ssl /etc/apache2/ssl/

RUN mkdir /var/run/apache2 && \
    chown www-data /var/run/apache2

EXPOSE 80/tcp
EXPOSE 443/tcp

ENTRYPOINT ["/usr/sbin/apachectl"]
CMD ["-D", "FOREGROUND"]

{% endhighlight %}

Explicação dos parâmetros utilizados acima:

* **FROM** - Define a imagem que será utilizada como base para a construção da imagem personalizada.
* **WORKDIR** - Diretório a partir onde todo comando irá executar.
* **COPY** - Copia um arquivo ou diretório local para dentro da imagem.
* **RUN**  - Utilizado para executar qualquer comando do SO durante a construção da imagem.
* **ADD**  - Mesma função do COPY, com mais recursos.
* **EXPOSE** - Expõe a porta que será utilizada pelo serviço para fora do contêiner.
* **ENTRYPOINT** - Comando que será executado em "foreground" pelo contêiner.
* **CMD** - Quando utilizado com o ENTRYPOINT define os parâmetros passado para o comando.
{: .text-justify}

O comando básico utilizado para gerar uma imagem é o seguinte:

{% highlight bash %}
server# docker image build -t <repo:tag> -f Dockerfile .
{% endhighlight %}


## Utilizando Tags

Após criar uma imagem, é possível renomear as tags utilizadas no momento da criação através do seguinte comando:

{% highlight bash %}
docker image tag atual<image:tag> nova<image:tag>
{% endhighlight %}
