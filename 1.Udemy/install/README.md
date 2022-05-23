# Rancher

<kbd>
    <img src="https://github.com/fabiokerber/Rancher/blob/main/1.Udemy/img/190520220826.png">
</kbd>
<br />
<br />

# Recursos #
**Rancher Master**<br>
vCPU: 4<br>
Ram: 4GB<br>
SO: Ubuntu Server 20.04.4 LTS<br>
<br />

**Rancher Node k8s1**<br>
vCPU: 4<br>
Ram: 2GB<br>
SO: Ubuntu Server 20.04.4 LTS<br>
<br />

**Rancher Node k8s2**<br>
vCPU: 4<br>
Ram: 2GB<br>
SO: Ubuntu Server 20.04.4 LTS<br>
<br />

**Rancher Node k8s3**<br>
vCPU: 4<br>
Ram: 2GB<br>
SO: Ubuntu Server 20.04.4 LTS<br>
<br />

# Ajustar fuso #
```
$ sudo timedatectl set-timezone America/Sao_Paulo
```

# Adicionar hostname no /etc/hosts #
```
$ sudo vim /etc/hosts
192.168.0.120 rancher-master
192.168.0.121 rancher-k8s1
192.168.0.122 rancher-k8s2
192.168.0.123 rancher-k8s3
```

# Instalar Docker no master e nodes #
```
$ sudo apt install -y docker.io && sudo systemctl start docker && sudo systemctl enable docker
```

# Instalar rancher no master (latest)#
```
$ sudo docker run -d --name rancher --restart=unless-stopped -v /opt/rancher:/var/lib/rancher -p 80:80 -p 443:443 --privileged rancher/rancher
```

# Anotar senha após finalizar deploy rancher #
```
$ sudo docker ps -a (anotar CONTAINER ID!)
$ sudo docker logs <CONTAINER ID> 2>&1 | grep "Bootstrap Password:"
```

# Acessar o Rancher v2 (e ativar com a senha coletada => v2.6.3+)#
```
https://192.168.0.120
```

# Criar cluster - Parte 1#
```
1. "Add Cluster"

2. "From existing nodes (Custom)"
    a. "Add Cluster - Custom"
        - Cluster Name: curso
    b. "Advanced Options"
        - Nginx Ingress (Disabled) < Será utilizado o Traefik

3. "Next"
```

# Criar cluster - Parte 2#
Nesta etapa deve-se selecionar os três checkbox para que os três **workers** possam ter o etcd e control plane.<br>
Após selecionado, click em **Show advanced options**.<br>
Preencha o campo **Node Name** com o nome do primeiro node e copie o código abaixo e execute-o no respectivo node.<br>
Faça isso para os três nodes alterando o campo **Node Name**, copiando o código e executando em cada node.<br>
```
1. Node Name: rancher-k8s1 | rancher-k8s2 | rancher-k8s3
    Obs: Adicionar primeiro e aguardar até que normalize a inclusão e API, então adicione o segundo e aguarde, etc...

2. Exemplo: 
    a. $ ... --node-name rancher-k8s1 ...
    b. $ ... --node-name rancher-k8s2 ...
    c. $ ... --node-name rancher-k8s3 ...

3. "Done"
```

# Nodes conectados #
<kbd>
    <img src="https://github.com/fabiokerber/Rancher/blob/main/1.Udemy/img/230520220838.png">
</kbd>
<br />
<br />


# Backup

----------------- Falhou devido ao NAT do Vagrant -----------------

# Iniciar Rancher Master & Nodes #
```
$ cd Rancher/1.Udemy/install
$ vagrant up
```

# Anotar Password (v2.6.3+)#
```
$ vagrant ssh rancher_master -c 'sudo docker ps -a' (anotar CONTAINER ID!)
$ vagrant ssh rancher_master -c 'sudo docker logs <CONTAINER ID> 2>&1 | grep "Bootstrap Password:"'
```

# Acessar o Rancher v2 (e ativar com a senha coletada => v2.6.3+)#
```
https://MASTER_IP
```

# Criar cluster - Parte 1#
```
https://MASTER_IP

1. "Add Cluster"

2. "From existing nodes (Custom)"
    a. "Add Cluster - Custom"
        - Cluster Name: curso
    b. "Advanced Options"
        - Nginx Ingress (Disabled) < Será utilizado o Traefik

3. "Next"
```

# Criar cluster - Parte 2#
Nesta etapa deve-se selecionar os três checkbox para que os três **workers** possam ter o etcd e control plane.<br>
Após selecionado, click em **Show advanced options**.<br>
Preencha o campo **Node Name** com o nome do primeiro node e copie o código abaixo e execute-o no respectivo node.<br>
Faça isso para os três nodes alterando o campo **Node Name**, copiando o código e executando em cada node.<br>
```
1. Node Name: rancher-k8s1 | rancher-k8s2 | rancher-k8s3
    Obs: Adicionar primeiro e aguardar até que normalize a inclusão e API, então adicione o segundo e aguarde, etc...

2. Exemplo: 
    a. $ vagrant ssh rancher_k8s1 -c '... --node-name rancher-k8s1 ...'
    b. $ vagrant ssh rancher_k8s2 -c '... --node-name rancher-k8s2 ...'
    c. $ vagrant ssh rancher_k8s3 -c '... --node-name rancher-k8s3 ...'

3. "Done"
```

----------------- Falhou devido ao NAT do Vagrant -----------------

# Adicionar k8s{1,2,3} ao master#
```
https://MASTER_IP
```

