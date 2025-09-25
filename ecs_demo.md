### This is an ECS Demo based on 
https://github.com/solo-io/ecs-demo

This guide was tested with tool installed locally on an Ubuntu 24.04 LTS. 

1. Install aws cli

```
  sudo apt update
  sudo apt install -y unzip curl
  curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
  unzip awscliv2.zip 
  cd aws
  sudo ./install 
  aws version
  aws configure sso
```

2. Configure AWS KEYS

You will need AWS Key in your session to be able to run eksctl. Stick them in your ~/.bashrc
   
4. Install eksctl

Check for the lates version on [https://github.com/eksctl-io/eksctl/releases](
https://github.com/eksctl-io/eksctl/releases)
```
curl -LO https://github.com/eksctl-io/eksctl/releases/download/v0.214.0/eksctl_Linux_amd64.tar.gz
tar -xvzf eksctl_Linux_amd64.tar.gz 
sudo mv eksctl /usr/local/bin/
eksctl version
```

3. Create the Configuration for EKS

```
cat >eks_config.sh <<EOF
export AWS_REGION=eu-central-1        # The AWS region where the cluster will be deployed
export OWNER_NAME=$(whoami)        # The name of the cluster owner (auto-fills with your username)
export EKS_VERSION=1.33            # Version of EKS to be used for the cluster
export CLUSTER_NAME=ambientdemo    # Name of the EKS cluster. The ECS cluster will be ecs-$CLUSTER_NAME
export NUMBER_NODES=2              # The number of nodes in your EKS cluster
export NODE_TYPE="t2.medium"       # The instance type for the nodes in the EKS cluster
EOF
```

4. Load Config and Create eks cluster

Initial EKS cluster setup takes ~15min
```
. eks_config.sh
eval "echo \"$(cat manifests/eks-cluster.yaml)\"" | eksctl create cluster --config-file -
```

We need to update the vpc-cni after initial setup
```
eksctl update addon --cluster ambientdemo --name vpc-cni
```

   
5. Deploy the Kubernetes Gateway API CRD

Sep-2025
```
kubectl apply -f https://github.com/kubernetes-sigs/gateway-api/releases/download/v1.3.0/standard-install.yaml
```
   
7. Install istioctl from solo

Got to the [Solo Support Page for installing the solo version of istio](https://support.solo.io/hc/en-us/articles/4414409064596-Istio-images-built-by-Solo-io)
Follow the instructions to install istioctl.

8. 
9. 


