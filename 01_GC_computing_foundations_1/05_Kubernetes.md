Kubernetes Engine: Qwik Start
=============================

 **Menu de navegação** no canto superior esquerdo. ![Ícone do menu de navegação](https://cdn.qwiklabs.com/9vT7xPlxoNP%2FPsK0J8j0ZPFB4HnnpaIJVCDByaBrSHg%3D)

### Ative o Google Cloud Shell

O Google Cloud Shell é uma máquina virtual com ferramentas de desenvolvimento. Ele conta com um diretório principal permanente de 5 GB e é executado no Google Cloud. O Google Cloud Shell permite acesso de linha de comando aos seus recursos do GCP.

1. No Console do GCP, na barra de ferramentas superior direita, clique no botão **Abrir o Cloud Shell**.
   
   ![Ícone do Cloud Shell](https://cdn.qwiklabs.com/vdY5e%2Fan9ZGXw5a%2FZMb1agpXhRGozsOadHURcR8thAQ%3D)

2. Clique em **Continue** (continuar):
   
   ![cloudshell_continue](https://cdn.qwiklabs.com/lr3PBRjWIrJ%2BMQnE8kCkOnRQQVgJnWSg4UWk16f0s%2FA%3D)

Demora alguns minutos para provisionar e conectar-se ao ambiente. Quando você está conectado, você já está autenticado e o projeto é definido como seu **PROJECT_ID** . Por exemplo:

![Terminal do Cloud Shell](https://cdn.qwiklabs.com/hmMK0W41Txk%2B20bQyuDP9g60vCdBajIS%2B52iI2f4bYk%3D)

**gcloud** é a ferramenta de linha de comando do Google Cloud Platform. Ele vem pré-instalado no Cloud Shell e aceita preenchimento com tabulação.

É possível listar o nome da conta ativa com este comando:

```shell
    gcloud auth list
```

Saída:

```shell
    ACTIVE: *
    ACCOUNT: student-01-xxxxxxxxxxxx@qwiklabs.net
    To set the active account, run:
        $ gcloud config set account `ACCOUNT`
```

É possível listar o ID de projeto com este comando:

```shell
    gcloud config list project
```

Saída:

```shell
    [core]
    project = <project_ID>
```

Exemplo de saída:

    [core]
    project = qwiklabs-gcp-44776a13dea667a6

A documentação completa do **gcloud** está disponível na página [Visão geral do gcloud](https://cloud.google.com/sdk/gcloud) do Google Cloud.

Tarefa 1: defina uma zona do Compute padrão.
--------------------------------------------

A [zona do Compute](https://cloud.google.com/compute/docs/regions-zones/#available) é um local regional próximo que armazena seus clusters e os recursos correspondentes. Por exemplo, `us-central1-a` é uma zona na região `us-central1`. Inicie uma nova sessão no Cloud Shell.

1. **Defina a região padrão do Compute**,
   
   ```shell
   
   gcloud config set compute/region 
   
   ```
   
   **Saída esperada**:
   
   Updated property [compute/region].

2. **Defina a zona padrão do Compute**,
   
   ```shell
      gcloud config set compute/region us-east5
   
   ```
   
   **Saída esperada**:
   
   Updated property [compute/zone].

Tarefa 2: crie um cluster do GKE
--------------------------------

Um [cluster](https://cloud.google.com/kubernetes-engine/docs/concepts/cluster-architecture) consiste em pelo menos uma máquina **mestre do cluster** e diversas máquinas worker chamadas de **nós**. Os nós são [instâncias de máquina virtual (VM) do Compute Engine](https://cloud.google.com/compute/docs/instances/) que executam os processos do Kubernetes necessários para integrá-los ao cluster.

**Observação**: nomes de cluster precisam começar com uma letra, terminar com um caractere alfanumérico e não podem ter mais de 40 caracteres.

Execute este comando:

1. **Criar um cluster**
   
   ```shell
   gcloud container clusters create --machine-type=e2-medium --zone=us-east5-c lab-cluster 
   ```

Ignore os avisos na resposta. A criação do cluster pode demorar alguns minutos.

**Saída esperada**:

```shell
NAME: lab-cluster
LOCATION:  
MASTER_VERSION: 1.22.8-gke.202
MASTER_IP: 34.67.240.12
MACHINE_TYPE: e2-medium
NODE_VERSION: 1.22.8-gke.202
NUM_NODES: 3
STATUS: RUNNING
```

Tarefa 3: receba as credenciais de autenticação para o cluster
--------------------------------------------------------------

Depois de criar o cluster, você precisará de credenciais de autenticação para interagir com ele.

1. **Autenticar com o cluster**:
   
   ```shell
   
   gcloud container clusters get-credentials lab-cluster 
   
   ```
   
   **Saída esperada**:
   
   ```shell
   
   Fetching cluster endpoint and auth data.kubeconfig entry generated for my-cluster.
   
   ```

Tarefa 4: implantar um aplicativo no cluster
--------------------------------------------

Agora é possível implantar um aplicativo conteinerizado no cluster. Neste laboratório, você executará o `hello-app` no seu cluster.

O GKE usa objetos do Kubernetes para criar e gerenciar os recursos do cluster. O Kubernetes fornece o objeto de [implantação](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) para implantar aplicativos sem estado como servidores da Web. Os objetos de [serviço](https://kubernetes.io/docs/concepts/services-networking/service/) definem as regras e o balanceamento de carga para acessar seu aplicativo pela Internet.

1. Para **criar uma nova implantação** `hello-server` da imagem do contêiner `hello-app`, execute este comando [kubectl create](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create):

```shell
kubectl create deployment hello-server --image=gcr.io/google-samples/hello-app:1.0
```

   **Saída esperada**:

```shell
deployment.apps/hello-server created
```

   Esse comando do Kubernetes cria um objeto de implantação que representa `hello-server`. Neste caso, `--image` especifica uma imagem de contêiner a ser implantada. O comando extrai a imagem de exemplo de um bucket do [Container Registry](https://cloud.google.com/container-registry/docs), e `gcr.io/google-samples/hello-app:1.0` indica a versão específica da imagem a ser extraída. Se nenhuma for especificada, a versão mais recente será usada.

```shell
   NAME             TYPE            CLUSTER-IP      EXTERNAL-IP     PORT(S)           AGE
   hello-server     loadBalancer    10.39.244.36    35.202.234.26   8080:31991/TCP    65s
   kubernetes       ClusterIP       10.39.240.1               433/TCP           5m13s
```

   **Observação**: pode levar um minuto para o endereço IP ser gerado. Execute o comando anterior novamente se o status da coluna `EXTERNAL-IP` for **pendente**.

2. Para **criar um serviço do Kubernetes**, um recurso que permite expor o aplicativo ao tráfego externo, execute o comando [kubectl expose](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#expose) a seguir:
   
   ```shell
   kubectl expose deployment hello-server --type=LoadBalancer --port 8080
   ```

   Nesse comando:

* `--port` especifica a porta que o contêiner expõe.

* `type="LoadBalancer"` cria um balanceador de carga do Compute Engine para o contêiner.
  
  **Saída esperada**:
  
  ```shell
  service/hello-server exposed
  ```
  
  3. Para **inspecionar** o serviço `hello-server`, execute [kubectl get](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get):
     
     ```shell
     kubectl get service
     ```
  
  **Saída esperada**:
  
  ```shell
  NAME             TYPE            CLUSTER-IP      EXTERNAL-IP     PORT(S)           AGE
  hello-server     loadBalancer    10.39.244.36    35.202.234.26   8080:31991/TCP    65s
  kubernetes       ClusterIP       10.39.240.1               433/TCP           5m13s
  ```
  
  **Observação**: pode levar um minuto para o endereço IP ser gerado. Execute o comando anterior novamente se o status da coluna `EXTERNAL-IP` for **pendente**.
4. Para visualizar o aplicativo no navegador da Web, abra uma nova guia e digite o endereço a seguir, substituindo `[EXTERNAL IP]` pelo `EXTERNAL-IP` do `hello-server`.
5. 

```shell
http://[EXTERNAL-IP]:8080
```

   **Saída esperada**: a guia do navegador mostra a mensagem **Hello World!**, a versão e o nome do host.

Tarefa 5: excluir o cluster
---------------------------

1. Para **excluir** o cluster, execute este comando:
   
   ```shell
   gcloud container clusters delete lab-cluster 
   
   ```

2. Quando solicitado, digite **Y** para confirmar.
   
   A exclusão do cluster pode levar alguns minutos. Para mais informações sobre clusters do GKE excluídos, consulte o artigo do Google Kubernetes Engine (GKE), [Como excluir um cluster](https://cloud.google.com/kubernetes-engine/docs/how-to/deleting-a-cluster).
