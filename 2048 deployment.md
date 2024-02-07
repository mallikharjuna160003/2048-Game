# Deploying a 2048 Game Application using the EKS cluster
- install kubectl, aws cli, eks cli from the official documentation.
- aws configure in local termnial for this we need aws access key ID
   - Go to your profile and click on security credentials then create access keys
     
kubectl installation: https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/
EKS CLI installation: https://eksctl.io/installation/#
AWS CLI installation: https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

```sh
$ aws configure
AWS Access Key ID [None]: 
AWS Secret Access Key [None]: 
Default region name [None]: us-west-1
Default output format [None]: 
```
create EKS cluster using eksctl in terminal
```sh
% eksctl create cluster --name demo-cluster --region us-east-1 --fargate
using fargate here so it will control  the eks cluster master nodes up we can also use ec2 instances. It will take 15 minutes
By running the below command I got the below error as I have given wrong aws configure values make sure you give exact values download the secret key while creating access key.
 eksctl create cluster --name demo-cluster --region us-east-1 --fargate
Error: checking AWS STS access – cannot get role ARN for current session: operation error STS: GetCallerIdentity, https response error StatusCode: 403, RequestID: 9d771030-07fc-4d35-9299-79f831dcec44, api error InvalidClientTokenId: The security token included in the request is invalid.
After the updating aws configure run the create ekscluster command
```

