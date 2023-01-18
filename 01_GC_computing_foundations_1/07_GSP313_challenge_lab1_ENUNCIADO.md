Create and Manage Cloud Resources: laboratório com desafio
==========================================================

1 hora1 crédito

[

](https://www.cloudskillsboost.google/focuses/10258/reviews?parent=catalog)

GSP313
------

![Laboratórios autoguiados do Google Cloud](https://cdn.qwiklabs.com/GMOHykaqmlTHiqEeQXTySaMXYPHeIvaqa2qHEzw6Occ%3D)

Visão geral
-----------

Nos laboratórios com desafio, apresentamos uma situação e um conjunto de tarefas. Para concluí-las, em vez de seguir instruções passo a passo, você usará o que aprendeu nos laboratórios da Quest. Um sistema automático de pontuação (mostrado nesta página) avaliará seu desempenho.

Nos laboratórios com desafio, não ensinamos novos conceitos do Google Cloud. O objetivo dessas tarefas é aprimorar aquilo que você já aprendeu, como a alteração de valores padrão ou a leitura e pesquisa de mensagens para corrigir seus próprios erros.

Para alcançar a pontuação de 100%, você precisa concluir todas as tarefas no tempo definido.

Este desafio é recomendado para os estudantes que se inscreveram nos laboratórios da Quest [Create and Manage Cloud Resources](https://google.qwiklabs.com/quests/120). Confira o conteúdo deles antes de iniciar este laboratório. Vamos começar?

Conhecimentos avaliados:

* Criar uma instância

* Criar um cluster do Kubernetes com três nós e executar um serviço simples

* Criar um balanceador de carga HTTP(S) na frente de dois servidores da Web

### Preparação

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

Cenário do desafio
------------------

Você começou a trabalhar como engenheiro de nuvem júnior na Jooli Inc. Sua função é ajudar a gerenciar a infraestrutura da empresa, e suas tarefas incluem o provisionamento de recursos para projetos.

Os supervisores esperam que você já tenha habilidades e conhecimento suficientes para fazer isso, por isso não fornecem nenhum guia passo a passo.

Veja algumas normas da Jooli Inc. que você precisa seguir:

1. Crie todos os recursos na região ou zona padrão, a menos que haja uma instrução diferente.

2. Os nomes geralmente têm o formato _team-resource_. Por exemplo, uma instância poderia receber o nome **nucleus-webserver1**.

3. Economize recursos. Como os projetos são monitorados, o uso excessivo levará ao encerramento do projeto (e talvez até à sua demissão), então é preciso ter cuidado. Estas são as únicas orientações da equipe de monitoramento: a menos que haja uma instrução diferente, use **f1-micro** para VMs pequenas do Linux e **n1-standard-1** para o Windows ou outros aplicativos, como nós do Kubernetes.

### Seu desafio

Assim que você se senta à mesa e abre seu novo laptop, começa a receber várias solicitações da equipe Nucleus. Leia a descrição de cada uma e crie os recursos.

Tarefa 1: crie uma instância para o projeto jumphost
----------------------------------------------------

Você vai usar essa instância para fazer a manutenção do projeto.

**Requisitos:**

* Dê o nome **`nucleus-jumphost-886`** à instância.
* Use um tipo de máquina _f1-micro_.
* Use o tipo de imagem padrão (Debian Linux).

Clique em _Verificar meu progresso_ para ver o objetivo.

Crie uma instância para o projeto jumphost

Verificar meu progresso

Tarefa 2: crie um cluster de serviço do Kubernetes
--------------------------------------------------

O número de recursos que você pode criar no projeto é limitado. Se você não receber o resultado esperado, exclua o cluster atual antes de criar outro. Caso você não faça isso, o laboratório será encerrado, e sua conta será bloqueada. Você vai precisar entrar em contato com o suporte do Qwiklabs para resolver esse problema.

A equipe está criando um aplicativo que vai usar um serviço em execução no Kubernetes. Você vai precisar:

* criar um cluster na zona us-east1-b para hospedar o serviço;
* usar o contêiner "hello-app" do Docker (`gcr.io/google-samples/hello-app:2.0`) como um marcador de posição que será substituído pelo trabalho da equipe mais tarde;
* expor o app na porta `8082` .

kubectl create deployment nucleus-webserver1 --image=gcr.io/google-samples/hello-app:2.0

Clique em _Verificar meu progresso_ para ver o objetivo.

Crie um cluster do Kubernetes

Verificar meu progresso

Tarefa 3: configure um balanceador de carga HTTP
------------------------------------------------

Você vai usar servidores da Web nginx para exibir o site, mas precisa garantir que o ambiente seja tolerante a falhas. Crie um balanceador de carga HTTP com um grupo gerenciado de instâncias de **dois servidores da Web nginx**. Use o código abaixo para configurá-los. A equipe vai substituir o código pela configuração correta mais tarde.

O número de recursos que você pode criar no projeto é limitado. Por isso, não gere mais de duas instâncias no grupo gerenciado. Se você fizer isso, o laboratório será encerrado, e baniremos sua conta.

```shell

cat << EOF > startup.sh#! /bin/bashapt-get updateapt-get install -y nginxservice nginx startsed -i -- 's/nginx/Google Cloud Platform - '"\$HOSTNAME"'/' /var/www/html/index.nginx-debian.htmlEOF

```

Você vai precisar:

* Crie um modelo de instância.
* Crie um pool de destino.
* Crie um grupo de instâncias gerenciadas.
* Defina uma regra de firewall chamada `allow-tcp-rule-794` para permitir o tráfego (80/tcp).
* Crie uma verificação de integridade.
* Crie um serviço de back-end e anexe o grupo gerenciado de instâncias à porta chamada (http:80).
* Crie um mapa de URL e direcione para ele o encaminhamento de solicitações do proxy HTTP.
* Crie uma regra de encaminhamento.

Clique em _Verificar meu progresso_ para ver o objetivo.





Create and Manage Cloud Resources: Challenge Lab
================================================

1 hour1 Credit

[

](https://www.cloudskillsboost.google/focuses/10258/reviews?locale=en&parent=catalog)

Rate Lab

GSP313
------

![Google Cloud self-paced labs logo](https://cdn.qwiklabs.com/GMOHykaqmlTHiqEeQXTySaMXYPHeIvaqa2qHEzw6Occ%3D)

Overview
--------

In a challenge lab you’re given a scenario and a set of tasks. Instead of following step-by-step instructions, you will use the skills learned from the labs in the quest to figure out how to complete the tasks on your own! An automated scoring system (shown on this page) will provide feedback on whether you have completed your tasks correctly.

When you take a challenge lab, you will not be taught new Google Cloud concepts. You are expected to extend your learned skills, like changing default values and reading and researching error messages to fix your own mistakes.

To score 100% you must successfully complete all tasks within the time period!

This lab is recommended for students who have enrolled in the [Create and Manage Cloud Resources](https://google.qwiklabs.com/quests/120) quest. Are you ready for the challenge?

Topics tested:

* Create an instance

* Create a 3-node Kubernetes cluster and run a simple service

* Create an HTTP(s) load balancer in front of two web servers

### Setup

### Before you click the Start Lab button

Read these instructions. Labs are timed and you cannot pause them. The timer, which starts when you click **Start Lab**, shows how long Google Cloud resources will be made available to you.

This hands-on lab lets you do the lab activities yourself in a real cloud environment, not in a simulation or demo environment. It does so by giving you new, temporary credentials that you use to sign in and access Google Cloud for the duration of the lab.

To complete this lab, you need:

* Access to a standard internet browser (Chrome browser recommended).

**Note:** Use an Incognito or private browser window to run this lab. This prevents any conflicts between your personal account and the Student account, which may cause extra charges incurred to your personal account.

* Time to complete the lab---remember, once you start, you cannot pause a lab.

**Note:** If you already have your own personal Google Cloud account or project, do not use it for this lab to avoid extra charges to your account.

### How to start your lab and sign in to the Google Cloud Console

1. Click the **Start Lab** button. If you need to pay for the lab, a pop-up opens for you to select your payment method. On the left is the **Lab Details** panel with the following:
   
   * The **Open Google Console** button
   * Time remaining
   * The temporary credentials that you must use for this lab
   * Other information, if needed, to step through this lab

2. Click **Open Google Console**. The lab spins up resources, and then opens another tab that shows the **Sign in** page.
   
   **_Tip:_** Arrange the tabs in separate windows, side-by-side.
   
   **Note:** If you see the **Choose an account** dialog, click **Use Another Account**.

3. If necessary, copy the **Username** from the **Lab Details** panel and paste it into the **Sign in** dialog. Click **Next**.

4. Copy the **Password** from the **Lab Details** panel and paste it into the **Welcome** dialog. Click **Next**.
   
   **Important:** You must use the credentials from the left panel. Do not use your Google Cloud Skills Boost credentials.
   
   **Note:** Using your own Google Cloud account for this lab may incur extra charges.

5. Click through the subsequent pages:
   
   * Accept the terms and conditions.
   * Do not add recovery options or two-factor authentication (because this is a temporary account).
   * Do not sign up for free trials.

After a few moments, the Cloud Console opens in this tab.

**Note:** You can view the menu with a list of Google Cloud Products and Services by clicking the **Navigation menu** at the top-left. ![Navigation menu icon](https://cdn.qwiklabs.com/9vT7xPlxoNP%2FPsK0J8j0ZPFB4HnnpaIJVCDByaBrSHg%3D)

Challenge scenario
------------------

You have started a new role as a Junior Cloud Engineer for Jooli, Inc. You are expected to help manage the infrastructure at Jooli. Common tasks include provisioning resources for projects.

You are expected to have the skills and knowledge for these tasks, so step-by-step guides are not provided.

Some Jooli, Inc. standards you should follow:

1. Create all resources in the default region or zone, unless otherwise directed.

2. Naming normally uses the format _team-resource_; for example, an instance could be named **nucleus-webserver1**.

3. Allocate cost-effective resource sizes. Projects are monitored, and excessive resource use will result in the containing project's termination (and possibly yours), so plan carefully. This is the guidance the monitoring team is willing to share: unless directed, use **f1-micro** for small Linux VMs, and use **n1-standard-1** for Windows or other applications, such as Kubernetes nodes.

### Your challenge

As soon as you sit down at your desk and open your new laptop, you receive several requests from the Nucleus team. Read through each description, and then create the resources.

Task 1. Create a project jumphost instance
------------------------------------------

You will use this instance to perform maintenance for the project.

**Requirements:**

* Name the instance **`nucleus-jumphost-274`** .
* Use an _f1-micro_ machine type.
* Use the default image type (Debian Linux).

Click _Check my progress_ to verify the objective.

Create a project jumphost instance

Check my progress

Task 2. Create a Kubernetes service cluster
-------------------------------------------

**Note:** There is a limit to the resources you are allowed to create in your project. If you don't get the result you expected, delete the cluster before you create another cluster. If you don't, the lab might end and you might be blocked. To get your account unblocked, you will have to reach out to Google Cloud Skills Boost Support.

The team is building an application that will use a service running on Kubernetes. You need to:

* Create a zonal cluster using `us-west4-b` .
* Use the Docker container hello-app (`gcr.io/google-samples/hello-app:2.0`) as a placeholder; the team will replace the container with their own work later.
* Expose the app on port `8080` .

Click _Check my progress_ to verify the objective.

Create a Kubernetes cluster

Check my progress

Task 3. Set up an HTTP load balancer
------------------------------------

You will serve the site via nginx web servers, but you want to ensure that the environment is fault-tolerant. Create an HTTP load balancer with a managed instance group of **2 nginx web servers**. Use the following code to configure the web servers; the team will replace this with their own configuration later.

**Note:** There is a limit to the resources you are allowed to create in your project, so do not create more than 2 instances in your managed instance group. If you do, the lab might end and you might be banned.

cat << EOF > startup.sh#! /bin/bashapt-get updateapt-get install -y nginxservice nginx startsed -i -- 's/nginx/Google Cloud Platform - '"\$HOSTNAME"'/' /var/www/html/index.nginx-debian.htmlEOF

Copied!

content_copy

You need to:

* Create an instance template.
* Create a target pool.
* Create a managed instance group.
* Create a firewall rule named as `allow-tcp-rule-176` to allow traffic (80/tcp).
* Create a health check.
* Create a backend service, and attach the managed instance group with named port (http:80).
* Create a URL map, and target the HTTP proxy to route requests to your URL map.
* Create a forwarding rule.

Click _Check my progress_ to verify the objective.

Create the website behind the HTTP load balancer

Check my progress

Congratulations!
----------------

You learned how to create and manage Cloud resources.

![Create and Manage Cloud Resources Skill Badge](https://cdn.qwiklabs.com/hrlQwoBPXpe4Zc9mivDlGHUQ8FWQWh8VPZbWXlX25W8%3D)

Earn your next skill badge
--------------------------

This self-paced lab is part of the [Create and Manage Cloud Resources](https://google.qwiklabs.com/quests/120) quest. Completing this skill badge quest earns you the badge above, to recognize your achievement. Share your badge on your resume and social platforms, and announce your accomplishment using #GoogleCloudBadge.

### Google Cloud training and certification

...helps you make the most of Google Cloud technologies. [Our classes](https://cloud.google.com/training/courses) include technical skills and best practices to help you get up to speed quickly and continue your learning journey. We offer fundamental to advanced level training, with on-demand, live, and virtual options to suit your busy schedule. [Certifications](https://cloud.google.com/certification/) help you validate and prove your skill and expertise in Google Cloud technologies.

**Manual Last Updated November 21, 2022**

**Lab Last Tested November 21, 2022**

Copyright 2023 Google LLC All rights reserved. Google and the Google logo are trademarks of Google LLC. All other company and product names may be trademarks of the respective companies with which they are associated.
