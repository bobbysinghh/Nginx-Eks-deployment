[root@ip-172-31-95-94 ~]# aws configure --profile  eks
AWS Access Key ID [****************BVGA]:
AWS Secret Access Key [****************wH0D]:
Default region name [us-east-1]:
Default output format [None]:
----------------------------------------------------------------------------------------------------------------------------------------
eksctl create cluster --name mycluster123 --node-type m5.large --managed --region us-east-2  --profile eks
----------------------------------------------------------------------------------------------------------------------------------------
eksctl utils associate-iam-oidc-provider --cluster mycluster123 --approve
----------------------------------------------------------------------------------------------------------------------------------------
wget -O alb-ingress-iam-policy.json  https://raw.githubusercontent.com/kubernetes-sigs/aws-alb-ingress-controller/master/docs/examples/iam-policy.json
----------------------------------------------------------------------------------------------------------------------------------------
aws iam create-policy --policy-name ALBIngressControllerIAMPolicy --policy-document file://alb-ingress-iam-policy.json

take AWS_POLICY_ARN id and save it somewhere it will need while create iamserviceaccount.
----------------------------------------------------------------------------------------------------------------------------------------
kubectl apply -f cluster-roles-binding.yaml
----------------------------------------------------------------------------------------------------------------------------------------
aws sts get-caller-identity
----------------------------------------------------------------------------------------------------------------------------------------
eksctl create iamserviceaccount \
--name alb-ingress-controller \
--namespace kube-system \
--cluster mycluster123 \
--attach-policy-arn   ENTER_AWS_POLICY_ARN \
--region us-east-2 \
--approve
----------------------------------------------------------------------------------------------------------------------------------------
kubectl apply -f alb-ingress-controller.yaml
----------------------------------------------------------------------------------------------------------------------------------------
kubectl get deploy -n kube-system
----------------------------------------------------------------------------------------------------------------------------------------
kubectl get deploy -n kube-system
----------------------------------------------------------------------------------------------------------------------------------------
kubectl get pods  -n kube-system
----------------------------------------------------------------------------------------------------------------------------------------
kubectl apply -f nginx-configmap.yml
----------------------------------------------------------------------------------------------------------------------------------------
kubectl apply -f nginx-deployment.yml
----------------------------------------------------------------------------------------------------------------------------------------
kubectl apply -f nginx-service.yml
----------------------------------------------------------------------------------------------------------------------------------------
kubectl apply -f ingress.yaml
----------------------------------------------------------------------------------------------------------------------------------------
kubectl get ingress 
enter the domain_name of ingress in url section of browser to view the outputs.

----------------------------------------------------------------------------------------------------------------------------------------