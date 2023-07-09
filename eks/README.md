

# Gatekeeper with EKS and ECR

**Prerequisites**

* AWS Account
* Install eks cluster (do the steps from folder a-, b-, and c)
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


