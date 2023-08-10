# eks-cluster-access-with-iam

```
kubectl -n kube-system edit cm aws-auth

apiVersion: v1
data:
  mapRoles: |
    - groups:
      - system:bootstrappers
      - system:nodes
      - system:node-proxier
      rolearn: arn:aws:iam::051725806636:role/eks-fargate-profile-example
      username: system:node:{{SessionName}}
    - groups:
      - system:bootstrappers
      - system:nodes
      rolearn: arn:aws:iam::051725806636:role/uat-munna-rnd-worker
      username: system:node:{{EC2PrivateDNSName}}
    - groups:
      - system:masters
      rolearn: arn:aws:iam::051725806636:role/uat-munna-rnd-worker
      username: "role-developer"
kind: ConfigMap
metadata:
  creationTimestamp: "2023-08-10T04:55:45Z"
  name: aws-auth
  namespace: kube-system
  resourceVersion: "64383"
  uid: 049ba604-27fe-4ae5-9cf3-2327522217ba
```
