# 2.1 Preparação do Ambiente
Preparação dos Hosts do ambiente.
## Topologia
A Topogia mostrada na figura: Hosts do Ambiente, detalha cada nó da nossa rede.
A Sub-rede adotada neste ambiente, é uma rede privada 192.168.188.0/24. Cada host, tem seu endereço ip mostrado juntamente com a figura.

## Funções:

Ansible Control Host: Máquina de controle do Ansible Engine. Aqui estarão todos os recursos necessários para implantação da nossa infraestrutura de automação.

node1: Máquina responsável por executar o servidor web com aplicação real.
node2: Máquina responsavel pelos serviços de DNS, NTP e File Server.
node3: Servidor de Banco de Dados

​

## Role

Inventory Name

Ansible Control Host
ansible
managed host 1
node1
managed host 2
node2
managed host 3
node3

**Todas as maquinas estão executando CentOS versão 7.**

## Host Templates
Assumindo que todos os hosts estão configurados com imagem padrão, é necessário configurar os recursos de rede e hosts e pacotes necessarios para instalação do Ansible Engine.
Preparando a acessibilidade.

Para deixar o ambiente todo acessível para a máquina bastion, devemos configurar os seguintes passos:

Passo 1: Configurar o arquivo /etc/hosts do bastion

Obs: Caso a subrede dos seus hosts seja diferente de 192.168.188.0/24, altere conforme o seu cenário.

Passo 2: Gerar a chave SSH no para permitir a maquina bastion acessar todos os hosts. Ca CLI do bastion execute:

ssh-keygen -t rsa -b 2048

ssh-keygen -t rsa -b 2048
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa): lab_rsa_key
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in lab_rsa_key.
Your public key has been saved in lab_rsa_key.pub.
The key fingerprint is:
SHA256:uUWjJNUurqi6BwcVKTeXyHVWGLBhIFcEzkXlpYfIWeY root@bastion.mylab.local
The key's randomart image is:
+---[RSA 2048]----+
|..=@X+B++.       |
|.=B+o%.=  .      |
| +oo= E o.o      |
|.      +.+..     |
| .     .S..      |
|. .     .o       |
| o   . ..        |
|  . . .          |
|o+..             |
+----[SHA256]-----+
​
Passo 3: Criar um arquivo no diretório padrão do usuário root de lista de hosts

cat << EOF> hosts.txt
webserver
netserver
dbserver
shipserver
EOF
Passo 4: Copiar a chave pública para os demais hosts com o seguinte comando:

 for i in $(cat hosts); do ssh-copy-id -i /root/.ssh/id_rsa root@$i; sleep 1; done
​
