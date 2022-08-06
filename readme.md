# Guia de uso do Ocomon v4 em ambiente Docker

Esse guia tem a finalidade de explicar forma bem enxuta, como subir um ambiente docker para uso em produção, do sistema de chamados [Ocomon](https://ocomonphp.sourceforge.io/), na versão 4.

## Requisitos
- Docker CE instalado no seu servidor
    - Utilei com sucesso o Ubuntu Server 22.04 LTS com a versão mais recente do Docker.
    - [Digital Ocean ensina passo à passo a instalação do Docker.](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-22-04)
    - Pode rodar para debug e testes no Windows, usando o WSL2. [Tutorial da CodeEdu direto ao ponto aqui.](https://github.com/codeedu/wsl2-docker-quickstart#docker-engine-docker-nativo-diretamente-instalado-no-wsl2)

- Docker Compose instalado no seu servidor
    - [Tutorial da própria Docker](https://docs.docker.com/compose/install/compose-plugin/#install-using-the-repository)
    - Também funciona no WSL2 seguindo o tutorial da CodeEdu.

## Organizando arquivos para o serviço do Ocomon
Requisitos atendidos, vamos colocar a mão na massa.

Você deve criar uma pasta com o nome desejado, para organizar o seu serviço Docker do Ocomon.
Dentro dessa pasta, devemos ter todos arquivos e pastas que estão nesse repositório.

- [./docker-compose.yml](./docker-compose.yml) - contém os comandos para iniciar o ambiente dos containers, para o banco de dados e para o servidor web.
- [./volumes](./volumes) - contém os diretórios que serão persistidos pelos containers, pois originalmente, o container destrói todos os dados que foram gerados dentro deles, a menos, que seja usado um "*volume*" para manter os dados após o container ter sido removido.
Existe um diretório para os arquivos o banco de dados e outro para o servidor web.
- [./src_www](./src_www) - contém o Dockerfile para gerar uma imagem docker customizada, usando como base a imagem oficial do [PHP8.1-Apache](https://hub.docker.com/_/php/tags?page=1&name=8.1-apache-bullseye). Isso é necessário, pois a imagem oficial não tem todos os módulos PHP que o Ocomon utiliza.

## Como subir o ambiente no Docker

