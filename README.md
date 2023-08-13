# eks-cluster-access-with-iam

## Steps-1: Launch a bastion host and assign a iam role (e.g. arn:aws:iam::Account-ID:role/uat-munna-rnd-worker)
## Steps-2: Login in to jenkins or eks creator host. Create a role/rolebinding with required permission (pod,deploymen,secret/get,create,delete etc)
## Steps-3: edit existing aws-auth configmap and add a new entry under mapRoles with groups name and role arn. if we set group as system:master then bastion host will get full access, if we set our created role/rolebinding (e.g. developers) then bastion host will get only specified permission. 


```
kubectl -n kube-system edit cm aws-auth

apiVersion: v1
data:
  mapRoles: |
    - groups:
      - system:bootstrappers
      - system:nodes
      - system:node-proxier
      rolearn: arn:aws:iam::Account-ID:role/eks-fargate-profile-example
      username: system:node:{{SessionName}}
    - groups:
      - system:bootstrappers
      - system:nodes
      rolearn: arn:aws:iam::Account-ID:role/uat-munna-rnd-worker
      username: system:node:{{EC2PrivateDNSName}}
    - groups:
      - developers
      rolearn: arn:aws:iam::Account-ID:role/uat-munna-rnd-worker
      username: "role-developer"
kind: ConfigMap
metadata:
  creationTimestamp: "2023-08-10T04:55:45Z"
  name: aws-auth
  namespace: kube-system
  resourceVersion: "64383"
  uid: 049ba604-27fe-4ae5-9cf3-2327522217ba
```
