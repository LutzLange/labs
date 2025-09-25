### This is an ECS Demo based on 
https://github.com/solo-io/ecs-demo

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

2. Install eksctl

Check for the lates version on [https://github.com/eksctl-io/eksctl/releases](
https://github.com/eksctl-io/eksctl/releases)
```
curl -LO https://github.com/eksctl-io/eksctl/releases/download/v0.214.0/eksctl_Linux_amd64.tar.gz
tar -xvzf eksctl_Linux_amd64.tar.gz 
sudo mv eksctl /usr/local/bin/
eksctl version
```

3. 
