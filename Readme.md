
```shell
# Install Postgres
kubectl get namespace
kubectl create namespace postgres
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install -n postgres my-postgresql bitnami/postgresql --version 15.5.7
# Note: my-postgresql.postgres.svc.cluster.local
# Note: auth: postgres/iaQC75CBk3 db:postgres

# Install Redis
kubectl create namespace redis
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install -n redis my-redis bitnami/redis --version 19.5.5
## Note: my-redis-master.redis.svc.cluster.local for read/write operations (port 6379)
## Note: my-redis-replicas.redis.svc.cluster.local for read-only operations (port 6379)
## password: JhFlBZdJxC

# Install ArgoCD Server
kubectl create namespace argocd
helm install -n argocd my-argocd oci://ghcr.io/argoproj/argo-helm/argo-cd --version 7.1.4 --values argocd.values.yml
## Note: kubectl get crd
## Note: kubectl delete crd applications.argoproj.io
## admin/11111111

# Install Gitlab Server (Testing)
helm create gitlab
helm package charts/gitlab

kubectl create namespace gitlab
helm install -n gitlab my-gitlab charts/gitlab --values gitlab.values.yml
helm upgrade -n gitlab my-gitlab charts/gitlab --values gitlab.values.yml
helm uninstall my-gitlab -n gitlab
## admin/

# # Install Gitlab Server (PROD)
kubectl create namespace gitlab
helm repo add gitlab http://charts.gitlab.io/
helm install -n gitlab my-gitlab gitlab/gitlab --version 8.0.2 --values gitlab.values.yml
helm uninstall my-gitlab -n gitlab
## admin/11111111

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