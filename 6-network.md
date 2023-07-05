<h1>Network</h1>
<ul>
    <li>
        <a href="#tipo">
            Tipos de rede
        </a>
    </li>
        <a href="#ls">
            Listar as redes do docker
        </a>
    </li>
    <li>
        <a href="#prune">
            Remover todoas as networks que não estão sendo utilizada
        </a>
    </li>
    <li>
        <a href="#inspect">
            Inspecionar a network
        </a>
    </li>
    <li>
        <a href="#create">
            Criação de rede
        </a>
    </li>
    <li>
        <a href="#bridge_caso">
            Rede Bridge - problema resolução de nome na criação de container na rede default
        </a>
    </li>
    <li>
        <a href="#host_caso">
            Host - Acessando um ip local da máquina onde está instalado o docker de dentro do container
        </a>
    </li>
    <li>
        <a href="#run_network">
            Criação de container especificando uma network
        </a>
    </li>
    <li>
        <a href="#network_connect">
            Coneta um container em uma rede
        </a>
    </li>
</ul>


O objetivo da `network` é fazer com que um `container` se comunica com o outro.

<b id="tipo">Tipos de `network`</b>

- `bridge`: É utilizada para que um container se comunique com outro. Este tipo de rede é o padrão quando se cria um container se expecificar a rede
- `host`: O tipo `host` unir a rede do `docker` com a rede da máquina onde está instalada o `docker`.
- `overlay`: Não é um formato comum. A rede `overlay` serve para comunicar diversas redes de docker em máquina diferentes para que haja escalabilidade de um container que está rodando uma aplicação.
- `maclan`: Não é comum de se utiliza. O `maclan` server para colocar um endereço de MAC no container para que este se pareça com uma máquina que está conectada em uma rede.
- `none`: É define que o container não tenha rede, ou seja, isolar o container para não colocar em uma rede.

<b id="create">Criação de rede</b>

```
docker network create --driver bridge minharede
```


<b id="ls">Listar as redes do docker</b>

```
docker network ls
```

<b id="prune">Remover todas as networks que não estão sendo utilizada</b>

```
docker network prune
```

<b id="inspect">Inspecionar a network</b>

```
docker network inspect bridge
```
- `bridge` é o tipo de rede do docker que está sendo inspecionada.


<b id="bridge_caso">Rede Bridge - problema resolução de nome na criação de container na rede default</b>

- Quando os container são criado sem um `network` definida, um dos problema encontrado é a resolução de nome na rede dos container.

Para simular este comportamente. Vamos criar os container
```bash
# ubuntu1
docker run -d --name ubuntu1 1aquila1/ubuntu-ping bash

# ubuntu2
docker run -d --name ubuntu2 1aquila1/ubuntu-ping bash
```

Depois acesso o ubuntu1
```
docker exec -it ubuntu1 bash
```

Logando no `ubuntu1` execute o comando
```
ping ubuntu2
```

<b id="host_caso">Host - Acessando um ip local da máquina onde está instalado o docker de dentro do container</b>

Defina uma container na rede `host`
```
docker run --rm -it --network host  --name ub1 1aquila1/ubuntu bash
```

No linux acessa como `http://ip:porta`
```bash
# Acessando o nextjs
curl http://localhost:3000
```

No windows ou mac `http://host.docker.internal:portal`
```bash
#Acessando o nextjs
curl http://host.docker.internal:3000
```


<b id="host_caso2">Host - Acessando uma aplicação na porta padrão sem expor através do comando <code>-p</code></b>

Quando é criado um container com o tipo de rede `host` sem expecificar a porta a aplicação que está executando no container é acessado.

```
docker run -d --name ng1 --network host nginx
```
Agora é acessa a página inicial do nginx na porta padrão
```
curl http://localhost
```


<b id="run_network">Criação de container especificando uma network</b>

```
docker run -it --name ubuntu1 --network minharede ubuntu bash
```
- Em `minharede`, pode ser qualquer uma disponível no drive de rede do `docker`. 

<b id="network_connect">Coneta um container em uma rede</b>

```
docker network connect nome_da_rede nome_do_container
```
Exemplo
```
docker network connect minharede ubuntu3
```
