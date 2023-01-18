# Cloud Functions: Qwik Start – linha de comando

Visão geral
-----------

O Cloud Functions é um ambiente de execução sem servidor para criar e conectar serviços em nuvem. Com ele, você pode escrever funções simples com uma única finalidade e vinculadas a eventos emitidos pela sua infraestrutura e pelos serviços em nuvem. Sua Função do Cloud é acionada quando um evento em análise é disparado. Seu código pode ser executado em um ambiente totalmente gerenciado. Não é necessário provisionar infraestruturas ou se preocupar com gerenciamento de servidores.

As funções do Cloud podem ser escritas em Node.js, Python e Go, além de ser usadas em ambientes de execução com linguagens de programação específicas. Use essas funções em qualquer ambiente Node.js padrão para simplificar a portabilidade e os testes locais.

Conecte e amplie os serviços em nuvem
-------------------------------------

Com a camada de conexão de lógica do Cloud Functions, é possível criar um código para conectar e ampliar os serviços na nuvem. Detecte e responda a um upload de arquivo para o Cloud Storage, uma alteração de registro ou uma mensagem recebida em um tópico do Cloud Pub/Sub.

O Cloud Functions amplia os serviços na nuvem e permite atender a um número crescente de casos de uso com lógica de programação arbitrária.

