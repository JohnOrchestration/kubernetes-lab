```shell
minikube delete
minikube start --driver=virtualbox --no-vtx-check --dns-proxy --cpus 6 --memory 18000 --disk-size=100Gb
minikube addons enable ingress
minikube addons enable ingress-dns

Add-DnsClientNrptRule -Namespace ".test" -NameServers "$(minikube ip)"

```


```shell
# Install Postgres
kubectl get namespace
kubectl create namespace postgres
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install -n postgres my-postgresql bitnami/postgresql --version 15.5.7 --values postgres.values.yml
# Note: my-postgresql.postgres.svc.cluster.local

# Install Redis
kubectl create namespace redis
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install -n redis my-redis bitnami/redis --version 19.5.5 --values redis.values.yml
## Note: my-redis-master.redis.svc.cluster.local for read/write operations (port 6379)
## Note: my-redis-replicas.redis.svc.cluster.local for read-only operations (port 6379)
## Note: url: redis://:JhFlBZdJxC@my-redis-master.redis.svc.cluster.local:6379

# Install ArgoCD Server
kubectl create namespace argocd
helm install -n argocd my-argocd oci://ghcr.io/argoproj/argo-helm/argo-cd --version 7.1.4 --values argocd.values.yml
helm uninstall my-argocd -n argocd
## Note: kubectl get crd
## Note: kubectl delete crd applications.argoproj.io

# Install Gitlab Server (Testing)
helm package charts/gitlab

kubectl create namespace gitlab
helm install -n gitlab my-gitlab charts/gitlab --values gitlab.values.dev.yml
helm uninstall my-gitlab -n gitlab

helm install -n gitlab my-gitlab-runner gitlab/gitlab-runner --version 0.66.0 --values gitlab-runner.values.yml


# # Install Gitlab Server (PROD)
kubectl create namespace gitlab
helm repo add gitlab http://charts.gitlab.io/
helm install -n gitlab my-gitlab gitlab/gitlab --version 8.0.2 --values gitlab.values.yml
helm uninstall my-gitlab -n gitlab

# Install Habor
kubectl create namespace harbor
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install -n harbor my-harbor bitnami/harbor --version 21.4.6 --values habor.values.yml
helm upgrade -n harbor my-harbor bitnami/harbor --version 21.4.6 --values habor.values.yml
## admin/uP16djN2iL

```

```shell
127.0.0.1 john.gitlab.com
127.0.0.1 john.harbor.com
127.0.0.1 john.argo.com
```