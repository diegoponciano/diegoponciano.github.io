---
layout: post
category: lessons
tagline: "Supporting tagline"
tags : [intro, dev, smtp, tutorial]
title: "MailHog: Servidor de e-mails para desenvolvimento"
---
{% include JB/setup %}

O [MailHog](https://github.com/mailhog/MailHog) é um servidor SMTP feito pra simplificar o ambiente de desenvolvimento.  
Já existem outras [soluções](http://mailcatcher.me/) [parecidas](https://pypi.python.org/pypi/maildump), mas essa, feita em Go, facilita nossa vida por evitar dependências.

## Instalação

No próprio [repositório](https://github.com/mailhog/MailHog/releases) do projeto, você encontra os links dos binários disponíveis para download.   
No linux, por exemplo, baixe e instale:

    wget --quiet -O ~/mailhog https://github.com/mailhog/MailHog/releases/download/v0.1.7/MailHog_linux_amd64
    chmod +x ~/mailhog


Execute o MailHog com ```./mailhog```, e abra no endereço [http://localhost:8025/](http://localhost:8025/) a interface web.    
No seu código, aponte as configurações SMTP para ```127.0.0.1:1025```.   
Num projeto Django, basta adicionar no arquivo `settings.py`:

    EMAIL_HOST = '127.0.0.1'
    EMAIL_PORT = '1025'

Use ```./mailhog --help``` se precisar de outras opções de configuração.   
E é isso :)

## Web
O MailHog tem configurações MUITO básicas de autenticação, e pouca personalização, não recomendo utilizá-lo além dos testes.   
Aliás, existem [diversos](http://www.mailgun.com/) [serviços](https://aws.amazon.com/pt/ses/) para te evitar configurar seu próprio servidor de e-mail.   
Mas se você quiser, pode expôr o MailHog (numa instância de testes, por exemplo) facilmente usando nginx e supervisor.   

Crie uma entrada simples do supervisor, (no ubuntu, elas ficam em ```/etc/supervisor/conf.d/```) chamada *mailhog.conf*, com:

    [program:mailhog]
    command=/home/user/mailhog

E pro nginx, crie uma entrada com:

    upstream mailhog {
        server localhost:8025 max_fails=1 fail_timeout=2s;
    }
    map $http_upgrade $connection_upgrade {
        default upgrade;
        "" close;
    }
    server {
        listen 80;
        server_name mail.meuservidor.com;
        location / {
            proxy_pass http://mailhog;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection $connection_upgrade;
        }
    }

Feito isso, aponte o DNS pro tal servidor, e pronto!
