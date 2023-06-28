<h1>Volme</h1>

Os volumes são uma parte específica do `docker machine` e servem para compartilhamento de arquivos entres os container e outras funções.

<h2>Criação</h2>

O comando `create` é utilizado para criar um novo volume no `docker machine`.
```
docker volume create meuvolume
```
O nome `meuvolume` é o nome do `volume` que será criado no `docker machine`.

<h2>Listar</h2>

Para listar os volumes existente no `docker machine` é utilizado o comando `ls`.
```
docker volume ls
```

<h2>Inspecionar</h2>

O comando `inspect` é utilizando quando queremos  ver informações sobre o volume como: data de criação, tipo etc.
```
docker volume inspect meuvolume
```