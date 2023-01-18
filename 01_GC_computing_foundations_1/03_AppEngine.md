App Engine: Qwik Start - Python
===============================

20 minutos5 créditos

[

](https://www.cloudskillsboost.google/focuses/107342232/reviews?parent=course_session)

Avaliar laboratório

GSP067
------

Visão geral
-----------

Com o App Engine, os desenvolvedores podem se concentrar no que fazem melhor: escrever código. O ambiente padrão do App Engine é baseado em instâncias de contêiner que são executadas na infraestrutura do Google. Os contêineres são pré-configurados com um dos diversos ambientes de execução disponíveis (Java 7, Java 8, Python 2.7, Go e PHP). Cada ambiente de execução também contém bibliotecas compatíveis com [APIs padrão do App Engine](https://cloud.google.com/appengine/docs/about-the-standard-environment#index_of_features). Para muitos aplicativos, você só precisa de bibliotecas e ambientes de execução padrão.

Com o ambiente padrão do App Engine, fica mais fácil criar e implantar um aplicativo que será executado de maneira confiável, sob grande carga e com grandes quantidades de dados. Ele inclui os seguintes recursos:

* Armazenamento persistente com consultas, classificação e transações
* Escalonamento e balanceamento de carga automáticos
* Filas de tarefas assíncronas para realizar trabalhos fora do escopo de uma solicitação
* Tarefas agendadas para acionar eventos em horários específicos ou intervalos regulares
* Integração com outros [serviços e APIs do Google Cloud](https://cloud.google.com/products/)

Os aplicativos são executados em um ambiente seguro e controlado, permitindo que o ambiente padrão do App Engine distribua solicitações por vários servidores e escalonando os servidores de acordo com as demandas de tráfego. O aplicativo é executado no próprio ambiente seguro e confiável, independente do hardware, sistema operacional ou local físico do servidor.

Neste laboratório prático, você aprenderá a criar um aplicativo do App Engine que exibe uma mensagem curta.

### O que você aprenderá

Neste laboratório você vai fazer o seguinte em um app em Python:

* Download

* Atualizar

* Teste

* Implantar

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

    gcloud auth list

Saída:

    ACTIVE: *
    ACCOUNT: student-01-xxxxxxxxxxxx@qwiklabs.net
    To set the active account, run:
        $ gcloud config set account `ACCOUNT`

É possível listar o ID de projeto com este comando:

    gcloud config list project

Saída:

    [core]
    project = <project_ID>

Exemplo de saída:

    [core]
    project = qwiklabs-gcp-44776a13dea667a6

A documentação completa do **gcloud** está disponível na página [Visão geral do gcloud](https://cloud.google.com/sdk/gcloud) do Google Cloud.

Tarefa 1: ative a API Google App Engine Admin
---------------------------------------------

Com essa API, os desenvolvedores podem provisionar e gerenciar aplicativos do App Engine.

1. No **menu de navegação** à esquerda, clique em **APIs e serviços** > **Biblioteca**.

2. Digite "API App Engine Admin" na caixa de pesquisa.

3. Clique no card **API App Engine Admin**.

4. Selecione **Ativar**. Se a mensagem que solicita a ativação da API não for exibida, ela já estará ativada e você não vai precisar fazer nada.

Tarefa 2: faça o download do app Hello World
--------------------------------------------

Existe um aplicativo simples para Python chamado Hello World que pode ser usado para entender como funciona a implantação de um app no Google Cloud. Siga estas etapas para fazer o download do aplicativo Hello World na sua instância do Google Cloud:

1. Digite o comando a seguir para copiar o repositório do app de exemplo Hello World na sua instância do Google Cloud:
   
```shell

git clone https://github.com/GoogleCloudPlatform/python-docs-samples.git

```

2. Acesse o diretório que contém o código de amostra:
```shell
cd python-docs-samples/appengine/standard_python3/hello_world
```

Tarefa 3: teste o aplicativo
----------------------------

Teste o aplicativo com o servidor de desenvolvimento do Google Cloud (`dev_appserver.py`), incluído no SDK do App Engine pré-instalado.

1. No diretório helloworld onde está localizado o arquivo de configuração [app.yaml](https://cloud.google.com/appengine/docs/standard/python/config/appref) do aplicativo, inicie o servidor de desenvolvimento do Google Cloud com o seguinte comando:
```shell
dev_appserver.py app.yaml
```


O servidor da Web está em execução e recebendo solicitações na porta 8080.

2. Clique em **Visualização na Web** (![Ícone de visualização da Web](https://cdn.qwiklabs.com/7b9oXblGsiFuNK7hmDZjFB%2B7Lrwdv5T64bbmo8X9FAo%3D)) > **Visualizar na porta 8080** para ver os resultados.
   
   Em uma nova janela do navegador, você verá o seguinte:
   
   ![Janela do navegador com &quot;Hello World!&quot; na página](https://cdn.qwiklabs.com/BIPZByP1eH9Q0K2oQSImfuPKKHlWj%2FbpNiSmef%2BuOaQ%3D)

Tarefa 4: faça uma alteração
----------------------------

É possível deixar o servidor de desenvolvimento em execução enquanto você desenvolve o aplicativo. Esse servidor detecta alterações nos arquivos de origem e os atualiza, se necessário.

Vamos tentar. Coloque o servidor de desenvolvimento em execução. Abra outra janela de linha de comando e depois edite `main.py` para mudar "Hello World!" para "Hello, Cruel World!".

1. Clique em (**+**) ao lado da guia do Cloud Shell para abrir uma nova sessão de linha de comando.
   
   ![Botão &quot;+&quot;](https://cdn.qwiklabs.com/mDTDfc9iJWNUoUBakp5ocggWNwmpChiG7gDHnUCrACM%3D)

2. Digite este comando para acessar o diretório que contém o código de amostra:

```shell
cd python-docs-samples/appengine/standard_python3/hello_world
```

3. Digite o seguinte para abrir o main.py no nano e editar o conteúdo:
```shell
nano main.py
```

4. Mude "Hello World!" para "Hello, Cruel World!".

5. Feche e salve o arquivo.

6. Recarregue a janela do navegador com "Hello World!" ou clique em **Visualização na Web** (![Ícone de visualização da Web](https://cdn.qwiklabs.com/7b9oXblGsiFuNK7hmDZjFB%2B7Lrwdv5T64bbmo8X9FAo%3D)) > **Visualizar na porta 8080** para ver os resultados.
   
   ![Janela do navegador com &quot;Hello, Cruel World!&quot; na página](https://cdn.qwiklabs.com/znqxKObHIzucdgmme4nJP485ReBUAMdG%2BOKJP9XLzes%3D)

Tarefa 5: implante o aplicativo
-------------------------------

1. Para implantar o aplicativo no App Engine, execute o comando a seguir no diretório raiz do aplicativo, onde o arquivo app.yaml está localizado:

```shell
gcloud app deploy
```


Você vai precisar inserir o local em que ficará o App Engine:

```shell
Please choose the region where you want your App Engine applicationlocated:[1] asia-east2[2] asia-northeast1[3] asia-northeast2[4] asia-northeast3[5] asia-south1[6] asia-southeast2[7] australia-southeast1[8] europe-west[9] europe-west2[10] europe-west3[11] europe-west6[12] northamerica-northeast1[13] southamerica-east1[14] us-central[15] us-east1[16] us-east4[17] us-west2[18] us-west3[19] us-west4[20] cancelPlease enter your numeric choice:
```
2. Insira o número que representa sua região. O aplicativo do App Engine será criado.

Exemplo de saída:
```shell
Creating App Engine application in project [qwiklabs-gcp-233dca09c0ab577b] and region [asia-south1]....done.Services to deploy:descriptor:      [/home/gcpstaging8134_student/python-docs-samples/appengine/standard/hello_world/app.yaml]
source:          [/home/gcpstaging8134_student/python-docs-samples/appengine/standard/hello_world]target project:  [qwiklabs-gcp-233dca09c0ab577b]target service:  [default]target version:  [20171117t072143]target url:      [https://qwiklabs-gcp-233dca09c0ab577b.appspot.com]
Do you want to continue (Y/n)?
```
3. Digite **Y** para confirmar os detalhes e começar a implantação do serviço.

Exemplo de saída:
```shell
Beginning deployment of service [default]...Some files were skipped. Pass `--verbosity=info` to see which ones.You may also view the gcloud log file, found at[/tmp/tmp.dYC7xGu3oZ/logs/2017.11.17/07.18.27.372768.log].╔════════════════════════════════════════════════════════════╗╠═ Uploading 5 files to Google Cloud Storage                ═╣╚════════════════════════════════════════════════════════════File upload done.Updating service [default]...done.Waiting for operation [apps/qwiklabs-gcp-233dca09c0ab577b/operations/2e88ab76-33dc-4aed-93c4-fdd944a95ccf] to complete...done.Updating service [default]...done.Deployed service [default] to [https://qwiklabs-gcp-233dca09c0ab577b.appspot.com]You can stream logs from the command line by running:
  $ gcloud app logs tail -s defaultTo view your application in the web browser run:  $ gcloud app browse
```
**Observação**: se você receber uma mensagem de erro como "Unable to retrieve P4SA" ao implantar o app, execute o comando acima novamente.

Tarefa 6: veja o aplicativo
---------------------------

* Para iniciar o navegador, digite o comando abaixo e clique no link informado:

```shell
gcloud app browse
```


Exemplo de saída (seu link será diferente):

```shell
Did not detect your browser. Go to this link to view your app:https://qwiklabs-gcp-233dca09c0ab577b.appspot.com
```
![Janela do navegador com &quot;Hello, Cruel World!&quot; na página](https://cdn.qwiklabs.com/jcmBHIwDjWPXrIhp%2BdkpAFj91XplPzuzQFPEd0XSR4k%3D)

Seu aplicativo será implantado, e você poderá ler a mensagem curta no navegador.
