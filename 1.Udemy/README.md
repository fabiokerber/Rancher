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

### Ajustar fuso 
```
$ sudo timedatectl set-timezone America/Sao_Paulo
```

### Adicionar hostnames no /etc/hosts
```
$ sudo vim /etc/hosts
192.168.0.120 rancher-master
192.168.0.121 rancher-k8s1
192.168.0.122 rancher-k8s2
192.168.0.123 rancher-k8s3
```

### Instalar Docker no master e nodes
```
$ sudo apt install -y docker.io && sudo systemctl start docker && sudo systemctl enable docker
```

### Instalar rancher no master (latest)
```
$ sudo docker run -d --name rancher --restart=unless-stopped -v /opt/rancher:/var/lib/rancher -p 80:80 -p 443:443 --privileged rancher/rancher:v2.4.3
```

### Anotar senha após finalizar deploy rancher
```
$ sudo docker ps -a (anotar CONTAINER ID!)
$ sudo docker logs <CONTAINER ID> 2>&1 | grep "Bootstrap Password:"
```

### Acessar o Rancher v2 (e ativar com a senha coletada => v2.6.3+)
```
https://192.168.0.120
```

### Criar cluster - Parte 1
```
1. "Add Cluster"

2. "From existing nodes (Custom)"
    a. "Add Cluster - Custom"
        - Cluster Name: curso
    b. "Advanced Options"
        - Nginx Ingress (Disabled) < Será utilizado o Traefik

3. "Next"
```

### Criar cluster - Parte 2
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

### Nodes conectados
<kbd>
    <img src="https://github.com/fabiokerber/Rancher/blob/main/1.Udemy/img/230520220838.png">
</kbd>
<br />
<br />

### Instalar kubectl no master 
```
$ sudo apt update && sudo apt install -y apt-transport-https gnupg2
$ curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
$ echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
$ sudo apt update && sudo apt install -y kubectl
```

### Configurar kube/config para administrar o cluster (rancher-master)
Obs: Print para a versão 2.6.4 do rancher.<br> 

<kbd>
    <img src="https://github.com/fabiokerber/Rancher/blob/main/1.Udemy/img/230520221037.png">
</kbd>
<br />
<br />

```
$ mkdir ~/.kube && vi ~/.kube/config (paste kubeconfig)
$ kubectl get pods -n kube-system
$ kubectl get nodes
```

### Criando Persistent Volumes (Longhorn)
Sistema de volumes utilizado em Cluster específico para containers.<br>
É criado um volume no master e quando um pode sobe, conecta-se o volume aos containers.<br>
Instalar o App *Longhorn* acessando o cluster curso > Default > Apps > Launch.<br>

### Deploy do Graylog+Elasticsearch para visualização centralizada dos logs

### Habilitar o monitoramento do cluster Grafana+Prometheus
Acessar o cluster > Tools > Monitoring.

### Cronjob
Cronjob agendado geralmente utilizado para realizar backup de banco, por exemplo.

### ConfigMap
Pode ser utilizado por exemplo para armazenamento de valores de variáveis e quando o pode sobe, ele visualiza essas variáveis.<br>
Exemplo as variaveis ao invés de ficarem dentro do pod, são lidas externamente, o que facilita na hora de atualizá-las para um ou mais pods.

### Secrets
Equivalente ao ConfigMap, mas necessário para armazenar dados sensíveis.<br>

### Liveness
HeathCheck de aplicações.<br>

### RollingUpdate/SetImage
Atualização de imagem em etapas.<br>

### AutoScalling
Definir máximo de pods á serem "Deployados" com base no consumo de CPU.

### Node Selector
É possível setar labels a um determinado deploy para que o pod seja "deployado" sempre num mesmo node, por exemplo.
Ou caso um determinado Node tenha discos mais rápidos ou rede com maior velocidade, pode-se dar preferência para que um determinado pod seja executado naquele node.

### Pipeline com Rancher interno, conectando ao Github

### Kubeless
Serverless, equivalente ao Azure Functions.

### Helm
Gestor de aplicações do Kubernetes.<br>
```
$ curl -LO https://git.io/get_helm.sh
$ chmod 700 get_helm.sh
$ ./get_helm.sh
$ helm init
$ helm init --upgrade

$ kubectl create serviceaccount --namespace kube-system tiller
$ kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller
$ kubectl patch deploy --namespace kube-system tiller-deploy -p '{"spec":{"template":{"spec":{"serviceAccount":"tiller"}}}}'

$ helm search
$ helm repo update
$ helm install stable/redis
```

### Declarar políticas de rede

https://kubernetes.io/docs/tasks/administer-cluster/declare-network-policy/

https://github.com/ahmetb/kubernetes-network-policy-recipes/

https://ahmet.im/blog/kubernetes-network-policy/



# Backup
----------------- Falhou devido a configuração DNS -----------------

# Traefik (rancher-master)#
```
$ kubectl apply -f https://raw.githubusercontent.com/fabiokerber/Rancher/main/1.Udemy/traefik/traefik-rbac.yaml
$ kubectl apply -f https://raw.githubusercontent.com/fabiokerber/Rancher/main/1.Udemy/traefik/traefik-ds.yaml
$ kubectl --namespace=kube-system get pods
```

# Configuração DNS (traefik) #
```
$ wget https://raw.githubusercontent.com/fabiokerber/Rancher/main/1.Udemy/traefik/ui.yml

$ vi ui.yml
- host: traefik.rancher.kerberos.com

$ kubectl apply -f ui.yml
```

-------------------------------------------------------------------

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

-------------------------------------------------------------------

# Adicionar k8s{1,2,3} ao master#
```
https://MASTER_IP
```

