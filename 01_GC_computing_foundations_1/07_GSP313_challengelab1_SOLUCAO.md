Essa é uma proposta de solução para o **Create and Manage Cloud Resources: Challenge Lab**

Dê preferencia a realizar o lab em inglês. O lab em português foi menos revisado e contém mais bugs.

# Task 1: Create a project jumphost instance

No console do Cloud, acesse o Menu de navegação (Ícone do menu de navegação) e clique em Compute Engine > VM Instances.
A primeira inicialização pode levar alguns instantes.
Para criar uma instância, clique em CREATE INSTANCE.
O nome da instância deve ser o nome dado pela própria task.
Não mexe na REGION, nem em ZONE.
Em SERIES, seleciona N1
Em MACHINE TYPE, f1-micro

Pronto. Agora é só clicar em CREATE.

# Task 2: Create a Kubernetes service cluster

```shell
gcloud container clusters create [NOME DA INTÂNCIA] \
          --num-nodes 1 \
          --network nucleus-vpc \
          --zone [ZONA DADA NA TASK 2]
```

```shell
gcloud container clusters get-credentials nucleus-backend \
          --zone [ZONA DADA NA TASK 2]
```

```shell
kubectl create deployment hello-server \
          --image=gcr.io/google-samples/hello-app:2.0
```

```shell
kubectl expose deployment hello-server \
          --type=LoadBalancer \
          --port [PORTA DADA NA TASK 2]
```

Task 3: Set up an HTTP load balancer
=============================================

Por via das dúvidas, prefira esse código diretamente do seu lab, ao invés de usar o que estou mostrando aqui.

```shell
cat << EOF > startup.sh
#! /bin/bash
apt-get update
apt-get install -y nginx
service nginx start
sed -i -- 's/nginx/Google Cloud Platform - '"\$HOSTNAME"'/' /var/www/html/index.nginx-debian.html
EOF
```

```shell
gcloud compute instance-templates create web-server-template \
          --metadata-from-file startup-script=startup.sh \
          --network nucleus-vpc \
          --machine-type g1-small \
          --region [REGIÃO DADA NA TASK 2, se a zona é us-west4-b, a região é us-west4]
```

```shell
gcloud compute instance-groups managed create web-server-group \
          --base-instance-name web-server \
          --size 2 \
          --template web-server-template \
          --zone [ZONA DADA NA TASK 2]
```

```shell
gcloud compute firewall-rules create [NOME DADO NA TASK 3] \
          --allow tcp:80 \
          --network nucleus-vpc
```

```shell
gcloud compute http-health-checks create http-basic-check
```

```shell
gcloud compute instance-groups managed \
          set-named-ports web-server-group \
          --named-ports http:80 \
          --zone [ZONA DADA NA TASK 2]
```

```shell
gcloud compute backend-services create web-server-backend \
          --protocol HTTP \
          --http-health-checks http-basic-check \
          --global
```

```shell
gcloud compute backend-services add-backend web-server-backend \
          --instance-group web-server-group \
          --instance-group-zone [ZONA DADA NA TASK 2] \
          --global
```

```shell
gcloud compute url-maps create web-server-map \
          --default-service web-server-backend
```

```shell
gcloud compute target-http-proxies create http-lb-proxy \
          --url-map web-server-map
```

```shell
gcloud compute forwarding-rules create http-content-rule \
        --global \
        --target-http-proxy http-lb-proxy \
        --ports 80
```
