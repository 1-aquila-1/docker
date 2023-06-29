<h1>
Introdução
</h1>

<b>ps</b>

O comando `ps` é utilizado para listar os container em execução

```
docker ps
```

<b>ps -a</b>

O comando `ps -a` é utilizado para mostra os containers que estão em execução ou parado.

```
docker ps -a
```

<h3>Exibe somente os ID dos container</h3>

O comando `ps -a -q` exibe somente os ID dos container parado ou ativos.
```
docker ps -a -q
```

<b>run</b>

O comano `run` é utilizando para criar um container. Quando não é colocado a versão da imagem, o `docker` entende que é a última versão que será utilizada, a `latest`.

Quando a imagem não está na maquina local, o download da mesma é feita automaticamente

<i>É gerado um nome para o container quando no momento da execução não é definido</i>

```
docker run hello-world
```
- `hello-worl` é a imagem para criação do container.

<!--  -->

<b>run -it</b>

O comando `-it` significa que tem passagem de comando, após o nome da imagem.

```
docker run -it ubuntu bash
```

O comando `-it` é o mesmo que `-i -t`. Sendo que

<code>-i</code> significa executar o terminal no modo interativo. No caso do <i>bash</i> vai colocar o terminal atachado com o terminal do container para uso.

<code>-t</code> É para liberar o terminal para digitação

<!--  -->

<b id="rm" >run -it --rm</b> <i>nome_img comand</i>

O comando <code>docker run -it --rm</code> é utilizado para criar um container que após ser finalizado é excluido do docker.

```
docker run -it --rm ubuntu bash
```
O comando <code>--rm</code> é o que faz a remoção do container após a finalização.

<!--  -->

<b>--name</b>

O comando `--name` é utilizado par dar um nome para o container no momento da criação.
```
docker run --name nginx nginx
```

<b id="start">start</b>

O comando <code>docker start</code> é utilizado para executar um container que está parado.

Executando o container pelo `NAME`
```
docker start jovial_gould
```
Executando o container pelo `CONTAINER ID`
```
docker start 8a0ef1dda140
```

<h2>Publicando portas</h2>

Vamos trabalhar neste exemplo com <code>nginx</code>.

Ao criamos um container com o <code>nginx</code> a porta que é exposta no <code>docker</code> é a <code>80</code>.

```
docker run nginx
```

<i>Uma porta no <code>host</code> do <code>docker</code> não significa que ela será exposta na maquina que está sendo executado do docker.</i>

<b>-p</b>

O comando <code>-p</code> associa uma porta da maquina local com a rede do docker.

```
docker run -d -p 8080:80 nginx
```

No exemplo acima, a porta <code>8080</code> é da maquina local e a porta <code>80</code> é do docker.

<b>-d</b>

O comando <code>-d</code> libera o terminal do container em execução.

```
docker run -d nginx
```

<h2>Removendo um container</h2>

<b>stop</b>

O comando `stop` é utilizado para parar a execução de um container.

Parando o container pelo `CONTAINER ID`
```
docker stop 2e34er22
```

Parando o container pelo `NAME
```
docker stop competent_lumiere
```
<!--  -->

<b>rm</b>

O comando `rm` é utilizado para remover ou excluir um container do docker.

Removendo pelo `CONTAINER ID`
```
docker rm 13ec0ccb141f
```

Removendo pelo `NAME`
```
docker rm tender_chaum
```
<!--  -->

<b>-f</b>

O comando é utilizando para excluir um container em execução.
```
docker rm -f 81bfa990e643
```
<b>Remover mas de um container</b>

Podemos combinar comandos para remover os container ativo ou parado.
```
docker rm $(docker ps -a -q) -f
```
O comando `docker ps -a -q` retorna somente os ID dos container.

<!--  -->

<h2>Acessando e alterando arquivos no container</h2>

<b>exec</b>

O comando `exec` executa comando ou instrução dentro do container.
```
docker exec nome_ou_id_container comando_ou_instrução
```
Executando o comando `ls` no container em execução com o `NAME` ng1.
```
docker exec ng1 ls
```

Para executar o `bash` e utilizar o terminal, é preciso utilizar o comando `-it`.
```
docker exec -it ng1 bash
```
<h2>Bind Mounts</h2>

O `bind mounts` é trabalhar com pasta compartilhada da maquina local com o container.

<b>-v</b>

<i>Maneira antiga de se fazer o `bind mounts`</i>

Define o compartilhamente de arquivo da maquina local com o container e vise-e-vesa.
```
docker run -d --name nginx -p 8080:80 -v ~/volume/v1/:/usr/share/nginx/html nginx
```
No código acima, estamos montando um volume na maquina local cujo o caminho é `~/volume/v1` e associando com uma pasta no container localizada em `/usr/share/nginx/html`

<i style="color:red">
Ao utiliza o -v para criar o volume de compartilhamento, se a pasta não existe é criada.
</i>

<h3>
A maneira atual de fazer o <code>bind mounts</code> e a recomendada.
</h3>

<b>--mount</b>

```
docker run -d --name nginx -p 8080:80 --mount type=bind,source=/home/aquila/volume/v1,target=/usr/share/nginx/html nginx
```
- `source` é onde é colocado o caminho da pasta na maquina local
- `target` é onde é colocado o caminho da pasta no container
- O `type` tem que ser definido como `bind`

<b>Utilizando o volume em um container</b>

Com o comando `--mount` podemos adicionar o volume na criação de um container
```
docker run -d --name nginx -p 8080:80 --mount type=volume,source=meuvolume,target=/app nginx
```
- No `type` é definido como `volume`
- O `source` coloca o nome do volume existente no `docker host`
- E o `target` é definido o caminho da pasta no container, e caso ela não existe é criado.

<b>Definido o volume com o comando -v</b>
```
docker run --name ng3 -d -v meuvolume:/app nginx
```