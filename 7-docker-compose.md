<h1>Docker Compose</h1>

<ul>
    <li>
        <a href="#services">Definido um serviço</a>
    </li>
    <li>
        <a href="#up">up - colocar em execução dos container definido no docker-compose</a>
    </li>
    <li>
        <a href="#down">Down - Remove os container</a>
    <li>
    <li>
        <a href="#stop-v">stop - parando somente um container que está em execução do docker-compose</a>
    </li>
</ul>
<br/>

<b id="services">services - Definido um serviço</b>

- `services` seção que define os serviços que serão criados.
- `laravel` é o nome do serviço, mas pode ser qualquer nome
- `build` A tag build é opcional mas serve como uma alternativa para build das imagens do docker-compose
- `image` tag que define o nome da imagem do container
- `command` (opcional) nesta tag colocamos comandos que serão executados na criação do container.
- `container_name` tag que define o nome do container que será criado
- `restart` (opcional) recebe valor como `always`. E o `always` serve para restart do container de forma automatica caso ele pare
`tty` (opcional) como `true` habilita o container para permitir acesso no modo interativo
- `entrypoint` (opcional) utilizado para alterar a execução do entrypoint da imagem
- `depends_on` (opcional) tag onde podemos indica se o container ele depende de outro para ser executado.
- `volumes` (opcional) define o(s) volume(s) que o container vai utilizar
- `environment` (opcional) Define as variáveis de ambiente do container
- `networks` (opcional) tag que define a rede que o container irá utiliza.

<b>ports</b>

A tag `ports` que define a porta que será exposta pelo container. Ela é opcional. E só é utilizada para expor portas para ser acessado de forma externa, isso, quando a rede não é do tipo `host`.

Quanto é definido `- "80"` significa que o container está ouvido na porta `80` e sendo exposta também na porta `80`. Mas quando `- "8080:80` significa que o container está recebendo requisição na porta `8080` e o docker redireciona a requisição para porta `80` que é a porta interna que o container está ouvindo.

Exemplo:

```
version: '3'
services:
    laravel:
        build:
            context: ./laravel
            dockerfile: Dockerfile.prod
        image: 1aquila1/laravel:prod
        command: (comando)
        container_name: laravel
        restart: valor
        tty: true
        entrypoint: valor
        depends_on:
            - nome_container
        volumes:
            - diretorio_local:/diretorio_container
        environment:
            - NOME=valor
        networks:
            - minharede
        ports:
            - "8080:80"
```

<b id="networks">networks - definido a rede que os containers vão utilizar</b>

A seção `networks` é a seção onde são definida as redes que os container vão utiliza.
- `networks` é a tag que defini a seção de networks do docker-compose
- `minharede` é o nome da rede que será criada e pode ser qualquer um nome
- `driver` tag que define o `driver` de rede como `bridge` ou `host` etc.

```
networks:
    minharede:
        driver: bridge
```

<b id="build">Buildando imagens</b>

Na sessão de `build` são definido o dockerfile que será buildado para gerar a imagem do container. Nesta sessão são indicado dos parâmetro.
- `context` define o caminho do dockerfile
- `dockerfile` onde é definido o nome do dockerfile da imagem.

No momento do `build` do docker vai utilizar o nome definido na tag `image` para utiliza no nome da imagem. Caso contrário é definido um nome aleatório.

```
services:
  laravel:
    build:
      context: ./
      dockerfile: Dockerfile.laravel.prod
    image: 1aquila1/laravel:prod
    container_name: laravel

```

Por padrão ao executar os container o docker não build as imagem. Mas quando houver modificação no dockerfile precisamos buildar as imagens, é utilizando o comando `--build`:
```
docker-compose up -d --build
```

<b id="volumes">Volumes - Definindo volume no container</b>

Para definir o volume no container é utilizado a tag `volumes`.

```
services:
    volumes:
      - diretorio_local:/diretorio_container
```
- `diretorio_local` é o caminho da máquina loca para armazenar os aquivos que serão gerado ou modificado no container
- `diretorio_container` é o caminho do diretório no container

Exemplo: `db` vai compartilhar os mesmos arquivos em `/var/lib/mysql`

```
services:
    volumes:
      - ./db:/var/lib/mysql
```

<b>Dependência entre container</b>

Soluções que resolve a dependência entre container
- dockerize
- Docker wait-for-it

Exemplo da aplicação `wait-for-it`
```
https://github.com/codeedu/docker-wait-for-it
```

Uma das soluções para resolver as dependência entre os container é através do `dockerize`.

- github do dockeriza

```
https://github.com/jwilder/dockerize
```

Devemos configurar o `dockerize` no `dockerfile` da imagem que será criado o container que depende de outro para ser executado.

- Ubuntu Imagens (código do github do dockerize)

```
ENV DOCKERIZE_VERSION v0.7.0

RUN apt-get update \
    && apt-get install -y wget \
    && wget -O - https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz | tar xzf - -C /usr/local/bin \
    && apt-get autoremove -yqq --purge wget && rm -rf /var/lib/apt/lists/*
```

No `docker compose` precisa adicionad a tag `depends_on` e `entrypoint`.

Depois, podemos colocar o comando ante do arquivo de `entrypoint` da imagem.

```
entrypoint: dockerize -wait tcp://db:3306 -timeout 20s docker-entrypoint.sh
```
- O arquivo `docker-entrypoint.sh` é o arquivo padrão da imagem que neste caso é do node. Dependendo da imagem pode ser outro.


<h3>Comandos</h3>

<b id="up">up - colocar em execução dos container definido no docker-compose</b>

```
docker-compose up
```

<b id="stop-v">stop - parando somente um container que está em execução do docker-compose</b>

```
docker-compose stop nome_container
```
Exemplo:
```
docker-compose stop db
```

<b id="down">Down - Remove os container</b>

```
docker compose down
```

<b id="d">(d) Liberar o terminal no momento da execução do up</b>

```
docker-compose up -d
```

<b id="ps"> (ps) Lista os container em execução do docker compose</b>

```
docker-compose ps
```

<h3>Dockerize</h3>

<b id="wait">Dockerize - testando se um serviço está disponível</b>

Rodando com o timeout padrão
```
dockerize -wait tcp://db:3306
```
- db é o serviço do MySQL

Definindo um timeout
```
dockerize -wait tcp://db:3306 -timeout 50s
```
- 50s é a quantidade de tempo que o serviço ficará esperado. Mas pode ser qualquer valor