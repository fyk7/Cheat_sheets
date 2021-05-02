```sh
# Kubernetes on AWSで使用したコマンド
kubectl get all
kubectl get pods
kubectl get services
kubectl get nodes
kubectl config get-contexts
kubectl config use-context eks-work
kubectl config set-context eks-work --cluster eks-work-cluster.ap-northeast-1.eksctl.io --user XXXXXXXXXX@eks-work-cluster.ap-northeast-1.eksctl.io --namespace eks-work
kubectl port-forward nginx-pod 8080:80
kubectl delete service backend-app-service
kubectl delete pod nginx-pod
kubectl delete deployment backend-app
kubectl delete cronjob batch-app
kubectl apply -f 23_service_backend-app_k8s.yaml
kubectl apply -f 20_create_namespace_k8s.yaml
kubectl apply -f 02_nginx_k8s.yaml
ECR_HOST=XXXXXXXXXXX.dkr.ecr.ap-northeast-1.amazonaws.com envsubst < 43_cronjob_k8s.yaml.template | kubectl apply -f -
ECR_HOST=XXXXXXXXXXX.dkr.ecr.ap-northeast-1.amazonaws.com nenvsubst < 22_deployment_backend-app_k8s.yaml.template | kubectl apply -f -
DB_URL=jdbc:postgresql://eks-work-db.XXXXXXXXXX.ap-northeast-1.rds.amazonaws.com/myworkdb DB_PASSWORD='XXXXXXXXXXXX' envsubst < 21_db_config_k8s.yaml.template | kubectl apply -f -
BUCKET_SUFFIX=XXXXXXX envsubst < 41_config_map_batch_k8s.yaml.template | kubectl apply -f -
BUCKET_SUFFIX=XXXXXXX envsubst < 41_config_map_batch_k8s.yaml.template | kubectl apply -f -
AWS_ACCESSKEY=XXXXXXXXXXXXXX AWS_SECRETKEY=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX envsubst < 42_batch_secrets_k8s.yaml.template | kubectl apply -f -
history 100| awk '{$1=""; print $0}' | grep kubectl | sort | uniq -c | sort -r
```
