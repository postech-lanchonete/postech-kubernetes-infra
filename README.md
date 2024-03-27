# 🍔 Lanchonete do Bairro - Kubernetes Infrastructure

## Kubernetes

<p align="justify">
  O Kubernetes foi escolhido como sistema de orquestração de contêineres por oferecer recursos avançados para gerenciar aplicativos em ambientes de produção. Com recursos como escalabilidade automática, implantação declarativa e tolerância a falhas, o Kubernetes simplifica e automatiza o gerenciamento de aplicativos em contêineres, permitindo alta disponibilidade e escalabilidade. Sua arquitetura modular e extensível, juntamente com a ativa comunidade de desenvolvedores, tornam o Kubernetes uma escolha confiável para empresas que buscam uma solução robusta e escalável para executar e gerenciar seus aplicativos em contêineres.
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/postech-lanchonete/.github/cc2cb83e979e24eb2e68a7922c901f8773379bdf/profile/figura_2_kube.svg" />
</p>

Figura 2 - Visão geral da arquitetura do Kubernetes
<details>
  <summary>Explicação da imagem</summary>
  <ol>
    <li>Requisição Externa: A requisição inicial é originada externamente e direcionada ao sistema. </li>
    <li>Service "lanchonetebairro": Este serviço atua como ponto de entrada para a aplicação principal. O arquivo de configuração "<b>lanchonetebairro-svc.yaml</b>" define esse serviço, que desempenha um papel fundamental no roteamento da requisição para o destino adequado.</li>
    <li>Deployment: Esta camada consiste nos pods que compõem a aplicação. O arquivo de configuração "<b>lanchonetebairro-deployment.yaml</b>" define como esses pods são criados e escalados.</li>
    <li>HPA (Horizontal Pod Autoscaler): O arquivo de configuração "<b>lanchonetebairro-hpa.yaml</b>" define o HPA, que é responsável por ajustar automaticamente o número de réplicas dos pods com base na carga de trabalho, garantindo escalabilidade.</li>
    <li>Pods: São as instâncias individuais da aplicação que foram criadas de acordo com as especificações definidas no <b>deployment</b>.</li>
    <li>Service do banco de dados MariaDB: O arquivo "<b>db-svc.yaml</b>" configura o service que fornece uma interface para acessar o banco de dados MariaDB.</li>
    <li>Pod do MariaDB: O arquivo "<b>db-pod.yaml</b>" define o pod responsável por executar o banco de dados MariaDB.</li>
    <li>ConfigMap: O arquivo "<b>db.configmap.yaml</b>" define o ConfigMap, que é utilizado para armazenar e fornecer configurações para os componentes do sistema, como senhas e outras informações sensíveis.</li>
  </ol>

  <p align="justify">
    Essas camadas e serviços trabalham juntos para garantir que a requisição seja encaminhada corretamente, que a aplicação esteja disponível, escalável e que haja uma conexão adequada com o banco de dados.
  </p>
</details>

## Como rodar o projeto

Uma vez que o Docker esteja rodando com as configurações necessárias para a utilizacao do kubernetes, ou minikube, rode o seguinte comando:

Como estamos usando minikube para o desenvolvimento, é necessário rodar o seguinte comando para que o minikube possa usar as imagens do docker:

```shell
minikube start
```

Uma vez que o minikube esteja rodando, rode o seguinte comando para subir a infraestrutura do Kubernetes:

```sh
kubectl apply -f infra
```

Este comando irá subir toda a infraestrutura do Kubernetes, como os arquivos contendo os secrets, zookeeper, kafka e banco de dados necessários.

Caso queira, você pode acompanhar a subida e o estado de cada POD com o seguinte comando:

```sh
kubectl get pods --watch
```

Como a infra entre kafka e zookeeper está em um namespace separado, para conseguir ver os PODs, é necessário rolar um comando diferente.

```sh
kubectl get pods --all-namespaces  --watch
```

Uma vez que os PODs de infraestrutura estejam rodando, rode o comando para que os manifestos (services e deployments) subam.


```sh
kubectl apply -f manifests
```

Como estamos usando minikube, a porta da aplicação deve ser descoberta antes de podermos usar o sistema. Para isso, rode o comando a seguir que retornará a porta onde a aplicação está rodando:

```sh
minikube service postech-pedidos-service --url
```

Este comando deve estar rodando em um terminal separado pois ele cria um túnel entre a porta do seu ambiente local e o serviço Kubernetes dentro do cluster Minikube. Esse túnel permite que as solicitações feitas à porta local sejam redirecionadas para o serviço dentro do cluster.

Feito isso, entre no Swagger utilizando a url fornecida `/swagger-ui/index.html`.

Existem 5 servicos, por isso voce precisaria acessar cada um deles para obter a porta correta. Estes são os comandos, caso precise:

```sh
minikube service postech-clientes-service --url
```

```sh
minikube service postech-pagamento-service --url
```

```sh
minikube service postech-pedidos-service --url
```

```sh
minikube service postech-producao-service --url
```

```sh
minikube service postech-produtos-service --url
```

Ao final, para parar todos os serviços rode o seguinte comando:

```sh
kubectl delete -f infra
kubectl delete -f manifests
```
