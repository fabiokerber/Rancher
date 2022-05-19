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
<br />

**Rancher Node k8s1**<br>
vCPU: 4<br>
Ram: 2GB<br>
<br />

**Rancher Node k8s2**<br>
vCPU: 4<br>
Ram: 2GB<br>
<br />

**Rancher Node k8s3**<br>
vCPU: 4<br>
Ram: 2GB<br>

# Iniciar Rancher Master & Nodes #
```
$ cd Rancher/1.Udemy/install
$ vagrant up
```

# Anotar Password (v2.6.3+)#
```
$ vagrant ssh rancher_master -c 'sudo docker ps -a' (anotar CONTAINER ID!)
$ vagrant ssh rancher_master -c 'sudo docker logs container-id 2>&1 | grep "Bootstrap Password:"' (substituir container-id!)
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
1. Node Name: rancher-k8s1

2. Exemplo: 
    a. $ vagrant ssh rancher_master -c 'sudo docker run -d --privileged --restart=unless-stopped --net=host -v /etc/kubernetes:/etc/kubernetes -v /var/run:/var/run rancher/rancher-agent:v2.4.3 --server https://192.168.56.100 --token xl69vs252wv6p4bgbcdntjd9ntlh2f46xzgk7bskfgs82d5zj46z2b --ca-checksum 378d4885291130a42df99f81b3af1ccafee2368d15d2579340bc5ceba6eb3a5f --node-name rancher-k8s1 --etcd --controlplane --worker'
```

# Adicionar k8s{1,2,3} ao master#
```
https://MASTER_IP
```