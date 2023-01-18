## Como criar uma máquina virtual

### Visão geral

O Compute Engine permite criar máquinas virtuais que executam diversos sistemas operacionais, incluindo vários tipos de Linux (Debian, Ubuntu, Suse, Red Hat e CoreOS) e Windows Server na infraestrutura do Google. Você pode executar milhares de CPUs virtuais em um sistema projetado para ser rápido e oferecer consistência forte no desempenho.

Neste laboratório prático, você vai criar instâncias de máquina virtual de vários tipos usando o console do Google Cloud e a linha de comando gcloud. Além disso, você verá como conectar um servidor da Web NGINX à sua máquina virtual.

Em vez de copiar e colar os comandos do laboratório no local adequado, recomendamos que você digite os comandos para reforçar sua compreensão dos conceitos principais.

### Atividades deste laboratório

- Criar uma máquina virtual com o console do Cloud
- Criar uma máquina virtual com a linha de comando gcloud
- Implantar um servidor da Web e conectá-lo a uma máquina virtual

### Pré-requisitos

É recomendável ter familiaridade com os editores de texto padrão do Linux, como `vim`, `emacs` ou `nano`.

### Tarefa 1: crie uma nova instância no console do Cloud

Nesta seção, você vai aprender a criar tipos de máquinas predefinidos usando o Compute Engine no console do Cloud.

No console do Cloud, acesse o Menu de navegação (Ícone do menu de navegação) e clique em Compute Engine > Instâncias de VM.

A primeira inicialização pode levar alguns instantes.

Para criar uma instância, clique em CRIAR INSTÂNCIA.

Você pode configurar muitos parâmetros ao criar uma instância. Para este laboratório, use os seguintes parâmetros:

| **Campo**                            | **Valor**                                                                                   | **Mais informações**                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| ------------------------------------ | ------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Nome** | **gcelab**                                                                                  | Nome da instância de VM                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| **Região**                           | **us-east1**                                                                                | Para saber mais sobre as regiões, consulte o guia Regiões e zonas do Compute Engine.                                                                                                                                                                                                                                                                                                                                                                                         |
| **Zona**                             | **us-east1-b**                                                                              | Observação: lembre da zona selecionada. Você vai precisar dessa informação depois. Para saber mais sobre as zonas, consulte o guia Regiões e zonas do Compute Engine.                                                                                                                                                                                                                                                                                                        |
| **Série**                            | **E2**                                                                                      | Nome da série                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| **Tipo de máquina**                  | **2 vCPUs**                                                                                 | Esta é uma instância de 2 CPUs e 4 GB de RAM (e2-medium). Vários tipos de máquinas estão disponíveis, desde microinstâncias até instâncias com 32 núcleos/208 GB de RAM. Para mais informações, consulte o guia do Compute Engine Sobre as famílias de máquinas. Observação: os projetos novos têm uma cota de recursos padrão, o que limita o número de núcleos de CPU. Você poderá solicitar uma quantidade maior quando for trabalhar em projetos fora deste laboratório. |
| **Disco de inicialização**           | **Novo disco permanente equilibrado de 10 GB Imagem do SO: Debian GNU/Linux 11 (bullseye)** | Várias imagens estão disponíveis, por exemplo, Debian, Ubuntu, CoreOS e imagens premium, como Red Hat Enterprise Linux e Windows Server. Para saber mais, consulte a documentação do sistema operacional.                                                                                                                                                                                                                                                                    |
| **Firewall**                         | **Permitir tráfego HTTP**                                                                   | Selecione esta opção para acessar um servidor da Web que você vai instalar mais tarde. Observação: isso criará automaticamente uma regra de firewall para permitir o tráfego HTTP na porta 80.                                                                                                                                                                                                                                                                               |

Clique em Criar.

Levará cerca de um minuto para a máquina ser criada. Depois disso, a nova máquina virtual aparece na página Instâncias de VM.

Na linha da sua máquina virtual, clique em SSH, para se conectar a ela usando SSH.

Isso vai iniciar o cliente SSH diretamente no navegador.

Observação: consulte o guia do Compute Engine Conectar-se a VMs do Linux usando as ferramentas do Google. Esse guia tem mais informações sobre como usar o SSH para se conectar a uma instância.