As Funções do Cloud têm acesso às credenciais de contas de serviço do Google e, assim, são autenticadas facilmente na maioria dos serviços do Google Cloud, como Datastore, Cloud Spanner, APIs Cloud Translation e Cloud Vision e muito mais. Além disso, elas são compatíveis com diversas [bibliotecas de cliente Node.js](https://cloud.google.com/nodejs/apis), o que simplifica ainda mais essas integrações.

Eventos e gatilhos
------------------

Os eventos do Cloud são _ocorrências_ no ambiente de nuvem, como mudanças nas informações de um banco de dados, a adição de arquivos a um sistema de armazenamento ou a criação de uma nova instância de máquina virtual.

Os eventos ocorrem mesmo que você responda ou não a eles. Você cria uma resposta a um evento com um _gatilho_. Ele é uma declaração de que você tem interesse em um determinado evento ou grupo de eventos. Vincular uma função a um gatilho permite que você capture e interaja com os eventos. Para saber como criar e associar gatilhos com funções, acesse [a seção "Eventos e gatilhos" do guia do Cloud Functions](https://cloud.google.com/functions/docs/concepts/events-triggers).

Sem servidor
------------

Com o Cloud Functions, você não precisa gerenciar servidores, configurar software, atualizar frameworks e fazer patch de sistemas operacionais. Como o software e a infraestrutura são totalmente gerenciados pelo Google, você precisa apenas adicionar o código. Além disso, o provisionamento de recursos ocorre automaticamente em resposta aos eventos. Isso significa que uma função pode processar de poucas invocações por dia a muitos milhões sem que você precise fazer nada.

Casos de uso
------------

Você não precisa mais ter um servidor e um desenvolvedor dedicados para lidar com cargas de trabalho assíncronas, por exemplo, ETL leve, ou automações na nuvem, como acionamento de builds de aplicativos. Basta implantar uma Função do Cloud vinculada ao evento desejado e pronto.

Devido à natureza detalhada e sob demanda do Cloud Functions, ele também é a ferramenta ideal para APIs leves e webhooks. Como o provisionamento dos endpoints HTTP é automático, quando você implanta uma função HTTP, não é preciso fazer mudanças complexas nas configurações, assim como ocorre em outros serviços. Veja a tabela a seguir para conhecer outros casos de uso comuns do Cloud Functions:

| Caso de uso                                   | Descrição                                                                                                                                                                                                                                                                                                          |
| --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Processamento de dados / ETL                  | Detecte e responda a eventos do [Cloud Storage](https://cloud.google.com/storage), por exemplo, quando um arquivo é criado, modificado ou removido. Processe imagens, execute transcodificação de vídeos, valide e transforme dados e faça a invocação de qualquer serviço na Internet usando sua Função do Cloud. |
| Webhooks                                      | Com um simples [gatilho HTTP](https://cloud.google.com/functions/docs/calling/http), responda a eventos originados de sistemas de terceiros, como GitHub, Slack, Stripe, ou de qualquer lugar que possa enviar solicitações HTTP.                                                                                  |
| APIs leves                                    | Crie aplicativos com bits de lógica leves e de acoplamento flexível, rápidos de serem criados e escalonados. Suas funções podem ser orientadas por eventos ou invocadas diretamente por HTTP/S.                                                                                                                    |
| Back-end para dispositivos móveis             | Use a [plataforma Firebase](https://firebase.google.com/docs/functions/) do Google para desenvolvedores de apps e crie o back-end para dispositivos móveis no Cloud Functions. Detecte e responda a eventos do Firebase Analytics, do Realtime Database, do Authentication e do Storage.                           |
| Internet das Coisas (IoT, na sigla em inglês) | Imagine dezenas ou centenas de milhares de dispositivos fazendo streaming de dados no Cloud Pub/Sub e iniciando o Cloud Functions para processar, transformar e armazenar dados. O Cloud Functions permite que você faça isso tudo sem servidor.                                                                   |

Veja neste laboratório prático como criar, implantar e testar uma função do Cloud usando a linha de comando do Google Cloud Shell.

Veja neste laboratório prático como criar, implantar e testar uma função do Cloud usando a linha de comando do Google Cloud Shell.

### Atividades deste laboratório

* Criar uma função simples do Cloud Functions
* Implementar e testar a função
* veja os registros

Instalação
----------

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

```
    [core]
    project = <project_ID>
```

Exemplo de saída:

```
    [core]
    project = qwiklabs-gcp-44776a13dea667a6
```

A documentação completa do **gcloud** está disponível na página [Visão geral do gcloud](https://cloud.google.com/sdk/gcloud) do Google Cloud.

Tarefa 1: crie uma função
-------------------------

Primeiro, você criará uma função simples chamada "helloWorld". Ela escreve uma mensagem nos registros do Cloud Functions. Ela é acionada por eventos da Função do Cloud e aceita um callback usado para sinalizar que a função foi concluída.

Neste laboratório, o evento da Função do Cloud é um evento de tópico do Cloud Pub/Sub. O Cloud Pub/Sub é um serviço de mensagens em que os remetentes são separados dos destinatários das mensagens. Uma assinatura é necessária para um destinatário ser avisado e receber uma mensagem enviada ou postada. Saiba sobre o [Google Cloud Pub/Sub: um serviço de mensagens em escala do Google](https://cloud.google.com/pubsub/architecture) nos guias do Cloud Pub/Sub para entender os pub/subs.

Para entender os parâmetros de evento e callback, consulte [Funções em segundo plano](https://cloud.google.com/functions/docs/writing/background) na documentação do Cloud Functions.

Para criar uma Função do Cloud, faça o seguinte:

1. Na linha de comando do Cloud Shell, crie um diretório para o código da função:
   
   ```shell
   mkdir gcf_hello_world
   ```

2. Mova para o diretório `gcf_hello_world`:
   
   ```shell
   cd gcf_hello_world
   ```

3. Crie e abra `index.js` para editar:
   
   ```shell
   nano index.js
   ```

4. Copie o seguinte comando no arquivo `index.js`:
   
   ```
   /*** Background Cloud Function to be triggered by Pub/Sub.* This function is exported by index.js, and executed when* the trigger topic receives a message.** @param {object} data The event payload.* @param {object} context The event metadata.*/exports.helloWorld = (data, context) => {const pubSubMessage = data;const name = pubSubMessage.data    ? Buffer.from(pubSubMessage.data, 'base64').toString() : "Hello World";console.log(`My Cloud Function: ${name}`);};
   ```

5. Saia do nano (Ctrl+X) e salve (Y) o arquivo.

Tarefa 2: crie um bucket do Google Cloud Storage
------------------------------------------------

* Use o seguinte comando para criar um novo bucket do Cloud Storage para sua função:

gsutil mb -p [PROJECT_ID] gs://[BUCKET_NAME]

Copiado.

content_copy

* **PROJECT_ID** é o ID do projeto que aparece no painel de detalhes do lado esquerdo deste laboratório

* **BUCKET_NAME** é o nome que você atribui ao bucket. Ele precisa ser exclusivo globalmente. Para saber como nomear buckets, consulte as [Diretrizes de nomenclatura de bucket](https://cloud.google.com/storage/docs/naming-buckets) na documentação do Cloud Storage.

### Teste a tarefa concluída

Clique em **Verificar meu progresso** para conferir a tarefa realizada. Se você concluiu a tarefa, vai receber uma pontuação de avaliação.

Crie um bucket do Cloud Storage.

Verificar meu progresso

Tarefa 3: implante a função
---------------------------

Ao implantar uma nova função, você precisa especificar `--trigger-topic`, `--trigger-bucket` ou `--trigger-http`. Ao implantar uma atualização em uma função existente, ela mantém o gatilho existente, a menos que seja especificado de outra maneira.

Neste laboratório, você definirá `--trigger-topic` como `hello_world`.

1. Implante a função em um tópico do Pub/Sub chamado **hello_world**, substituindo `[BUCKET_NAME]` pelo nome do seu bucket:
   
   ```shell
   gcloud functions deploy helloWorld \  --stage-bucket [BUCKET_NAME] \  --trigger-topic hello_world \  --runtime nodejs8
   ```
   
   **Observação**: se um erro "OperationError" aparecer, ignore e execute novamente o comando.

Se solicitado, insira `Y` para permitir invocações não autenticadas de uma nova função.

2. Verifique o status da função:
   
   ```shell
   gcloud functions describe helloWorld
   ```

O status "ACTIVE" indica que a função foi implantada.

```
entryPoint: helloWorld
eventTrigger:
  eventType: providers/cloud.pubsub/eventTypes/topic.publish
  failurePolicy: {}
  resource:
...
status: ACTIVE
...
```

Cada mensagem publicada no tópico aciona uma execução de função. O conteúdo da mensagem é transmitido como dados de entrada.

Tarefa 4: teste a função
------------------------

Depois de implantar a função e confirmar que ela está ativa, teste se a função grava uma mensagem no registro da nuvem após detectar um evento.

* Insira o comando a seguir para criar um teste de mensagem da função:
  
  ```shell
  DATA=$(printf 'Hello World!'|base64) && gcloud functions call helloWorld --data '{"data":"'$DATA'"}'
  ```

A ferramenta da nuvem retorna o ID de execução para a função, o que significa que uma mensagem foi criada no registro.

Exemplo de saída:

```shell
executionId: 3zmhpf7l6j5b
```

Veja os registros para confirmar que há mensagens com o ID de execução.

Tarefa 5: veja os registros
---------------------------

* Verifique os registros para ver suas mensagens no histórico:
  
  ```
  gcloud functions logs read helloWorld
  ```

Se a função for executada corretamente, as mensagens no registro serão como estas:

```
LEVEL  NAME        EXECUTION_ID  TIME_UTC                 LOG
D      helloWorld  3zmhpf7l6j5b  2017-12-05 22:17:42.585  Function execution started
I      helloWorld  3zmhpf7l6j5b  2017-12-05 22:17:42.650  My Cloud Function: Hello World!
D      helloWorld  3zmhpf7l6j5b  2017-12-05 22:17:42.666  Function execution took 81 ms, finished with status: 'ok'
```

**Observação**: os registros vão aparecer por volta de 10 minutos. Para visualizar os registros de outra forma, acesse **Geração de registros** > **Explorador de registros**.

Seu aplicativo está implantado e testado, e você pode ver os registros.

### Termine a Quest

Este laboratório autoguiado faz parte das quests [Baseline: Deploy & Develop](https://google.qwiklabs.com/quests/37), [Baseline: Infrastructure](https://google.qwiklabs.com/quests/33) e [Optimizing your Google Cloud Costs](https://google.qwiklabs.com/quests/97). Uma Quest é uma série de laboratórios relacionados que formam um programa de aprendizado. Ao concluir uma Quest, você ganha um selo como reconhecimento da sua conquista. É possível publicar os selos e incluir um link para eles no seu currículo on-line ou nas redes sociais. Inscreva-se em qualquer Quest que tenha este laboratório para receber os créditos de conclusão na mesma hora. Confira o [catálogo do Google Cloud Ensina](http://cloudskillsboost.google/catalog) para ver todas as Quests disponíveis.

### Comece o próximo laboratório

Este laboratório também faz parte de uma série chamada Qwik Starts. Ela foi desenvolvida para apresentar os vários recursos disponíveis no Google Cloud. Pesquise "Qwik Starts" no [catálogo de laboratórios](https://google.qwiklabs.com/catalog) para encontrar algum que seja do seu interesse.

### Próximas etapas/Saiba mais

* Agora que você usou a linha de comando para iniciar uma função do Cloud, teste o laboratório [Cloud Functions: Qwik Start - Console](https://google.qwiklabs.com/catalog_lab/704) para fazer o mesmo só que usando o Console do Cloud.
* Saiba mais sobre o App Engine em [Visão geral do App Engine](https://cloud.google.com/appengine/docs/standard/python/an-overview-of-app-engine).

### Treinamento e certificação do Google Cloud

...ajuda você a aproveitar as tecnologias do Google Cloud ao máximo. [Nossas aulas](https://cloud.google.com/training/courses) incluem habilidades técnicas e práticas recomendadas para ajudar você a alcançar rapidamente o nível esperado e continuar sua jornada de aprendizado. Oferecemos treinamentos que vão do nível básico ao avançado, com opções de aulas virtuais, sob demanda e por meio de transmissões ao vivo para que você possa encaixá-las na correria do seu dia a dia. As [certificações](https://cloud.google.com/certification/) ajudam você a validar e comprovar suas habilidades e conhecimentos das tecnologias do Google Cloud.
