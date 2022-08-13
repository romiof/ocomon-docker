# Guia de uso do Ocomon v4 em ambiente Docker

Esse guia tem a finalidade de explicar de forma simples, como subir um ambiente docker para uso em produção, do sistema de chamados [Ocomon](https://ocomonphp.sourceforge.io/), na versão 4.

## Requisitos
- Docker CE instalado no seu servidor.
    - Utilizei com sucesso o Ubuntu Server 22.04 LTS com a versão mais recente do Docker.
    - A [Digital Ocean ensina passo à passo como instalar do Docker.](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-22-04)
    - Pode rodar para debug e testes no Windows, usando o WSL2. [Tutorial da CodeEdu aqui.](https://github.com/codeedu/wsl2-docker-quickstart#docker-engine-docker-nativo-diretamente-instalado-no-wsl2)
- Docker Compose instalado no seu servidor.
    - [Tutorial da própria Docker.](https://docs.docker.com/compose/install/compose-plugin/#install-using-the-repository)
    - Também funciona no WSL2 seguindo o tutorial da CodeEdu.

## Organizando arquivos para o serviço do Ocomon
Requisitos atendidos, vamos colocar a mão na massa.

- Você deve criar uma pasta com o nome desejado, para organizar o seu serviço Docker do Ocomon. Exemplo:

    `mkdir ocomon && cd ocomon`

- Pode fazer um clone:

    `git clone https://github.com/romiof/ocomon-docker`

 - Dentro dessa pasta, devemos ter todos arquivos e pastas que estão nesse repositório.
    - [./docker-compose.yml](./docker-compose.yml) - contém os comandos para iniciar o ambiente dos containers, para o banco de dados e para o servidor web.
    - [./volumes](./volumes) - contém os diretórios que serão persistidos pelos containers, pois originalmente, o container destrói todos os dados que foram gerados dentro deles, a menos, que seja usado um *volume* para manter os dados após o container ter sido removido.
    Existe um diretório para os arquivos o banco de dados e outro para o servidor web.
    - [./src_www](./src_www) - contém o Dockerfile para gerar uma imagem docker customizada, usando como base a imagem oficial do [PHP8.1-Apache](https://hub.docker.com/_/php/tags?page=1&name=8.1-apache-bullseye). Isso é necessário, pois a imagem oficial não tem todos os módulos PHP que o Ocomon utiliza.

## Como subir o ambiente no Docker

1. No terminal, navegue até a raiz da pasta que foi criada anteriormente

2. Inicie o Docker Compose com o seguinte comando:

    `docker compose up -d`

3. Caso precise para todo o projeto do docker compose, use o comando abaixo:
    
    `docker compose down`

3. Caso precise fazer um novo build da imagem do PHP, pode ser usado esse comando:
    
    `docker compose up -d --build`

## Coonfigurações para o primeiro uso do Ocomon
- Configure uma senha inicial para o root do MySQL, editando a variável de ambiente *MYSQL_ROOT_PASSWORD* na linha 32 do arquivo [./docker-compose.yml](./docker-compose.yml)
    - Após iniciar o container do MySQL, todos os bancos de dados, incluíndo os bancos de dados do sistema, serão acessíveis pelo volume mapeado para o container.
    - Assim, após termos um banco de dados do sistema criado, não é possível trocar a senha pelo arquivo docker-compose.
    - Isso se deve ao fato que a imagem de container do MySQL está programada para não modificar os bancos de dados de sistema nos volumes mapeados. Ela apenas faz a criação dos bancos quando encontra a pasta vazia, mas não modifica o que já existir.
- Após o MySQL subir, [troque a senha do root](https://stackoverflow.com/a/62357102).
- Opcionalmente, libere acesso remoto do usuário root do MySQL. [Usei as etapas 2 e 3 desses passos com sucesso](https://stackoverflow.com/a/65636249).
- Com a nova senha de root, você pode fazer toda a gestão do banco pelo [DBeaver](https://dbeaver.io/download/), o qual eu acho muito mais fácil do que fazer pela shell do MySQL, de dentro do container.
- Feito, isso, descompacte o zip do Ocomon para o diretório [./volumes/www](./volumes/www).
- E agora siga as instruções do arquivo de instalação do Ocomon conforme sua versão anterior.
    - Se estiver fazendo uma instalação nova para atualizar um ambiente antigo, faça um DUMP e após um RESTORE do MySQL usando o DBeaver ou o shell do MySQL, após siga as instruções conforme a versão que era usada do Ocomon.

## Dicas Extra
- Toda edição dos arquivos e gestão do Linux e do Docker, pode ser feito com o [Visual Studio Code](https://code.visualstudio.com/)
- Para isso, instale as seguintes extensões no seu VS Code:
    - [Remote SSH](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh)
    - [Remote WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)
    - [Docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)
- Após conectar com sucesso no Remote SSH ou no Remote WSL, vá na página das extensões e habilite elas para o uso na conexão remota.
