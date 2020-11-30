---
layout: post
title: "Criando e utilizando contêineres"
date: 2020-08-15 16:27:15
image: '/assets/img/'
description: 
tags:
- docker
categories:
- Docker
twitter_text:
exercise: 'https://docs.google.com/forms/d/e/1FAIpQLSeX_8fi7rcE2LQlL6ATMq1etJpkxHPYI-9szTvH1MLfoxilag/viewform'
lab: 'https://docs.google.com/forms/d/e/1FAIpQLSd_LmTPQfLvMv-Kv3oMQMyyjeWdWJtdnEOvR6RDtUPtahT71A/viewform'
presentation: '/assets/pp/Docker_Dia02.ppsx'
---

Agora nós vamos entender as versões do docker e como funciona suas releases, entender o formato e sintaxe do comando de gerenciamento do docker, utilizar alguns comandos para gerenciamento de contêineres e aprender alguns conceitos básicos de rede no docker.
{: .text-justify}

## Como funciona as edições e releases do Docker

O docker em 2017, fez uma reformulação no versionamento e no nome dos produtos que passaram a ser da seguinte forma:

- Docker Datacenter passou a ser Docker EE.
- Docker Engine passou a ser Docker CE.

O Docker possui duas versões:

#### Enterprise Edition (Docker EE)

É a versão comercial do docker, onde é preciso adquirir uma licença para utilizar.

- **Basic** - Esse nível de versão vem acompanhada da plataforma Docker, o suporte e certificações.
- **Standard** - Essa nível inclui tudo da versão basic e acrescenta um sistema de gerenciamento de contêineres (Docker Datacenter).
- **Advanced** - Esse é o último nível, que inclui tudo das versões anteriores e acrescenta o escaneamento de seguraça para os contêineres (Docker Security Scanning).
{: .text-justify}


#### Community Edition (Docker CE)

É a versão open source e sem custos com licença.
Essa versão é classificada de acordo com a periodicidade de atualização, que pode ser da seguinte forma:  

- **Nightly** - Versão alpha, e é Lançado uma vez ao dia.
- **Edge** - É a versão beta do docker, é lançada mensalmente com os últimos recursos. E tem suporte de 1 mês.
- **Stable** - Essa versão é lançada a cada quatro meses. Pussui suporte por 4 meses desde o lançamento.
{: .text-justify}


O versionamento do docker, após a reformulação citada acima, passou a obedecer o seguinte formato:

**YY.MM** - Onde **YY** são os dois últimos digitos do ano esperado de lançamento da release. E **MM** é a representação númerica para o mês de lançamento da release.
Após a reformulação feita pelo docker em 2017, a primeira release passou a ser 17.03.
Para correção de bugs após o lançamento das versões é adicionado um valor númerico adicional. Por exemplo: 17.03.1


## Principais diferenças entre Docker CE e Docker EE

A tabela abaixo mostra as principais diferenças entre as versões do docker:
![Docker Architecture](/assets/img/docker_ce_ee.png)

## Diferenças entre Contêiner x Máquina Virtual

![VM x Contêiner](/assets/img/container_x_vm.png)

- Máquinas virtuais armazenam a aplicação e os dados dela dentro da VM. Contêineres armazenam **APENAS** á aplicação e/ou o serviço que o serve.
- Em uma arquitetura de micro-serviços, cada pequeno serviço é representado por um contêiner que compreendem de forma conjunta uma aplicação.
- Máquina virtual é um binário que empacota o sistema operacional hospedeiro e as aplicaões. Contêineres empacotam em um binário chamado imagem apenas o necessário para rodá-la.
- Máquinas virtuais são mutáveis, podem mudar de estado. Contêineres são imutáveis e não mudam de estado.
- Contêineres consomem menos recursos computacionais que máquinas virtuais, uma vez que podem compartilhar a mesma imagem e não precisam carregar um sistema operacional completo.

## Executando um contêiner

#### Formato do comando Docker (cliente)

Como já visto anteriormente o comando docker é utilizado para gerenciamento de forma geral. 
Segue abaixo o formato desse comando:

* **Formato** **antigo:**

{% highlight bash %}
docker <command> (options)
{% endhighlight %}

* **Novo** **formato:**

{% highlight bash %}
docker <command> <sub-command> (options)
{% endhighlight %}

#### Iniciando um contêiner

O comando agora mandar executar um contêiner que possui uma aplicação chamada "hello" que exibe algumas informações.
Vamos executar ele usando o comando abaixo:

{% highlight bash %}
server# docker container run hello-world 
{% endhighlight %}

Executando um contêiner a partir de uma imagem **nginx**:

{% highlight bash %}
server# docker container run --publish 8080:80 --detach nginx
{% endhighlight %}

* **\-\-publish-** ou **-p**: Publica uma porta para fora do contêiner.
* **\-\-detach** ou **-d**: Executa o contêiner em background e libera o terminal.

#### O que acontece quando nós executamos um contêiner ?

No fluxograma abaixo, podemos ver as etapas executadas pelo docker para colocar um contêiner em execução:

![Docker Architecture](/assets/img/docker_runtime_flow.png)

#### E o que está acontecendo dentro do contêiner ?

Para responder essa pergunta, podemos executar alguns comandos de gerenciamento:

* O comando abaixo lista os **processos** em um contêiner:

{% highlight bash %}
server# docker container top <id ou nome do contêiner>
{% endhighlight %}

* Esse comando **inspeciona** e mostra detalhes sobre a configuração de um contêiner:

{% highlight bash %}
server# docker container inspect <id ou nome do contêiner>
{% endhighlight %}

* Para verificar a **performance** do contêiner, pode-se utilizar o seguinte comando:

{% highlight bash %}
server# docker container stats <id ou nome do contêiner>
{% endhighlight %}

* Verificando os **logs** da aplicação dentro do contêiner:

{% highlight bash %}
server# docker container logs -f <id ou nome do contêiner>
{% endhighlight %}

* Executando um **shell** dentro de um contêiner:

{% highlight bash %}
server# docker container exec -ti <id ou nome do contêiner> bash
{% endhighlight %}
