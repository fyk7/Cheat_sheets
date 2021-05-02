```sh
# northeastをnorthestと記述したことでハマった。typoに注意。
eksctl create cluster \
--vpc-public-subnets subnet-XXXXXXXXXX,subnet-YYYYYYYYYY,subnet-ZZZZZZZZZ \
--name eks-work-cluster \
--region ap-northest-1 \
--version 1.19 \
--nodegroup-name eks-work-nodegroup \
--node-type t2.small \
--nodes 2 \
--nodes-min 2 \
--nodes-max 5 \
--profile another-profile

kubectl config set-context eks-work \
--cluster eks-work-cluster.ap-northeast-1.eksctl.io \
--user XXXXXXXXX@eks-work-cluster.ap-northeast-1.eksctl.io \
--namespace eks-work

eksctl delete cluster --name eks-work-cluster
```
