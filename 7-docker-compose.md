<h1>Docker Compose</h1>

<ul>
    <li>
        <a href="#services">Definido um serviço</a>
    </li>
</ul>
<br/>

<b id="services">services - Definido um serviço</b>
- `services` seção que define os serviços que serão criados.
- `image` tag que define o nome da imagem do container
- `container_name` tag que define o nome do container que será criado
- `networks` tag que define a rede que o container irá utiliza.

<b>ports</b>

A tag `ports` que define a porta que será exposta pelo container. Ela é opcional. E só é utilizada para expor portas para ser acessado de forma externa, isso, quando a rede não é do tipo `host`.

Quanto é definido `- "80"` significa que o container está ouvido na porta `80` e sendo exposta também na porta `80`. Mas quando `- "8080:80` significa que o container está recebendo requisição na porta `8080` e o docker redireciona a requisição para porta `80` que é a porta interna que o container está ouvindo.

Exemplo:

```
version: '3'
services:
    laravel:
        image: 1aquila1/laravel:prod
        container_name: laravel
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


<h3>Comandos</h3>

<b id="up">up - colocar em execução dos container definido no docker-compose</b>

```
docker-compose up
```