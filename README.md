# Instalação

1. Criar uma pasta ou link simbólico `public` na raiz deste projeto. Este será o local onde seus projetos devem ser instalados.  
2. Rodar o comando `docker-compose up -d --build` no terminal na pasta deste projeto para realizar a instalação dos containers correspondentes. (Talvez você tenha que utilizar sudo para rodar este comando.
3. Uma vez que a instalação tenha terminado utilize o comando `docker ps` para verificar se os contaiers estão ativos. Você verá algo parecido com isso:
```
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
71f531234074        nginx               "nginx -g 'daemon ..."   6 minutes ago       Up 6 minutes        0.0.0.0:80->80/tcp       dockermagento2_nginx_1
c89fd7c3af6e        php:7.0-fpm         "docker-php-entryp..."   6 minutes ago       Up 6 minutes        0.0.0.0:9000->9000/tcp   dockermagento2_phpfpm_1
4b55726a0a03        mariadb             "docker-entrypoint..."   6 hours ago         Up 6 hours          0.0.0.0:3306->3306/tcp   dockermagento2_mariadb_1
```

Uma vez que a instalaçção foi concluída, você tera os seguintes comandos na pasta do projeto utilizando o terminal:

Ligar: `docker-compose start`
Desligar: `docker-compose stop`
Reiniciar `docker-compose restart`
Ver lista de containers: `docker ps`
Acessar o container: `docker exec -i -t {NOME DO CONTAINER} /bin/bash

# Documentação

O arquivo `docker-compose.yml` é responsável por ditar os containers e suas configurações, la existem as instruções para a criação dos seguintes containers:

-  mariadb
-  phpfpm
-  nginx

## mariadb
É um container de banco de dados utilizando a distribuição `mariadb`. Ao rodar o comando de instalação deste ambiente, será criada uma pasta chamada database e o conteúdo dela são os bancos de dados que forem sendo criados.
O banco de dados tem as seguintes configurações:

`usuário: root`
`senha: root`

A senha pode ser alterada no arquivo `docker-compose.yml`, mas isso deve ser feito antes de instalar.

## php-fpm
Foi construído um build para ele, dizendo todas as específicações que a máquina deve ter. Todos os arquivos dentro da pasta `build/php-fpm` são responsáveis por isso. Caso necessite instalar alguma nova extens˜ão do `PHP` 'é preciso alterar o arquivo `build/php-fpm/Dockerfile` e adicionar a nova extensão. Al'ém disto, a pasta `php` contem todos os arquivos de configuração d php. caso precise fazer alguma alteração nas configurações, edite o arquivo `php/php.ini`.
Extensões do php também devem ser adicionadas nesta pasta, seguindo o exemplo das demais.

Versão do php: `7.0`

## nginx
As configurações do nginx podem ser encontradas dentro da pasta `nginx/nginx.conf` e as configurações de cada site podem ser colocadas dentro de `nginx/sites-enable`.

Ja existe um arquivo chamado `nginx/sites-enable/default.conf` que é uma configuração básica. Todos os projetos que estiverem na pasta `public` podem ser acessados através de `{NOME DO PROJETO}.dev`, contanto que o `.dev` esteja apontando para o localhost (Isso deve ser configurado no dnsmasq ou no hosts.

  Os logs vão estar na pasta `logs`.
