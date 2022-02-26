# Set up guides on Digital Ocean

### Install doctl
```sh
sudo snap install doctl
```
```sh
doctl auth init
```

### Make this two directory
```sh
mkdir .kube 
```
```sh
mkdir .config
```

### Making connection
```sh
sudo snap connect doctl:kube-config
```

### Install Kubectl binary with curl on Linux
Download the lated release. </br>
Command:
```sh
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```
</br>
Validate the binary (optional) 
</br>
Download the kubectl checksum file:
```sh
curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
```
</br>
</br>

Validate the kubectl binary against the checksum file.
```sh
echo "$(<kubectl.sha256) kubectl" | sha256sum --check
```

If valid, the output is:
</br>
```kubectl:OK```



