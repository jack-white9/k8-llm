# k8-llm

Deploys a scalable LLM microservice using Kubernetes.

## 📚 Getting started

### Prerequisites

1. [minikube](https://minikube.sigs.k8s.io/docs/)
2. [kubectl](https://kubernetes.io/docs/reference/kubectl/)

### Setup

1. Start minikube

```sh
minikube start
```

2. Point shell to minikube's Docker daemon

```sh
eval $(minikube docker-env)
```

3. Build Docker image

```sh
docker build -t llm-api .
```

### Start deployment

1. Create deployment

```sh
kubectl apply -f kubernetes/deployment.yaml
```

2. Verify that two replica pods have been created

```sh
kubectl get pod

# should return something similar to:
#
# NAME                                   READY   STATUS    RESTARTS   AGE
# llm-api-deployment-799f65478b-9drrm   1/1     Running   0          7s
# llm-api-deployment-799f65478b-j4vwb   1/1     Running   0          7s
```

### Start service

1. Create service

```sh
kubectl apply -f kubernetes/service.yaml
```

2. Verify that service has been created

```sh
kubectl get service

# should return something similar to:
#
# NAME               TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
# kubernetes         ClusterIP      <ip>           <none>        443/TCP          65m
# llm-api-service   LoadBalancer   <ip>           <pending>     3000:30080/TCP   33m
```

3. Test service

```sh
minikube service llm-api-service --url

# using a seperate terminal instance
curl http://<external_service_ip>:30080 # returns "Hello World"
```
