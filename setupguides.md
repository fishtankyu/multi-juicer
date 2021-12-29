```sh
sudo snap install doctl
```
```sh
doctl auth init
```
```sh
mkdir .kube 
```
```sh
mkdir .config
```
```sh
sudo snap connect doctl:kube-config
```
```sh
doctl kubernetes cluster create juicy-k8s
```

