# Instalação o docker no ubuntu

```
sudo apt update && sudo apt upgrade
```
```
sudo apt remove docker docker-engine docker.io containerd runc
```
```
sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release
```

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

```
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Instale o Docker Engine
```
sudo apt-get update
```
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

Dê permissão para rodar o Docker com seu usuário corrente:
```
sudo usermod -aG docker $USER
```

Inicie o serviço do Docker:
```
sudo service docker start
```