# Google Cloud Computing Foundations

## Cloud SDK

Vamos conhecer o Kit de Desenvolvimento de Software (SDK) Cloud para usar ferramentas de linha de comando do Google Cloud em um PC. Ele contém ferramentas da linha de comando para gerenciar recursos e aplicativos hospedados no Google Cloud. Isso inclui: CLI gcloud, a principal interface de linha de comando para produtos e serviços do Cloud; gsutil, para acessar o Cloud Storage pela linha de comando e bq, uma ferramenta da linha de comando para o BigQuery. Todas as ferramentas do SDK Cloud instaladas ficam no diretório bin. Para instalar o SDK Cloud no PC, acesse cloud.google.com/sdk e selecione o sistema operacional para fazer o download do software. Depois siga as instruções referentes ao seu sistema operacional. Depois da instalação, você precisa configurar o SDK Cloud para seu ambiente do Google Cloud.

Execute o comando `gcloud init`. Você vai precisar informar suas credenciais de login, o projeto padrão e a região e zona padrão.

## Cloud Shell

gcloud é a ferramenta de linha de comando do Google Cloud Platform. Ele vem pré-instalado no Cloud Shell e aceita preenchimento com tabulação.

É possível listar o nome da conta ativa com este comando:

`gcloud auth list`

Saída:

```shell
ACTIVE: *
ACCOUNT: student-01-xxxxxxxxxxxx@qwiklabs.net
```

To set the active account, run:
`    $ gcloud config set account `ACCOUNT` `

É possível listar o ID de projeto com este comando:

gcloud config list project

Saída:

[core]
project = <project_ID>

Exemplo de saída:

[core]
project = qwiklabs-gcp-44776a13dea667a6

A documentação completa do gcloud está disponível na página Visão geral do gcloud do Google Cloud.
Depois da ativação do Cloud Shell, você pode usar a linha de comando para invocar a ferramenta gcloud do SDK do Cloud ou outras ferramentas disponíveis na instância da máquina virtual. Mais adiante no laboratório, você vai usar o diretório `$HOME`, que armazena arquivos de vários projetos e sessões do Cloud Shell no disco permanente. O diretório `$HOME` é particular e não pode ser acessado por outros usuários.

### Como definir variáveis de ambiente

Essas variáveis definem o ambiente e economizam tempo na criação de scripts com APIs ou executáveis.

Crie uma variável de ambiente para armazenar seu ID do projeto:

```shell

export PROJECT_ID=$(gcloud config get-value project)
```

Crie uma variável de ambiente para armazenar sua Zona:

```shell
export ZONE=$(gcloud config get-value compute/zone)
```

Para verificar se as variáveis foram definidas corretamente, execute os comandos abaixo:

```shell
echo -e "PROJECT ID: $PROJECT_ID\nZONE: $ZONE"
```

Se as variáveis tiverem sido definidas corretamente, os comandos "echo" vão gerar o ID do projeto e a zona.

### Como criar uma máquina virtual com a ferramenta gcloud

Use a ferramenta gcloud para criar uma nova instância de máquina virtual (VM).
Execute o comando a seguir para criar sua VM:

```shell
gcloud compute instances create gcelab2 --machine-type e2-medium --zone $ZONE
```

**Saída:**

```shell
Created [https://www.googleapis.com/compute/v1/projects/qwiklabs-gcp-04-326fae68bc3d/zones/us-east1-c/instances/gcelab2].
NAME     ZONE           MACHINE_TYPE   PREEMPTIBLE  INTERNAL_IP  EXTERNAL_IP   STATUS
gcelab2  {{{project_0.startup_script.project_zone}}}  e2-medium               10.128.0.2   34.67.152.90  RUNNING
```

**Detalhes do comando**

`gcloud compute` permite gerenciar os recursos do Compute Engine de forma mais simples do que com a API do Compute Engine.

`instances create` cria uma nova instância.

`gcelab2` é o nome da VM.

A sinalização `--machine-type` especifica o tipo de máquina e2-medium.

A sinalização `--zone` especifica o local de criação da VM.

Ao omitir `--zone`, a ferramenta gcloud pode inferir a zona desejada com base nas propriedades padrão. Se as outras configurações de instância necessárias, como machine type e image, não forem especificadas no comando create, elas serão definidas com os valores padrão.

**Tarefa 3: conecte-se à sua instância de VM**

O gcloud compute facilita a conexão com instâncias. O comando gcloud compute ssh adiciona um wrapper no SSH que cuida da autenticação e do mapeamento dos nomes de instâncias para os endereços IP.

Para se conectar à VM com SSH, execute o comando abaixo:

```shell
gcloud compute ssh gcelab2 --zone $ZONE
```

Saída:

```shell
WARNING: The public SSH key file for gcloud does not exist.
WARNING: The private SSH key file for gcloud does not exist.
WARNING: You do not have an SSH key for gcloud.
WARNING: [/usr/bin/ssh-keygen] will be executed to generate a key.
This tool needs to create the directory
[/home/gcpstaging306_student/.ssh] before being able to generate SSH Keys.
Do you want to continue? (Y/n)
```

Para continuar, digite Y.

```shell
Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase)
```

Para deixar a senha longa vazia, pressione ENTER.
Você se conectou à máquina virtual criada no laboratório. Percebeu como o prompt de comando mudou?

O que você vê no prompt agora é semelhante a sa_107021519685252337470@gcelab2.

A referência antes de @ indica a conta que está sendo usada.
Após o símbolo @ está a indicação da máquina host acessada.
Instale o servidor da Web nginx na máquina virtual

```shell
sudo apt install -y nginx
```

Como nenhuma ação é necessária aqui, desconecte-se do SSH e saia do shell remoto executando o comando abaixo:
Você voltará ao prompt de comando do projeto.
