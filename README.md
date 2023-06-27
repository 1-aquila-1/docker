# Instalação o docker no ubuntu

<pre>
    <code>
sudo apt update && sudo apt upgrade
sudo apt remove docker docker-engine docker.io containerd runc
sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release
    </code>
</pre>

<pre>
    <code>
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    </code>
</pre>

<div>
Instale o Docker Engine
    <pre>
        <code>
sudo apt-get update
        </code>
    </pre>
    <pre>
        <code>
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
        </code>
    </pre>
</div>

<div>
Dê permissão para rodar o Docker com seu usuário corrente:
    <pre>
        <code>
sudo usermod -aG docker $USER
        </code>
    </pre>
</div>

<div>
Inicie o serviço do Docker:
    <pre>
        <code>
sudo service docker start
        </code>
    </pre>
</div>


---







