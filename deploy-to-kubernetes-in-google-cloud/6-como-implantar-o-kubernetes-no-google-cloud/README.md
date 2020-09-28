# Como implantar o Kubernetes no Google Cloud: laboratório com desafio

## Visão Geral

Você precisa concluir uma série de tarefas dentro do tempo estabelecido. Em vez de seguir instruções passo a passo, você deverá analisar o cenário e realizar as tarefas por conta própria. Um sistema automático de pontuação (mostrado nesta página) avaliará seu desempenho.


Para receber a pontuação máxima de 100%, você precisa terminar no tempo definido.


Os laboratórios com desafio não ensinam conceitos do Google Cloud. Para solucionar o desafio proposto, use o que você aprendeu nos outros laboratórios desta Quest. Você deverá ampliar suas habilidades e alterar os valores padrão do laboratório ao realizar as tarefas, mas não aprenderá conceitos novos.


Este laboratório é recomendado apenas para quem já concluiu a Quest [Kubernetes in Google Cloud](https://google.qwiklabs.com/quests/29).

> Antes de começar, faça uma revisão dos laboratórios da Quest "Kubernetes in Google Cloud".

Conhecimentos avaliados:


- Criar imagens do Docker em um host
- Executar contêineres do Docker em um host
- Armazenar imagens do Docker no Google Container Repository (GCR)
- Implantar imagens do GCR no Kubernetes
- Enviar atualizações para o Kubernetes
- Automatizar implantações no Kubernetes com o Jenkins


## Cenário do desafio

Você acabou de terminar o treinamento sobre como criar e gerenciar contêineres, e agora tem a chance de mostrar o que aprendeu. A equipe de desenvolvimento da Jooli Inc. precisa de ajuda com um novo projeto de ambiente de aplicativo que usa o Kubernetes. Uma parte do trabalho já foi feita, mas outras requerem os conhecimentos de um especialista.

Você deverá criar imagens de contêineres, armazená-las em um repositório e configurar um pipeline de CI/CD do Jenkins para automatizar o build do produto. Seu supervisor, Kurt, pedirá para você realizar estas tarefas:

- Criar uma imagem do Docker e armazenar o Dockerfile
- Testar a imagem do Docker
- Enviar a imagem do Docker ao Container Repository
- Usar a imagem para criar e expor uma implantação no Kubernetes
- Atualizar a imagem e enviar uma alteração para a implantação
- Criar um pipeline no Jenkins para implantar uma nova versão da sua imagem quando o código-fonte mudar

Veja algumas normas da Jooli Inc. que você precisa seguir:


- Crie todos os recursos na região `us-east1` e na zona `us-east1-b`, a menos que haja uma instrução diferente.

- Use as VPCs do projeto.

- Em geral, os nomes têm o formato equipe-recurso (ex.: __kraken-webserver1__).

- Economize recursos. Como os projetos são monitorados, o uso excessivo de recursos pode levar ao encerramento desse projeto (e talvez da sua função). Por isso, tenha cuidado. Esta é a orientação da equipe de monitoramento: a menos que haja uma instrução diferente, use `n1-standard-1`.


### Seu desafio

Assim que abrir o novo laptop na sua mesa, você receberá a solicitação a seguir para realizar as tarefas. Boa sorte!

> Não precisa esperar o laboratório ser provisionado. Você pode fazer as tarefas de 1 a 3 antes do término do provisionamento. Basta confirmar que a estação de trabalho existe antes de iniciar a tarefa 1.

## Tarefa 1: crie uma imagem do Docker e armazene o Dockerfile

Abra o Cloud Shell e execute source `<(gsutil cat gs://cloud-training/gsp318/marking/setup_marking.sh)`. Esse comando instalará scripts de marcação que você pode usar para verificar seu progresso.

Use o Cloud Shell para clonar o repositório do código-fonte `valkyrie-app` (ele está no seu projeto).

O código-fonte do app está em `valkyrie-app/source`. Crie `valkyrie-app/Dockerfile` e adicione a configuração abaixo.

```Dockerfile
FROM golang:1.10
WORKDIR /go/src/app
COPY source .
RUN go install -v
ENTRYPOINT ["app","-single=true","-port=8080"]
```

Use `valkyrie-app/Dockerfile` para criar uma imagem do Docker chamada __valkyrie-app__ com a tag __v0.0.1__.

Depois de criar a imagem do Docker, execute `step1.sh` para fazer a verificação local do seu trabalho. Após receber a resposta de que a marcação local foi concluída, veja seu progresso.


> Se não aparecer uma marca de seleção verde, clique em "Pontuação" no canto superior direito e em "Executar etapa" na etapa em questão. Você verá um pop-up com uma dica.

## Tarefa 2: teste a imagem do Docker

Inicie um contêiner com a imagem __valkyrie-app:v0.0.1__. Você precisa mapear a porta 8080 do host para a porta 8080 no contêiner. Adicione `&` ao final do comando para que o contêiner seja executado em segundo plano.

Quando o contêiner estiver em execução, você verá a página pela __Visualização na Web__.

Depois disso, execute `step2.sh` para fazer a verificação local do seu trabalho. Após receber a resposta de que a marcação local foi concluída, veja seu progresso.

> Se não aparecer uma marca de seleção verde, clique em "Pontuação" no canto superior direito e em "Executar etapa" na etapa em questão. Você verá um pop-up com uma dica.

## Tarefa 3: envie a imagem do Docker ao Container Repository

Envie a imagem do Docker __valkyrie-app:v0.0.1__ ao Container Registry.

Marque o contêiner novamente como __gcr.io/YOUR_PROJECT/valkyrie-app:v0.0.1__.


> Se não aparecer uma marca de seleção verde, clique em "Pontuação" no canto superior direito e em "Executar etapa" na etapa em questão. Você verá um pop-up com uma dica.

## Tarefa 4: crie e exponha uma implantação no Kubernetes

Kurt criou os arquivos `deployment.yaml` e `service.yaml` para implantar sua nova imagem de contêiner em um cluster do Kubernetes chamado valkyrie-dev. Os dois arquivos estão em `valkyrie-app/k8s`.

Lembre-se de que você precisa ter as credenciais do Kubernetes para implantar a imagem no cluster.

Antes de criar as implantações, revise os arquivos `deployment.yaml` e `service.yaml`. Kurt acredita que deixou alguns valores de marcador nos arquivos e que eles precisam ser definidos.


Verifique o balanceador de carga quando ele estiver disponível.

> Se não aparecer uma marca de seleção verde, clique em "Pontuação" no canto superior direito e em "Executar etapa" na etapa em questão. Você verá um pop-up com uma dica.

## Tarefa 5: atualize a implantação com uma nova versão da imagem valkyrie-app

Antes de implantar um novo código, evite interrupções aumentando o número de réplicas de um para três.

> Se não aparecer uma marca de seleção verde, clique em "Pontuação" no canto superior direito e em "Executar etapa" na etapa em questão. Você verá um pop-up com uma dica.

Kurt armazenou em uma ramificação chamada __kurt-dev__ as alterações que fez no código-fonte. Você precisa mesclar __kurt-dev__ em __master__. Use `git merge origin/kurt-dev`.

Gere o build com o novo código como a versão v0.0.2 de valkyrie-app, envie a imagem atualizada ao Container Repository e reimplante o cluster valkyrie-dev. Você saberá que tem a nova versão v0.0.2 se os títulos dos cards ficarem verdes.

> Se não aparecer uma marca de seleção verde, clique em "Pontuação" no canto superior direito e em "Executar etapa" na etapa em questão. Você verá um pop-up com uma dica.

## Tarefa 6: crie um pipeline no Jenkins para implantar seu app

Com o Jenkins, é possível automatizar a criação e o envio do contêiner para o repositório. Quando você fizer uma mudança no código-fonte, como a implantação do Jenkins no cluster `valkyrie-dev`, conecte-se ao Jenkins e configure um job para gerar o build.

Lembre-se de fazer o seguinte:

- Obtenha a senha com `printf $(kubectl get secret cd-jenkins -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo`.

- Use os comandos abaixo para se conectar ao console do Jenkins. Antes disso, elimine qualquer contêiner `docker ps` em execução, se houver.


```bash
export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/component=jenkins-master" -l "app.kubernetes.io/instance=cd" -o jsonpath="{.items[0].metadata.name}")
kubectl port-forward $POD_NAME 8080:8080 >> /dev/null &
```

- Defina suas credenciais para usar a __conta de serviço do Google com metadados__.

- Crie um job de pipeline apontando para a ramificação `*/master` no código-fonte.

Antes de confirmar e gerar o build, faça duas alterações nos arquivos:


- Edite `valkyrie-app/Jenkinsfile` e substitua YOUR_PROJECT pelo ID do seu projeto.

- Edite `valkyrie-app/source/html.go` e substitua as duas ocorrências "green" por "orange".

Use `git` para fazer o seguinte:


- Alterar os códigos e confirmar as mudanças na ramificação mestre

- Enviar as alterações de volta para o repositório

Quando estiver tudo pronto, acione um build manualmente. Como o primeiro demora um pouco, monitore o processo. O build substituirá os contêineres em execução por outros com tags diferentes. Você verá os cabeçalhos na cor laranja.

> Se não aparecer uma marca de seleção verde, clique em "Pontuação" no canto superior direito e em "Executar etapa" na etapa em questão. Você verá um pop-up com uma dica.

## Parabéns!

### Termine a Quest

Este laboratório autoguiado faz parte da Quest do Qwiklabs [Kubernetes in Google Cloud](https://google.qwiklabs.com/quests/29). Uma Quest é uma série de laboratórios relacionados que formam o programa de aprendizado. Ao concluir esta Quest, você ganha o selo acima como reconhecimento pela sua conquista. Você pode publicar seus selos e incluir um link para eles no seu currículo on-line ou nas redes sociais. Se você já fez este laboratório, inscreva-se nesta Quest para receber os créditos de conclusão imediatamente. [Veja outras Quests do Qwiklabs](https://google.qwiklabs.com/catalog).


### Comece o próximo laboratório

Este laboratório também faz parte de uma série chamada "laboratórios com desafio". Eles foram feitos para avaliar o que você aprendeu sobre o Google Cloud. Procure "laboratório com desafio" no catálogo de laboratórios para testar suas habilidades.

