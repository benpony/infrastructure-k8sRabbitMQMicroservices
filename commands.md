
- - -
$ minikube delete
$ minikube start --vm-driver=virtualbox --memory 4096 --cpus 2
$ minikube addons enable ingress
- - -

# install rabbitmq
https://github.com/bitnami/charts/
..
$ helm repo add bitnami https://charts.bitnami.com/bitnami
$ helm search repo bitnami | grep "rabbitmq"
$ helm install rabbitmq bitnami/rabbitmq
$ helm delete rabbitmq bitnami/rabbitmq

```
$ helm upgrade --install rabbitmq \
    --set ingress.enabled=true \
    --set ingress.hostName=rabbitmq.local \
    --set auth.username=admin \
    --set auth.password=admin \
    --set auth.erlangCookie=secretpassword \
    bitnami/rabbitmq
```

## output
NAME: rabbitmq
LAST DEPLOYED: Tue Sep 29 19:57:35 2020
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
** Please be patient while the chart is being deployed **

Credentials:

    echo "Username      : admin"
    echo "Password      : $(kubectl get secret --namespace default rabbitmq -o jsonpath="{.data.rabbitmq-password}" | base64 --decode)"
    echo "ErLang Cookie : $(kubectl get secret --namespace default rabbitmq -o jsonpath="{.data.rabbitmq-erlang-cookie}" | base64 --decode)"

RabbitMQ can be accessed within the cluster on port  at rabbitmq.default.svc.

To access for outside the cluster, perform the following steps:

To Access the RabbitMQ AMQP port:

1. Create a port-forward to the AMQP port:

    kubectl port-forward --namespace default svc/rabbitmq 5672:5672 &
    echo "URL : amqp://127.0.0.1:5672/"

2. Access RabbitMQ using using the obtained URL.

To Access the RabbitMQ Management interface:

1. Get the RabbitMQ Management URL and associate its hostname to your cluster external IP:

   export CLUSTER_IP=$(minikube ip)
   echo "RabbitMQ Management: http://rabbitmq.local/"
   echo "$CLUSTER_IP  rabbitmq.local" | sudo tee -a /etc/hosts

2. Open a browser and access RabbitMQ Management using the obtained URL.
---


# build docker images
$ docker build -t <your username>/image-name .
$ docker image rm <your username>/image-name

```
/producer 
$ docker build -t benpony/py-producer . && \
  docker push benpony/py-producer:latest 

/consumer 
$ docker build -t benpony/py-consumer . && \
  docker push benpony/py-consumer:latest 
```

# deployment via helm install
$ helm install -f myvalues.yaml my-release-name .
$ helm install my-release-name .
$ helm uninstall my-release-name 

```
$ helm install producer ./producer/deployment
$ helm install consumer ./consumer/deployment
..
$ helm upgrade --install producer ./deployment
$ helm upgrade --install consumer ./deployment

$ helm uninstall py-rmq-producer && \
  helm install py-rmq-producer ./deployment
```


CI / CD
 - - - - - -
 install jenkins
 https://github.com/bitnami/charts/tree/master/bitnami/jenkins/#installing-the-chart

```
$ helm install jenkins \
    --set ingress.enabled=true \
    --set ingress.hostName=jenkins.local \
    --set jenkinsUser=admin \
    --set jenkinsPassword=admin \
    bitnami/jenkins

$ helm install jenkins \
    --set master.ingress.enabled=true\
    --set master.ingress.hostName=jenkins.local \
    --set master.adminUser=admin \
    --set master.adminPassword=admin \
    jenkins/jenkins
```





- - -
install chart registry on local cluster
https://github.com/avielb/k8s-experts/blob/master/helm/deploy-repository.txt
..
# assert valid chart
```
$ helm install test --debug --dry-run ./  
```

```
$ helm install chartmuseum stable/chartmuseum
$ helm plugin install https://github.com/chartmuseum/helm-push
$ helm repo add chartmuseum http://$(minikube ip):32688
$ helm repo update
$ helm push mychart-0.1.0.tgz chartmuseum
```

## helm install output
NAME: chartmuseum
LAST DEPLOYED: Sat Sep 26 18:57:05 2020
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
** Please be patient while the chart is being deployed **

Get the ChartMuseum URL by running:

  export POD_NAME=$(kubectl get pods --namespace default -l "app=chartmuseum" -l "release=chartmuseum" -o jsonpath="{.items[0].metadata.name}")
  echo http://127.0.0.1:8080/
  kubectl port-forward $POD_NAME 8080:8080 --namespace default

--> http://192.168.99.107:32688/

- - -
create, upload, pull, install chart
$ helm package deployment/
$ helm push deployment/ chartmuseum
$ helm repo update
$ helm pull chartmuseum/rmqapps-consumer
$ helm upgrade --install consumer chartmuseum/rmqapps-consumer

..

# give permissions for jenkins service account on default namespace
```
$ kubectl apply -f jenkins-cluster-role.yaml 
$ kubectl apply -f jenkins/jenkins-service-account.yaml 
$ kubectl create clusterrolebinding service-reader-pod --clusterrole=service-reader --serviceaccount=default:jenkins
$ kubectl describe clusterrolebinding service-reader-pod
```
#output
Name:         service-reader-pod
Labels:       <none>
Annotations:  <none>
Role:
  Kind:  ClusterRole
  Name:  service-reader
Subjects:
  Kind            Name     Namespace
  ----            ----     ---------
  ServiceAccount  jenkins  default


