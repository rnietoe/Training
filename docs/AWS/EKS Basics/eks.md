# EKS Basics

[Getting started with Amazon EKS – eksctl](https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html)

* eksctl: simple tool to create, delete, and get information about our cluster
* kubectl: the k8s command line utilizy. used to control Kubernetes clusters

EC2 vs Fargate

running across three AZs in a single region

https://kubernetes.io/docs/home/
https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects

```sh
# [Installing or updating the latest version of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
which aws
sudo ./aws/install --bin-dir /usr/local/bin --install-dir /usr/local/aws-cli --update
aws --version
# configure your admin user using its access key
aws configure 
# Installing or updating kubectl
curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.24.9/2023-01-11/bin/linux/amd64/kubectl
chmod +x ./kubectl # Apply execute permissions to the binary.
mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$PATH:$HOME/bin
# Installing or updating eksctl
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin # sudo mv /tmp/eksctl /usr/local/bin
eksctl version

# Create the Amazon EKS cluster and nodes will create 2 CF stacks: cluster and managed nodes
eksctl create cluster --name my-cluster --region eu-west-1 --nodegroup-name standard-workers --node-type t3.medium --nodes 3 --nodes-min 1 --nodes-max 4 --managed
eksctl get cluster
# Create or update a kubeconfig file for your cluster to configure your computer to communicate with your cluster
aws eks update-kubeconfig --region eu-west-1 --name my-cluster
kubectl get svc
```

first the LB service:

```yml
apiVersion: v1
kind: Service
metadata:
  name: nginx-svc
  labels:
    env: dev
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    env: dev
```

and later the backend workloads:

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    env: dev
spec:
  replicas: 3
  selector:
    matchLabels:
      env: dev
  template:
    metadata:
      labels:
        env: dev
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
```

```sh
kubectl apply -f ./service.yaml
kubectl get service
kubectl apply -f ./deployment.yaml
kubectl get deployment
kubectl get pod
kubectl get rs
kubectl get node
```