```sh
eksctl create cluster --name demo-cluster --region us-east-1 --fargate
2024-02-07 22:48:19 [ℹ]  eksctl version 0.170.0
2024-02-07 22:48:19 [ℹ]  using region us-east-1
2024-02-07 22:48:20 [ℹ]  setting availability zones to [us-east-1c us-east-1a]
2024-02-07 22:48:20 [ℹ]  subnets for us-east-1c - public:192.168.0.0/19 private:192.168.64.0/19
2024-02-07 22:48:20 [ℹ]  subnets for us-east-1a - public:192.168.32.0/19 private:192.168.96.0/19
2024-02-07 22:48:20 [ℹ]  using Kubernetes version 1.27
2024-02-07 22:48:20 [ℹ]  creating EKS cluster "demo-cluster" in "us-east-1" region with Fargate profile
2024-02-07 22:48:20 [ℹ]  if you encounter any issues, check CloudFormation console or try 'eksctl utils describe-stacks --region=us-east-1 --cluster=demo-cluster'
2024-02-07 22:48:20 [ℹ]  Kubernetes API endpoint access will use default of {publicAccess=true, privateAccess=false} for cluster "demo-cluster" in "us-east-1"
2024-02-07 22:48:20 [ℹ]  CloudWatch logging will not be enabled for cluster "demo-cluster" in "us-east-1"
2024-02-07 22:48:20 [ℹ]  you can enable it with 'eksctl utils update-cluster-logging --enable-types={SPECIFY-YOUR-LOG-TYPES-HERE (e.g. all)} --region=us-east-1 --cluster=demo-cluster'
2024-02-07 22:48:20 [ℹ]  
2 sequential tasks: { create cluster control plane "demo-cluster", 
    2 sequential sub-tasks: { 
        wait for control plane to become ready,
        create fargate profiles,
    } 
}
2024-02-07 22:48:20 [ℹ]  building cluster stack "eksctl-demo-cluster-cluster"
2024-02-07 22:48:21 [ℹ]  deploying stack "eksctl-demo-cluster-cluster"
2024-02-07 22:48:51 [ℹ]  waiting for CloudFormation stack "eksctl-demo-cluster-cluster"
2024-02-07 22:49:23 [ℹ]  waiting for CloudFormation stack "eksctl-demo-cluster-cluster"
2024-02-07 22:50:24 [ℹ]  waiting for CloudFormation stack "eksctl-demo-cluster-cluster"
2024-02-07 22:51:25 [ℹ]  waiting for CloudFormation stack "eksctl-demo-cluster-cluster"
2024-02-07 22:52:26 [ℹ]  waiting for CloudFormation stack "eksctl-demo-cluster-cluster"
2024-02-07 22:53:27 [ℹ]  waiting for CloudFormation stack "eksctl-demo-cluster-cluster"
2024-02-07 22:54:28 [ℹ]  waiting for CloudFormation stack "eksctl-demo-cluster-cluster"

2024-02-07 22:55:29 [ℹ]  waiting for CloudFormation stack "eksctl-demo-cluster-cluster"
2024-02-07 22:56:30 [ℹ]  waiting for CloudFormation stack "eksctl-demo-cluster-cluster"
2024-02-07 22:57:31 [ℹ]  waiting for CloudFormation stack "eksctl-demo-cluster-cluster"
2024-02-07 22:58:33 [ℹ]  waiting for CloudFormation stack "eksctl-demo-cluster-cluster"
2024-02-07 23:00:41 [ℹ]  creating Fargate profile "fp-default" on EKS cluster "demo-cluster"
2024-02-07 23:05:02 [ℹ]  created Fargate profile "fp-default" on EKS cluster "demo-cluster"
2024-02-07 23:05:33 [ℹ]  "coredns" is now schedulable onto Fargate
2024-02-07 23:06:41 [ℹ]  "coredns" is now scheduled onto Fargate
2024-02-07 23:06:41 [ℹ]  "coredns" pods are now scheduled onto Fargate
2024-02-07 23:06:41 [ℹ]  waiting for the control plane to become ready
2024-02-07 23:06:42 [✔]  saved kubeconfig as "/home/sunkara/.kube/config"
2024-02-07 23:06:42 [ℹ]  no tasks
2024-02-07 23:06:42 [✔]  all EKS cluster resources for "demo-cluster" have been created
2024-02-07 23:06:43 [ℹ]  kubectl command should work with "/home/sunkara/.kube/config", try 'kubectl get nodes'
2024-02-07 23:06:43 [✔]  EKS cluster "demo-cluster" in "us-east-1" region is ready
```
After cluster creation 
![Screenshot from 2024-02-07 23-06-07](https://github.com/mallikharjuna160003/2048-Game/assets/74324685/9e1abc33-2a8a-43b3-b284-9d744b16fd53)

These are fields available after creating the EKS cluster

- API server endpoint
- OpenID Connect provider URL
- Cluster IAM role ARN
- Cluster ARN
- Certificate authority

Fargate nodes
![image](https://github.com/mallikharjuna160003/2048-Game/assets/74324685/ea7d9978-50eb-47f6-8066-95b7eeb8c77a)

fargate profile is used for creating in instances within the namespaces to deploy into other namespaces we can create other namespaces as well.
update the aws cli with newly created cluster information
```sh
$ aws eks update-kubeconfig --name demo-cluster --region us-east-1
Added new context arn:aws:eks:us-east-1:<account-id>:cluster/demo-cluster to /home/sunkara/.kube/config
```

```sh
eksctl create fargateprofile \
    --cluster demo-cluster \
    --region us-east-1 \
    --name alb-sample-app \   -> name of the fargate profile
    --namespace game-2048  -->this is newly creating namespace
```
![image](https://github.com/mallikharjuna160003/2048-Game/assets/74324685/c511f109-db3f-4ea8-81e6-1a03d5ebd370)

After creating the alb-sample-app fargate profile 
![image](https://github.com/mallikharjuna160003/2048-Game/assets/74324685/5353bd35-1737-4e28-9e1e-378f77498e69)

To get the pods
```sh
$ kubectl get pods -n game-2048 
NAME                                           READY   STATUS    RESTARTS   AGE
eks-sample-linux-deployment-5b568bf897-fkjl2   0/1     Pending   0          38s
eks-sample-linux-deployment-5b568bf897-w98pf   0/1     Pending   0          38s
eks-sample-linux-deployment-5b568bf897-wmlw5   0/1     Pending   0          38s
```
```sh
$ kubectl get pods -n game-2048 
NAME                                           READY   STATUS    RESTARTS   AGE
eks-sample-linux-deployment-5b568bf897-fkjl2   0/1     Pending   0          38s
eks-sample-linux-deployment-5b568bf897-w98pf   0/1     Pending   0          38s
eks-sample-linux-deployment-5b568bf897-wmlw5   0/1     Pending   0          38s

```

to see the pod logs
```sh
$ kubectl logs  eks-sample-linux-deployment-5b568bf897-fkjl2 -n game-2048
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2024/02/07 18:13:21 [notice] 1#1: using the "epoll" event method
2024/02/07 18:13:21 [notice] 1#1: nginx/1.23.4
2024/02/07 18:13:21 [notice] 1#1: built by gcc 10.2.1 20210110 (Debian 10.2.1-6) 
2024/02/07 18:13:21 [notice] 1#1: OS: Linux 5.10.205-195.807.amzn2.x86_64
2024/02/07 18:13:21 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1024:65535
2024/02/07 18:13:21 [notice] 1#1: start worker processes
2024/02/07 18:13:21 [notice] 1#1: start worker process 29
2024/02/07 18:13:21 [notice] 1#1: start worker process 30

```
to get the service pod
```sh
$ kubectl get svc -n game-2048
NAME                       TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
eks-sample-linux-service   ClusterIP   10.100.36.169   <none>        80/TCP    7s

```
to ge the ingress pod. For this we dont have host ip as we are not created the ingress controller yet only ingress resouce created.
```sh
$ kubectl get ing -n game-2048
NAME           CLASS   HOSTS   ADDRESS   PORTS   AGE
ingress-2048   alb     *                 80      12m
```
Create OIDC iam for accessing the aws resouces.
```sh
eksctl utils associate-iam-oidc-provider --cluster $cluster_name --approve

```
OIDC configure to give access to aws resouces for the ingress controller

```sh
$cluster_name=demo-cluster
$oidc_id=$(aws eks describe-cluster --name demo-cluster --region us-east-1  --query "cluster.identity.oidc.issuer" --output text | cut -d '/' -f 5)
$eksctl utils associate-iam-oidc-provider --cluster demo-cluster --region us-east-1 --approve
2024-02-08 00:02:31 [ℹ]  will create IAM Open ID Connect provider for cluster "demo-cluster" in "us-east-1"
2024-02-08 00:02:32 [✔]  created IAM Open ID Connect provider for cluster "demo-cluster" in "us-east-1"
```
Get the IAM policy from the AWS docs
```
curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/install/iam_policy.json
```

Create IAM policy
```sh
aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam_policy.json
```

```sh
aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam_policy.json
{
    "Policy": {
        "PolicyName": "AWSLoadBalancerControllerIAMPolicy",
        "PolicyId": "ANPAXYSX5JZBRVDW7RV6L",
        "Arn": "arn:aws:iam::<account-id>:policy/AWSLoadBalancerControllerIAMPolicy",
        "Path": "/",
        "DefaultVersionId": "v1",
        "AttachmentCount": 0,
        "PermissionsBoundaryUsageCount": 0,
        "IsAttachable": true,
        "CreateDate": "2024-02-07T18:38:55+00:00",
        "UpdateDate": "2024-02-07T18:38:55+00:00"
    }
}
```
Attaching IAM policy to load-balancer-controller
```sh
$ eksctl create iamserviceaccount   --cluster=demo-cluster   --namespace=kube-system   --name=aws-load-balancer-controller  \
 --role-name AmazonEKSLoadBalancerControllerRole   \
--attach-policy-arn=arn:aws:iam::<account-id>:policy/AWSLoadBalancerControllerIAMPolicy \
 --region us-east-1  --approve


2024-02-08 00:16:31 [ℹ]  1 iamserviceaccount (kube-system/aws-load-balancer-controller) was included (based on the include/exclude rules)
2024-02-08 00:16:31 [!]  serviceaccounts that exist in Kubernetes will be excluded, use --override-existing-serviceaccounts to override
2024-02-08 00:16:31 [ℹ]  1 task: { 
    2 sequential sub-tasks: { 
        create IAM role for serviceaccount "kube-system/aws-load-balancer-controller",
        create serviceaccount "kube-system/aws-load-balancer-controller",
    } }2024-02-08 00:16:31 [ℹ]  building iamserviceaccount stack "eksctl-demo-cluster-addon-iamserviceaccount-kube-system-aws-load-balancer-controller"
2024-02-08 00:16:32 [ℹ]  deploying stack "eksctl-demo-cluster-addon-iamserviceaccount-kube-system-aws-load-balancer-controller"
2024-02-08 00:16:32 [ℹ]  waiting for CloudFormation stack "eksctl-demo-cluster-addon-iamserviceaccount-kube-system-aws-load-balancer-controller"
2024-02-08 00:17:03 [ℹ]  waiting for CloudFormation stack "eksctl-demo-cluster-addon-iamserviceaccount-kube-system-aws-load-balancer-controller"
2024-02-08 00:17:04 [ℹ]  created serviceaccount "kube-system/aws-load-balancer-controller"
```
add helm chart repo it will use it to run the controller
```sh
$ helm repo add eks https://aws.github.io/eks-charts &&  helm repo update eks

```
install loadbalancer
```sh
helm install aws-load-balancer-controller eks/aws-load-balancer-controller \            
  -n kube-system \
  --set clusterName=<your-cluster-name> \
  --set serviceAccount.create=false \
  --set serviceAccount.name=aws-load-balancer-controller \
  --set region=<region> \
  --set vpcId=<your-vpc-id>
```
output:
```sh
NAME: aws-load-balancer-controller
LAST DEPLOYED: Thu Feb  8 00:25:21 2024
NAMESPACE: kube-system
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
AWS Load Balancer controller installed!

```
```sh
ound in default namespace.
sunkara@sunkara-VivoBook:~/Documents/2048-app$ kubectl get pods -n game-2048
NAME                                           READY   STATUS    RESTARTS   AGE
eks-sample-linux-deployment-5b568bf897-fkjl2   1/1     Running   0          44m
eks-sample-linux-deployment-5b568bf897-w98pf   1/1     Running   0          44m
eks-sample-linux-deployment-5b568bf897-wmlw5   1/1     Running   0          44m
sunkara@sunkara-VivoBook:~/Documents/2048-app$ kubectl get ingress -n game-2048
NAME           CLASS   HOSTS   ADDRESS                                                                   PORTS   AGE
ingress-2048   alb     *       k8s-game2048-ingress2-bcac0b5b37-1175388174.us-east-1.elb.amazonaws.com   80      51m
sunkara@sunkara-VivoBook:~/Documents/2048-app$ kubectl get deployment -n kube-system aws-load-balancer-controller
NAME                           READY   UP-TO-DATE   AVAILABLE   AGE
aws-load-balancer-controller   2/2     2            2           3m10s
sunkara@sunkara-VivoBook:~/Documents/2048-app$ kubectl get deployment -n kube-system
NAME                           READY   UP-TO-DATE   AVAILABLE   AGE
aws-load-balancer-controller   2/2     2            2           3m34s
coredns                        2/2     2            2           92m
sunkara@sunkara-VivoBook:~/Documents/2048-app$ kubectl get pods -n kube-system
NAME                                            READY   STATUS    RESTARTS   AGE
aws-load-balancer-controller-5fdc5b8d6c-d4rb8   1/1     Running   0          3m44s
aws-load-balancer-controller-5fdc5b8d6c-qcjf6   1/1     Running   0          3m44s
coredns-5587ff98f-ds5hb                         1/1     Running   0          83m
coredns-5587ff98f-pgchw                         1/1     Running   0          83m
```
```sh
$cat full_deployment.yml 
---
apiVersion: v1
kind: Namespace
metadata:
  name: game-2048
---
apiVersion: apps/v1
kind: Deployment
metadata:
  namespace: game-2048
  name: eks-sample-linux-deployment
  labels:
    app: eks-sample-linux-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: eks-sample-linux-app
  template:
    metadata:
      labels:
        app: eks-sample-linux-app
    spec:
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: kubernetes.io/arch
                operator: In
                values:
                - amd64
                - arm64
      containers:
      - name: nginx
        image: public.ecr.aws/nginx/nginx:1.23
        ports:
        - name: http
          containerPort: 80
        imagePullPolicy: IfNotPresent
      nodeSelector:
        kubernetes.io/os: linux
---
apiVersion: v1
kind: Service
metadata:
  name: eks-sample-linux-service
  namespace: game-2048
  labels:
    app: eks-sample-linux-app
spec:
  selector:
    app: eks-sample-linux-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  namespace: game-2048
  name: ingress-2048
  annotations:
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/target-type: ip
spec:
  ingressClassName: alb
  rules:
    - http:
        paths:
        - path: /
          pathType: Prefix
          backend:
            service:
              name: service-2048
              port:
                number: 80
```


