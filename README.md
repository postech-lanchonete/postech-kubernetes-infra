# 🍔 Lanchonete do Bairro - Kubernetes Infrastructure

## Kubernetes

<p align="justify">
  O Kubernetes foi escolhido como sistema de orquestração de contêineres por oferecer recursos avançados para gerenciar aplicativos em ambientes de produção. Com recursos como escalabilidade automática, implantação declarativa e tolerância a falhas, o Kubernetes simplifica e automatiza o gerenciamento de aplicativos em contêineres, permitindo alta disponibilidade e escalabilidade. Sua arquitetura modular e extensível, juntamente com a ativa comunidade de desenvolvedores, tornam o Kubernetes uma escolha confiável para empresas que buscam uma solução robusta e escalável para executar e gerenciar seus aplicativos em contêineres.
</p>

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
