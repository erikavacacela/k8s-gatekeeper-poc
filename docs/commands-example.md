```
➜  k8s-gatekeeper-poc git:(main) ✗ kubectl apply -f constraint_templates/k8svalidregistry_template.yaml
constrainttemplate.templates.gatekeeper.sh/k8svalidregistry created

➜  k8s-gatekeeper-poc git:(main) ✗ kubectl apply -f contraints/all_pods_must_have_valid_registry.yaml
k8svalidregistry.constraints.gatekeeper.sh/images-must-have-valid-registry created

➜  k8s-gatekeeper-poc git:(main) ✗ kubectl apply -f examples/bad/simple_pod_invalid_registry.yaml
Error from server (Forbidden): error when creating "examples/bad/simple_pod_invalid_registry.yaml": admission webhook "validation.gatekeeper.sh" denied the request: [images-must-have-valid-registry] container image refers to illegal registry (must be 984724474376.dkr.ecr.us-east-2.amazonaws.com/)

➜  k8s-gatekeeper-poc git:(main) ✗ kubectl apply -f examples/good/simple_pod_valid_registry.yaml
pod/custom-nginx created

➜  k8s-gatekeeper-poc git:(main) ✗ kubectl get pods
NAME           READY   STATUS              RESTARTS   AGE
custom-nginx   0/1     ContainerCreating   0          4s

➜  k8s-gatekeeper-poc git:(main) ✗ kubectl get pods
NAME           READY   STATUS    RESTARTS   AGE
custom-nginx   1/1     Running   0          8s

➜  k8s-gatekeeper-poc git:(main) ✗ kubectl port-forward pods/custom-nginx 8080:80
Forwarding from 127.0.0.1:8080 -> 80
Forwarding from [::1]:8080 -> 80
Handling connection for 8080

```