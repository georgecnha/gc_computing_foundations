Configurar balanceadores de carga HTTP e de rede
================================================

1 hora  - 1 crédito

GSP007
------

Visão geral

Neste laboratório prático, você aprenderá as diferenças entre os balanceadores de carga HTTP e de rede e a configurá-los para aplicativos em execução nas máquinas virtuais (VMs) do Compute Engine.

Há várias maneiras de [balancear a carga no Google Cloud](https://cloud.google.com/load-balancing/docs/load-balancing-overview#a_closer_look_at_cloud_load_balancers). Este laboratório mostra as etapas de configuração dos seguintes balanceadores de carga:

* [Balanceador de carga de rede](https://cloud.google.com/compute/docs/load-balancing/network/)
* [Balanceador de carga HTTP(S)](https://cloud.google.com/compute/docs/load-balancing/http/)

Recomendamos que os alunos digitem os comandos para aprender os principais conceitos. Muitos laboratórios incluem um bloco de código com os comandos necessários. É possível copiá-los desse bloco e colar nos locais apropriados durante o laboratório.

O que você vai aprender
-----------------------

* Configurar um balanceador de carga de rede

* Configurar um balanceador de carga HTTP

* Aprender na prática as diferenças entre balanceadores de carga HTTP e de rede

Configuração e requisitos
-------------------------

#### Antes de clicar no botão Start Lab

Leia estas instruções. Os laboratórios são cronometrados e não podem ser pausados. O timer é iniciado quando você clica em **Começar o laboratório** e mostra por quanto tempo os recursos do Google Cloud ficarão disponíveis.

Este laboratório prático do Qwiklabs permite que você realize as atividades em um ambiente real de nuvem, não em uma simulação ou demonstração. Você receberá novas credenciais temporárias para fazer login e acessar o Google Cloud durante o laboratório.

#### O que é necessário

Para fazer este laboratório, você precisa ter:

* acesso a um navegador de Internet padrão (recomendamos o Chrome);
* tempo para concluir as atividades.

**Observação**: não use seu projeto ou sua conta do Google Cloud neste laboratório.

**Observação**: se estiver usando um dispositivo Chrome OS, abra uma janela anônima para executar o laboratório.

### Como iniciar seu laboratório e fazer login no console do Google Cloud

1. Clique no botão **Começar o laboratório**. Se for preciso pagar, você verá um pop-up para selecionar a forma de pagamento. No painel **Detalhes do laboratório** à esquerda, você verá o seguinte:
   
   * O botão **Abrir Console do Cloud**
   * Tempo restante
   * As credenciais temporárias que você vai usar neste laboratório
   * Outras informações se forem necessárias

2. Clique em **Abrir Console do Google**. O laboratório ativa recursos e depois abre outra guia com a página **Fazer login**.
   
   **_Dica_**: coloque as guias em janelas separadas lado a lado.
   
   **Observação**: se aparecer a caixa de diálogo **Escolher uma conta**, clique em **Usar outra conta**.

3. Caso seja preciso, copie o **Nome de usuário** no painel **Detalhes do laboratório** e cole esse nome na caixa de diálogo **Fazer login**. Clique em **Avançar**.

4. Copie a **Senha** no painel **Detalhes do laboratório** e a cole na caixa de diálogo **Olá**. Clique em **Avançar**.
   
   **Importante**: você precisa usar as credenciais do painel à esquerda. Não use suas credenciais do Google Cloud Ensina.
   
   **Observação**: se você usar sua própria conta do Google Cloud neste laboratório, é possível que receba cobranças adicionais.

5. Acesse as próximas páginas:
   
   * Aceite os Termos e Condições.
   * Não adicione opções de recuperação nem autenticação de dois fatores (porque essa é uma conta temporária).
   * Não se inscreva em testes gratuitos.

Depois de alguns instantes, o console do GCP vai ser aberto nesta guia.

**Observação**: para ver uma lista dos produtos e serviços do Google Cloud, clique no **Menu de navegação** no canto superior esquerdo. ![Ícone do menu de navegação](https://cdn.qwiklabs.com/9vT7xPlxoNP%2FPsK0J8j0ZPFB4HnnpaIJVCDByaBrSHg%3D)

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

```
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

```
    [core]
    project = qwiklabs-gcp-44776a13dea667a6
```

A documentação completa do **gcloud** está disponível na página [Visão geral do gcloud](https://cloud.google.com/sdk/gcloud) do Google Cloud.

Tarefa 1. Definir a região e a zona padrão para todos os recursos
-----------------------------------------------------------------

1. Defina a zona padrão no Cloud Shell:

```shell
gcloud config set compute/zone us-east5-b
```

2. Defina a região padrão:

```shell
gcloud config set compute/region us-east5

```

Saiba como escolher zonas e regiões na documentação [Guia de regiões e zonas](https://cloud.google.com/compute/docs/zones) do Compute Engine.

Tarefa 2: Criar várias instâncias do servidor da Web
----------------------------------------------------

Neste cenário de balanceamento de carga, crie três instâncias de VM do Compute Engine. Nelas, instale o Apache e adicione uma regra de firewall que permita a chegada do tráfego HTTP.

O código fornecido define a zona como `us-east5-b` . Com a configuração do campo de tags, é possível mencionar todas essas instâncias simultaneamente, assim como acontece nas regras de firewall. Esses comandos também instalam o Apache em todas as instâncias e atribuem uma página inicial exclusiva a cada uma delas.

1. Crie uma máquina virtual www1 na zona padrão:

```shell
gcloud compute instances create www1 \
--zone=us-east5-b \
--tags=network-lb-tag \
--machine-type=e2-small \
--image-family=debian-11 \
--image-project=debian-cloud \
--metadata=startup-script='#!/bin/bash      
apt-get update      
apt-get install apache2 -y     
service apache2 restart      
echo "<h3>Servidor da Web: www1</h3>" | tee /var/www/html/index.html'
```

2. Crie uma máquina virtual www2 na zona padrão:
   
   ```shell
   gcloud compute instances create www2 \
   --zone=us-east5-b \
   --tags=network-lb-tag \
   --machine-type=e2-small \
   --image-family=debian-11 \
   --image-project=debian-cloud \
   --metadata=startup-script='#!/bin/bash      
   apt-get update      
   apt-get install apache2 -y     
   service apache2 restart      
   echo "<h3>Servidor da Web: www2</h3>" | tee /var/www/html/index.html'
   
   ```

3. Crie uma máquina virtual www3 na zona padrão:

```shell
gcloud compute instances create www3 \
--zone=us-east5-b \
--tags=network-lb-tag \
--machine-type=e2-small \
--image-family=debian-11 \
--image-project=debian-cloud \
--metadata=startup-script='#!/bin/bash      
apt-get update      
apt-get install apache2 -y     
service apache2 restart      
echo "<h3>Servidor da Web: www3</h3>" | tee /var/www/html/index.html'

```

4. Crie uma regra de firewall para permitir o tráfego externo para as instâncias de VM:
   
   ```shell
   gcloud compute firewall-rules create www-firewall-network-lb \
   --target-tags network-lb-tag  --allow tcp:80
   
   ```

```

Agora você precisa dos endereços IP externos das instâncias para verificar se elas estão em execução.

5. Execute o comando a seguir para listá-las. Os endereços IP aparecem na coluna `EXTERNAL_IP`:

```shell
gcloud compute instances list
```

6. Verifique se cada instância está sendo executada com `curl` e substitua **[IP_ADDRESS]** pelo endereço IP de cada uma das VMs:

```shell
curl http://[IP_ADDRESS]
```

   Clique em **Verificar meu progresso** abaixo para conferir se você criou um grupo de servidores da Web.

Tarefa 3. Configurar o serviço de balanceamento de carga
--------------------------------------------------------

Ao configurar o serviço de balanceamento de carga, as instâncias de máquina virtual recebem os pacotes destinados ao endereço IP externo estático definido. As instâncias criadas com uma imagem do Compute Engine são configuradas automaticamente para lidar com esse endereço IP.

**Observação**: saiba mais sobre como configurar o balanceamento de carga da rede no [Guia de visão geral de balanceamento de carga de rede TCP/UDP externo](https://cloud.google.com/compute/docs/load-balancing/network/).

1. Crie um endereço IP externo estático para o balanceador de carga.

```shell
 gcloud compute addresses create network-lb-ip-1 \    
--region us-east5 
```

   Saída:

```
Created [https://www.googleapis.com/compute/v1/projects/qwiklabs-gcp-03-xxxxxxxxxxx/regions/us-east5/addresses/network-lb-ip-1].
```

2. Adicione um recurso legado de verificação de integridade HTTP.

```shell
gcloud compute http-health-checks create basic-check
```

3. Adicione um pool de destino na mesma região de suas instâncias. Execute o comando a seguir para criar o pool de destino e usar a verificação de integridade necessária para o funcionamento do serviço:

```shell
gcloud compute target-pools create www-pool \    
--region us-east5 --http-health-check basic-check
```

4. Adicione as instâncias ao pool:

```shell
gcloud compute target-pools add-instances www-pool \    
--instances www1,www2,www3
```

5. Adicione uma regra de encaminhamento:
   
   ```shell
   gcloud compute forwarding-rules create www-rule \    
      --region  us-east5 \
   --ports 80 \
   --address network-lb-ip-1 \
   --target-pool www-pool
   ```
   
   Clique em **Verificar meu progresso** abaixo e veja se um balanceador de carga de rede L4 foi criado para os servidores da Web.
   
   Configurar o serviço de balanceamento de carga

Tarefa 4. Como enviar tráfego às instâncias
-------------------------------------------

Agora que o serviço de balanceamento de carga foi configurado, comece a enviar tráfego para a regra de encaminhamento e veja a distribuição dele por instâncias diferentes.

1. Execute o comando a seguir para visualizar o endereço IP externo da regra de encaminhamento www-rule usada pelo balanceador de carga:
   
   ```shell
   gcloud compute forwarding-rules describe www-rule --region us-east5
   ```

2. Acesse o endereço IP externo:
   
   ```shell
   IPADDRESS=$(gcloud compute forwarding-rules describe www-rule --region us-east5 --format="json" | jq -r .IPAddress)
   ```

3. Mostre o endereço IP externo:

```shell
echo $IPADDRESS
```

4. Use o comando `curl` para acessar o endereço IP externo e substitua `IP_ADDRESS` por um endereço IP externo do comando anterior:
   
   ```shell
   while true; do curl -m1 $IPADDRESS; done
   ```
   
   A resposta do comando `curl` alterna de forma aleatória entre as três instâncias. Se ocorrer uma falha, aguarde cerca de 30 segundos até que a configuração esteja totalmente carregada e suas instâncias marcadas como íntegras antes de tentar novamente.

5. Use **Ctrl** + **c** para interromper a execução do comando.

Tarefa 5. Criar um balanceador de carga HTTP
--------------------------------------------

O balanceamento de carga HTTP(S) é implementado no Google Front End (GFE). Os GFEs são distribuídos globalmente e funcionam em conjunto usando a rede global e o plano de controle do Google. Você pode configurar regras que direcionem os URLs para conjuntos de instâncias diferentes.

As solicitações são sempre direcionadas ao grupo de instâncias mais próximo do usuário, desde que ele tenha capacidade suficiente e seja adequado para elas. Se não tiver capacidade suficiente, elas serão enviadas ao grupo mais próximo seguinte **com** capacidade.

Para configurar um balanceador de carga com um back-end do Compute Engine, suas VMs precisam estar em um grupo de instâncias. O grupo gerenciado de instâncias fornece VMs que executam os servidores de back-end de um balanceador de carga HTTP externo. Neste laboratório, os back-ends disponibilizam seus próprios nomes de host.

1. Crie primeiro o modelo do balanceador de carga:

```shell
gcloud compute instance-templates create lb-backend-template \
--region=us-east5 \   
--network=default \   
--subnet=default \   
--tags=allow-health-check \ 
--machine-type=e2-medium \
--image-family=debian-11 \
--image-project=debian-cloud \
--metadata=startup-script='#!/bin/bash     
apt-get update     
apt-get install apache2 -y     
a2ensite default-ssl     
a2enmod ssl     
vm_hostname="$(curl -H "Metadata-Flavor:Google" \http://169.254.169.254/computeMetadata/v1/instance/name)"

echo "Page served from: $vm_hostname" | \     
tee /var/www/html/index.html     
systemctl restart apache2'

```

Os [grupos gerenciados de instâncias](https://cloud.google.com/compute/docs/instance-groups) (MIGs) permitem operar apps em várias VMs idênticas. É possível tornar as cargas de trabalho escalonáveis e altamente disponíveis aproveitando serviços de MIGs automatizados, como escalonamento automático, recuperação automática, implantação regional (várias zonas) e atualização automática.

2. Crie o grupo gerenciado de instâncias com base no modelo:
   
   ```shell
   gcloud compute instance-groups managed create lb-backend-group \   
   --template=lb-backend-template \
   --size=2  \
   --zone=us-east5-b 
   ```

3. Crie a regra de firewall `fw-allow-health-check`.
   
   ```shell
   gcloud compute firewall-rules create fw-allow-health-check \
   --network=default \
   --action=allow \
   --direction=ingress \
   --source-ranges=130.211.0.0/22,35.191.0.0/16 \
   --target-tags=allow-health-check \ 
   --rules=tcp:80
   
   ```
   
   **Observação**: a regra de entrada permite tráfego dos sistemas de verificação de integridade do Google Cloud (`130.211.0.0/22` e `35.191.0.0/16`). Neste laboratório, usamos a tag de destino `allow-health-check` para identificar as VMs.

4. Agora que as instâncias estão funcionando, configure um endereço IP externo, estático e global que seus clientes podem usar para acessar o balanceador de carga:
   
   ```shell
   gcloud compute addresses create lb-ipv4-1 \  
   --ip-version=IPV4 \  
   --global
   ```
   
   **Anote o endereço IPv4 que foi reservado:**
   
   ```shell
   gcloud compute addresses describe lb-ipv4-1 \  
   --format="get(address)" \  
   --global
   ```

5. Crie uma verificação de integridade para o balanceador de carga:
   
   ```shell
   gcloud compute health-checks create http http-basic-check \  --port 80
   ```
   
   **Observação**: o Google Cloud fornece mecanismos de verificação de integridade que determinam se as instâncias de back-end respondem adequadamente ao tráfego. Para ver mais informações, consulte o [documento "Como criar verificações de integridade"](https://cloud.google.com/load-balancing/docs/health-checks).

6. Crie um serviço de back-end:

```shell

gcloud compute backend-services create web-backend-service \
--protocol=HTTP \
--port-name=http \
--health-checks=http-basic-check \
--global

```

7. Adicione seu grupo de instâncias como back-end do serviço de back-end:

```shell

gcloud compute backend-services add-backend web-backend-service \
--instance-group=lb-backend-group \
--instance-group-zone=us-east5-b \
--global
```

8. Crie um [mapa de URLs](https://cloud.google.com/load-balancing/docs/url-map-concepts) para encaminhar as solicitações de entrada ao serviço de back-end padrão:
   
   ```shell
   gcloud compute url-maps create web-map-http \    --default-service web-backend-service
   
   ```
   
   **Observação**: o mapa de URL é um recurso de configuração do Google Cloud usado para rotear solicitações para serviços de back-end ou buckets de back-end. Por exemplo, com um balanceador de carga HTTP(S) externo, é possível usar um único mapa de URL para rotear solicitações para destinos diferentes com base nas regras configuradas no mapa de URL:
   
   * Solicitações para https://example.com/video vão para um serviço de back-end.
   * Solicitações para https://example.com/audio vão para um serviço de back-end diferente.
   * Solicitações para https://example.com/images vão para um bucket de back-end do Cloud Storage.
   * As solicitações de qualquer outra combinação de host e caminho vão para um serviço de back-end padrão.

9. Crie um proxy HTTP de destino para encaminhar as solicitações ao mapa de URLs:

```shell
gcloud compute target-http-proxies create http-lb-proxy \    
--url-map web-map-http

```

10. Crie uma regra de encaminhamento global para encaminhar as solicitações recebidas para o proxy:

```shell
gcloud compute forwarding-rules create http-content-rule \
--address=lb-ipv4-1\ 
--global \
--target-http-proxy=http-lb-proxy \
--ports=80
```

**Observação**: uma [regra de encaminhamento](https://cloud.google.com/load-balancing/docs/using-forwarding-rules) e o endereço IP correspondente dela representam a configuração de front-end de um balanceador de carga do Google Cloud. Saiba mais sobre as noções básicas gerais das regras de encaminhamento com o [Guia de panorama geral de regras de encaminhamento](https://cloud.google.com/load-balancing/docs/forwarding-rule-concepts).

Clique em **Verificar meu progresso** abaixo para conferir se você criou um balanceador de carga HTTP(S) L7.

Criar um balanceador de carga HTTP

Tarefa 6. Como testar o tráfego enviado às instâncias
-----------------------------------------------------

1. No Console do Cloud, em **Menu de navegação**, acesse **Serviços de rede** > **Balanceamento de carga**.

2. Clique no balanceador de carga que você acabou de criar (`web-map-http`).

3. Na seção **Back-end**, clique no nome do back-end e confirme se as VMs estão **Íntegras**. Se não estiverem, espere alguns instantes e tente recarregar a página.

4. Quando finalmente estiverem íntegras, teste o balanceador de carga com um navegador da Web. Acesse `http://IP_ADDRESS/` e substitua `IP_ADDRESS` pelo endereço IP do balanceador de carga.

Isso talvez leve de três a cinco minutos. Se a conexão falhar, aguarde um minuto e atualize o navegador.

O navegador deve renderizar uma página que mostra o nome e a zona da instância que a forneceu, por exemplo, `Page served from: lb-backend-group-xxxx`.

1. In the Cloud Console, from the **Navigation menu**, go to **Network services** > **Load balancing**.

2. Click on the load balancer that you just created (`web-map-http`).

3. In the **Backend** section, click on the name of the backend and confirm that the VMs are **Healthy**. If they are not healthy, wait a few moments and try reloading the page.

4. When the VMs are healthy, test the load balancer using a web browser, going to `http://IP_ADDRESS/`, replacing `IP_ADDRESS` with the load balancer's IP address.

This may take three to five minutes. If you do not connect, wait a minute, and then reload the browser.

Your browser should render a page with content showing the name of the instance that served the page, along with its zone (for example, `Page served from: lb-backend-group-xxxx`).

Congratulations!
----------------

Parabéns!
---------

Você criou um balanceador de carga de rede e um balanceador de carga HTTP(S), e também praticou o uso de modelos de instância e grupos gerenciados de instâncias.
