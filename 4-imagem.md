<h1>Índice</h1>
<ul>
    <li>
        <a href="#pull">
            Download de imagem
        </a>
    </li>
    <li>
        <a href="#images">
            Listar as imgens disponível na máquina locals
        </a>
    </li>
    <li>
        <a href="#rmi">
           Remover uma imagem
        </a>
    </li>
</ul>

<h3 id="pull">Download de uma imagem</h3>

O comando `pull` é utilizado para fazer o download de uma imagem do `docker hub`.
```
docker pull ubuntu
```
Quando não é informado a `tag` ou a `versão` da imagem, o `docker` sempre faz o download a ultima versão que é a `lastest`.

Download de uma imagem com versão
```
docker pull nginx:stable-alpine3.17-perl
```

<h3 id="images">
Listar as imgens disponível na máquina local
</h3>

```
docker images
```

<h3 id="rmi">Remover uma imagem</h3>

O comando `rmi` é utilizando para remover uma imagem.
```
docker rmi nginx:stable-alpine3.17-perl
```

<h3>Upload de imagem para docker hub</b>

- Primeiro passo é fazer o login na conta do docker hub pelo terminal
```
docker login
```

- Fazer o push
```
docker push usuario/nome_img
```
Exemplo:
```
docker push usuario/nginx-siscad
```

<i style="color: red">
Obs: Depois de um tempo o docker hub excluir a imagem da plataforma, caso a imagem não seja utilizada.
</i>

- Deslogar do docker hub pelo terminal
```
docker logout
```

