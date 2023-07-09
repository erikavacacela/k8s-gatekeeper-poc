# k8s-gatekeeper-poc
This example describes how to use gatekeeper in k8s to compliance policy to deny objects that include container images referring to illegal registries.

**Required reading**

* [OPA + k8s](https://www.openpolicyagent.org/docs/latest/kubernetes-introduction/)
* [How to use Gatekeeper](https://open-policy-agent.github.io/gatekeeper/website/docs/howto)

## Running locally

**Prerequisites**
* Minikube

1. Install Gatekeeper

```shell
helm repo add gatekeeper https://open-policy-agent.github.io/gatekeeper/charts
helm install gatekeeper/gatekeeper --name-template=gatekeeper --namespace gatekeeper-system --create-namespace


2. Install constraint template

```shell
kubectl apply -f constraint_templates/k8svalidregistry_template.yaml
```

3. Install constraint

```shell
kubectl apply -f contraints/all_pods_must_have_valid_registry.yaml
```

4. Try to deploy a pod with invalid registry
```shell
kubectl apply -f examples/bad/simple_pod_invalid_registry.yaml
```

5. Try to deploy pod with valid registry

Build a docker image with a valid registry
```shell
docker build -t evacacela/hello-world:0.0.1 -f examples/good/web-app/Dockerfile examples/good/web-app/
```

Using Local Docker Images With Minikube
```shell
minikube docker-env
eval $(minikube -p minikube docker-env)
```
Deploy pod
```shell
kubectl appy -f examples/good/simple_pod_valid_registry.yaml
```

Show pods
```shell
kubectl get pods
```

Access to deployed web app
```shell
kubectl port-forward pods/custom-nginx 8080:80
```

Open browser
[http://localhost:8080](http://localhost:8080)


## Running with EKS and ECR

**Prerequisites**

* AWS Account
* Install eks cluster
* Setup credentials


1. Install Gatekeeper

```shell
helm repo add gatekeeper https://open-policy-agent.github.io/gatekeeper/charts
helm install gatekeeper/gatekeeper --name-template=gatekeeper --namespace gatekeeper-system --create-namespace
```

2. Install constraint template

```shell
kubectl apply -f constraint_templates/k8svalidregistry_template.yaml
```

3. Install constraint

Note: remplace [`evacacela/`](contraints/all_pods_must_have_valid_registry.yaml) with ECR registry

```shell
kubectl apply -f contraints/all_pods_must_have_valid_registry.yaml
```

4. Try to deploy a pod with invalid registry
```shell
kubectl apply -f examples/bad/simple_pod_invalid_registry.yaml
```

5. Try to deploy pod with valid registry

* [Build and push image to ECR](.github/workflows/docker-publish.yml)
* Reemplace [`evacacela/hello-world:0.0.1`](examples/good/simple_pod_valid_registry.yaml) value with ECR image name.
* Deploy pod
```shell
kubectl apply -f examples/good/simple_pod_valid_registry.yaml
```

Show pods
```shell
kubectl get pods
```

Access to deployed web app
```shell
kubectl port-forward pods/custom-nginx 8080:80
```


