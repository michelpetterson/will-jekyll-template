---
layout: post
title: "Conceitos de redes no Docker"
date: 2020-08-15 16:24:16
image: '/assets/img/'
description:
tags:
- docker
categories:
- Docker
twitter_text:
exercise: 'https://docs.google.com/forms/d/e/1FAIpQLSf67i0CbToziG1gPOTJPx0pg0JVK0nsJC9suOZWGO9GlTBK9g/viewform'
lab: 'https://docs.google.com/forms/d/e/1FAIpQLSfYpavDNlydSWxHBfayHMETgBiwxMvwPrXmfAF4U8KQBXI2wA/viewform'
presentation: '/assets/pp/Docker_Dia03.ppsx'
---

O Docker utiliza rede IP para fazer a comunicação privada entre os contêineres e também para disponibilizar os serviços que rodam neles com o mundo exterior. Nesse tópico veremos como podemos gerenciar, criar e remover redes no Docker, os tipos de redes disponíveis, como funciona a resolução de nomes DNS e a criacação de um DNS RR (Round-Robin).
{: .text-justify}

## Conceitos básicos

![Docker image](/assets/img/docker_net.png)

Abaixo listamos alguns conceitos básicos sobre como o docker gerencia redes para os contêineres:

* Cada contêiner é adicionado a uma rede virtual privada e, **por** **padrão**, cada rede dessas faz parte de uma bridge.
* Cada rede virtual adicionada ao contêiner é roteada para fora do host docker utilizando NAT, onde o ip privado do contêiner é mascarado com o ip de saída da interface apropriada no host docker.
* Cada contêiner em uma mesma rede virtual privada, pode se comunicar um com o outro sem precisar publicar (opção -p/--publish) a porta na execução dele.
{: .text-justify}

**TIP:** <em>Como boa prática, recomenda-se criar uma rede virtual privada para cada aplicação, inclusive fazendo menção ao nome da aplicação como nome da rede. Por exemplo: wp_net, drp_net, mdl_net...</em>
{: .text-justify}

### Drivers de redes nativos

O docker possui suporte a dois tipos de drivers, os chamados nativos que são incluídos junto com a instalação do docker e os chamados de terceiros, que são desenvolvidos por empresas ou pela comunidade e são instalados através do formato de plugin.
Cada driver oferece uma funcionalidade específica para cada caso.
{: .text-justify}

Podemos citar abaixo os drivers nativamente instalados no docker:

* **Bridge** - Cria uma rede privada no host docker e todos os contêineres nessa rede podem se comunicar. Funciona sob o escopo rede local. Normalmente utilizado para desenvolvedores ou iniciantes docker. A segurança é feita utilizando regras que bloqueiam o acesso entre as diversas redes que utilizam esse driver. O acesso externo é feito expondo as portas dos serviços para fora do contêiner.
* **Overlay** - Esse driver tem como caractística a configuração de rede múltipla entre servidores docker que funcionam em cluster. O escopo de funcionamento dele é o swarm. Utiliza a tecnologia VxLAN para estender as redes entre os hosts docker. Provê um canal de criptografia, fornece load balance para os serviços, fornece gerenciamento de endereçamento IPs (IPAM). 
* **MacVlan** - Esse driver fornece uma configuração para conectar o contêiner diretamente a interface física do host docker. Os contêineres são endereçados com endereços IPs roteáveis que estão na subnet rede externa. Devido a isso não preciso utilizar NAT para se comunicar com recursos que estão fora do cluster swarm. A comunicação entre o contêiner e o host pode tem a latência reduzida. O escopo de funcionamento é o local e a configuração é feita por host. Utiliza o conceito de interfaces pai, o qual pode ser um interface comum (eth), uma sub-interface ou uma interface bond.
{: .text-justify}

## Gerenciamento de redes

Agora que entendemos alguns conceitos padrão sobre o funcionamento de redes dentro do docker, vamos utilizar a CLI (comando docker) para entender como fazer o  gerenciar das redes utilizadas pelos contêineres.
{: .text-justify}

* Com o comando abaixo é possível listar todas as redes que forão criadas no docker:
{: .text-justify}

{% highlight bash %}
server# docker network ls
{% endhighlight %}

* Para obter mais detalhes sobre uma determinada rede, é utilizado o seguinte comando:
{: .text-justify}

{% highlight bash %}
server# docker network inspect <id ou nome da rede> 
{% endhighlight %}

* Na criação de uma nova rede, é utilizado o comando abaixo:
{: .text-justify}

{% highlight bash %}
server# docker network create --driver bridge <nome da rede> 
{% endhighlight %}

* Para adicionarmos uma rede á um contêiner que está rodando:
{: .text-justify}

{% highlight bash %}
server# docker network connect <id ou nome da rede>  <id ou nome do contêiner>
{% endhighlight %}

* Desconectar uma rede de um contêiner que está rodando:
{: .text-justify}

{% highlight bash %}
server# docker network disconnect <id ou nome da rede>  <id ou nome do contêiner>
{% endhighlight %}

## Entendendo e gerenciando o DNS no Docker
{: .text-justify}

Por padrão o docker possui um servidor DNS interno, onde é utilizado para receber consultas recursivas provenientes dos contêineres e também criar e gerenciar registros que são associados para comunicação intra-contêineres.
Uma das características mais importantes quando falamos sobre contêineres é a sua característica efêmera, ou seja, o contêiner ele nasceu para morrer, para ter um tempo curto de vida.
{: .text-justify}

#### Alguns pontos importantes

* Sempre que um contêiner é criado ou recriado, o docker aloca um novo ip para ele de forma aleatória. Por esse motivo **NÃO** é uma boa prática fazermos referência a endereços IPs na comunicação intra-contêineres.
{: .text-justify}
* Sempre que um contêiner é criado o docker automaticamente adiciona ao arquivo hosts, dentro contêiner, um registro com o ID associado ao IP do contêiner.
{: .text-justify}
* Por padrão a rede default (bridge) não possui servidor DNS interno integrado. Um workaround pode ser feito usando o parâmetro --link para criar nomes e associar a IPs ao arquivo hosts.
{: .text-justify}
* Quando um contêiner é criado em uma rede diferente da default, automaticamente um registro DNS com o nome do contêiner é criado e pode ser utilizado e resolvido por qualquer outro contêiner que faça parte dessa rede.
{: .text-justify}
* É possível criar CNAMEs para associação a serviços e criação de DNSs RR (Round-Robin).
{: .text-justify}

#### Comandos para gerenciar o DNS

* **Criando DNS RR**

Ao executar um contêiner podemos criar alias para utilizar em serviços e associar diversos contêineres para fazer um load balance.

{% highlight bash %}
server# docker container run --network <nome ou id da rede> --network-alias <nome do alias> <nome da imagem do contêiner>
{% endhighlight %}