### Tarefa 2: instale um servidor da Web NGINX

Agora você vai instalar um servidor da Web NGINX, um dos servidores mais conhecidos do mundo, para conectar sua máquina virtual a algo.

Atualize o SO:

```shell
 sudo apt-get update
```

Saída esperada:

```
 Get:1 http://security.debian.org stretch/updates InRelease [94.3 kB]
 Ign http://deb.debian.org strech InRelease
 Get:2 http://deb.debian.org strech-updates InRelease [91.0 kB]
 ...

```

Instale o NGINX:

```shell
 sudo apt-get install -y nginx
```

Saída esperada:

```
 Reading package lists… Done
 Building dependency tree
 Reading state information... Done
 The following additional packages will be installed:
 ...
```

Confirme se o NGINX está em execução:

```shell
 ps auwx | grep nginx
```

Saída esperada:

```
 root      2330  0.0  0.0 159532  1628 ?        Ss   14:06   0:00 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
 www-data  2331  0.0  0.0 159864  3204 ?        S    14:06   0:00 nginx: worker process
 www-data  2332  0.0  0.0 159864  3204 ?        S    14:06   0:00 nginx: worker process
 root      2342  0.0  0.0  12780   988 pts/0    S+   14:07   0:00 grep nginx
```

Para ver a página da Web, volte ao console do Cloud e clique no link IP externo na linha da máquina, ou adicione o valor do IP externo ao URL http://EXTERNAL_IP/ em uma nova janela ou guia do navegador.

Esta página da Web padrão do NGINX vai aparecer

### Tarefa 3: crie uma nova instância com a gcloud

Em vez de usar o console do Cloud para criar uma instância de máquina virtual, você pode usar a ferramenta de linha de comando gcloud, que vem pré-instalada no Google Cloud Shell. O Cloud Shell é uma máquina virtual baseada em Debian. Ele contém todas as ferramentas de desenvolvimento necessárias (gcloud, git, entre outras) e oferece um diretório principal permanente de 5 GB.

Observação: se você quiser testar esse processo no seu computador, leia o guia da ferramenta de linha de comando gcloud.
No Cloud Shell, use o comando gcloud para criar uma instância de máquina virtual na linha de comando:

```shell
     gcloud compute instances create gcelab2 --machine-type e2-medium --zone us-east1-b
```

Saída esperada:

```
     Created [...gcelab2].
     NAME: gcelab2
     ZONE: us-east1-b 
     MACHINE_TYPE: e2-medium
     PREEMPTIBLE:
     INTERNAL_IP: 10.128.0.3
     EXTERNAL_IP: 34.136.51.150
     STATUS: RUNNING
```

Os valores padrão da nova instância são estes:

A imagem mais recente do Debian 11 (bullseye)
O tipo de máquina e2-medium. Um disco permanente raiz com o mesmo nome da instância. O disco é automaticamente anexado a ela. Durante seu trabalho, é possível especificar um tipo de máquina personalizado.

Para ver todos os padrões, execute o seguinte:

```shell
 gcloud compute instances create --help
```

Observação: se você trabalha sempre na mesma região/zona e não quer anexar a flag --zone todas as vezes, recomendamos que defina a região e as zonas padrão que a gcloud usa.
Para fazer isso, execute estes comandos:

```shell
gcloud config set compute/zone ...

gcloud config set compute/region ...
```

Para sair de help, pressione CTRL+C.

Abra o Menu de navegação no console do Cloud e clique em Compute Engine > Instâncias de VM para conferir as duas novas instâncias.

Você também pode usar o SSH para se conectar à sua instância usando a gcloud. Adicione a zona ou omita a flag --zone se tiver definido a opção globalmente:

```shell
     gcloud compute ssh gcelab2 --zone us-east1-b 
```

Digite Y para continuar.

```shell
   Do you want to continue? (Y/n)
```

Pressione ENTER na seção de senha longa para deixar esse campo em branco.

```
   Generating public/private rsa key pair.
   Enter passphrase (empty for no passphrase)
```

Depois de se conectar, saia do shell remoto para se desconectar do SSH:

```shell
 exit
```
