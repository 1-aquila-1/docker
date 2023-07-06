<h1>Dockerfile</h1>

<ul>
    <li>
        <a href="#obs">
            Observação
        </a>
    </li>
    <li>
        <a href="#from">
            Imagem base
        </a>
    </li>
    <li>
        <a href="#run">
            Executar comando para instalação de dependência na imagem
        </a>
    </li>
    <li>
        <a href="#workdir">
            Criação de diretório e onde o terminal ficará aberto na execução do bash
        </a>
    </li>
    <li>
        <a href="#copy">
            Copiar conteúdo de uma pasta da maquila local para dentro de uma pasta da imagem
        </a>
    </li>
    <li>
        <a href="#build">
            Build - Gerar uma nova imagem
        </a>
    </li>
    <li>
        <a href="#expose">
            Expose - Expor uma porta padrão nos container criado
        </a>
    </li>
</ul>

<h3 id="obs">Observação</h3>

- Sempre quando for gerado uma imagem de uma aplicação...precisa copiar os arquivo para dentro da imagem.

<h3 id="from">Imagem base</h3>
<b>FROM</b>

A tag `FROM` indica qual imagem base será utilizada para criação da nova imagem.
```
FROM nginx:latest
```
<h3 id="run">Executar comando para instalação de dependência na imagem</h3>
<b>RUN</b>

A tag `RUN` serve para rodar comando para instalação de dependência da aplicação para sua execução.

Um ponto importante é que cada `RUN` precisa ser criado em um contexto para que o `docker` possa organizar as camada da imagem e com isso se houver alteração possa se alterado somente naquela camada do contexto.

```
RUN apt-get update
```
Sendo que a cada nova instrução de comando é adicionado uma nova tag `RUN`.
```
RUN apt-get update
RUN apt-get install vim -y
```

Outra forma de executar diversos comandos é utilizar o `&&` com o `\`.

A contra-barra `\` significa que ele vai começar na próxima linha abaixo.
```
RUN apt-get update && \
    apt-get install -y
```

<h3 id="workdir">Criação de diretório e onde o terminal ficará aberto na execução do bash</h3>

<b>WORKDIR</b>

O camando `WORKDIR` é utilizando para criar um diretório dentro do container e também onde terminal ficará aberto no bash ou será executado os camandos definido no `RUN`.
```
WORKDIR /app
```
O diretório `/app` caso não existe será criado.

<h3 id="copy">Copiar conteúdo de uma pasta da maquila local para dentro de uma pasta da imagem</h3>

<b>COPY</b>

```
COPY sis-cad/ /usr/share/nginx/html
```

- A pasta `sis-cad` acima é o diretório da máquina local.

Quanto está gerando uma imagem de determinada aplicação, podemos usa no comando `COPY` este atalho.
```
COPY . .
```

Este (`. .`) ele vai copiar os arquivos do diretório atual para o diretório setado no `WORKDIR`.

<h3 id="build">Build - Gerar uma nova imagem</h3>

O comando `build` é utilizado para gerar uma imagem a parte do `Dockerfile`.
```
docker build -t 1aquila1/nginx-com-vim:latest .
```
- O parâmetro `-t` é para definir o nome da imagem. Sempre segue este padrão: `usuario/nome_img:versão`
- O ponto `.` no final da instrução acima é para dizer que o arquivo `Dockerfile` está no mesmo diretório onde está sendo executado o camando, caso o contrario é colocando o caminho.

Build de uma imagem com nome diferente de Dockerfile.

```
docker build -t usuario/nomeimage:versao . -f Dockerfile.nome
```
Quando se quer dar outro nome para `Dockerfile` precisa ser assim:

```
Dockerfile.nome_que_se_deseja
```
Exeplo:
```
Dockerfile.prod
```

<h3>Entrypoint vs CMD</h3>

<b>CMD</b>

A tag `CMD` é utilizado escrever algo na tela e é utilizado também para que no momento da criação do container possamos executar outros comando que substituirá o que foi definido na imagem.

Na imagem é definido no `CMD` os seguinte comando
```
CMD ["echo", "Hello world"]
```

Se criamos um container. O comando definido no `CMD` vai ser executado.

Mas no momento de criamos o container e deseamos mudar o comando que será executado na tag `CMD`, isso é possivel. O docker substitui o comando adicionado no final

Primeiro exemplo
```
docker run --rm usuario/hello echo "oi"
```
Segundo exemplo
```
docker run --rm usuario/hello bash
```

<b>ENTRYPOINT</b>

A tag `entrypoint` o comando definido nela é permanente e não será mudado durante a criação do container.

```
ENTREYPOINT ["echo", "Hello"]
```

<i style="color: red">Quando a tag <code>ENTRYPOINT</code> e <code>CMD</code> estão definida no mesmo <code>Dockerfile</code>, o <code>CMD</code> entra como um parâmetro para o <code>ENTRYPOINT</code></i>

Enquando o `ENTRYPOINT` é fixo o `CMD` é variável, pois, pode ser mudando durante a criação do container.

<b>Processo para ver a definição de uma imagem</b>

Um ponto importante é que podemos ver como as imagens base que utilizamos para criar outras foi criada. Quando existe algum `script sh` no `ENTRYPOINT` na definição da imagem. Um ponto importante sobre o `script` é se tem no final dele o comando `exec "@"` que significa que o `script` aceita outros comando para ser executados depois dele, ou seja, do `ENTRYPOINT` no caso o `CMD`.

<h3 id="expose">
Expose - Expor uma porta padrão nos container criado
</h3>

```
EXPOSE N_PORTA
```
Exemplo:
```
EXPOSE 3000
```



