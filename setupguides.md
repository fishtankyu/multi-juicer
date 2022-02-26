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

</br>

Install kubectl

```sh
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

Test to ensure the version you installed is up-to-date

```sh
kubectl version --client
```

### Install helm

```sh
curl https://baltocdn.com/helm/signing.asc | sudo apt-key add -
```
```sh
sudo apt-get install apt-transport-https --yes
```
```sh
echo "deb https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
```
```sh
sudo apt-get update
```
```sh
sudo apt-get install helm
```

### Repository add multi-juicer

```sh
helm repo add multi-juicer https://iteratec.github.io/multi-juicer/
```

### Create a configuration file for juiceshop 

```sh
nano values.yaml
```
</br>
This is the configutation file that if you want to allow the user to see the hints,flags,githublink,versionnumber etc

```sh
juiceShop:

  config: |

    application:

      logo: JuiceShopCTF_Logo.png

      favicon: favicon_ctf.ico

      showVersionNumber: false

      showGitHubLinks: false

    challenges:

      showHints: false

    hackingInstructor:

      isEnabled: false

    ctf:

      showFlagsInNotifications: true
```

### Final Steps
#### Step 1: Starting with cluster
First we'll need a cluster, this will take a couple of minutes

```sh 
doctl kubernetes cluster create juicy-k8s
```
After completion verify that your kubectl content has been updated:
</br>
Should print something like: do-nyc1-juicy-k8s

```sh
kubectl config current-context
```

#### Step 2: Installing Multijuicer via helm
You'll need to add the multi-juicer help repo to you helm repos

```sh
helm repo add multi-juicer https://iteratec.github.io/multi-juicer/
```
```sh
helm install multi-juicer multi-juicer/multi-juicer
```
kubernets will now spin up the pods, to verify everthing is starting up, run:
```sh
kubectl get pods
```
This should show you two pods a juice-balancer pod and a progress-watchdog pod

#### Step 3: Expose to the world using loadbalancer

```sh 
helm install -f values.yaml multi-juicer multi-juicer/multi-juicer --set "balancer.service.type=LoadBalancer"
```
By running this to get the admin password for the muti-juicer
```sh
kubectl get secrets juice-balancer-secret -o=jsonpath='{.data.adminPassword}' | base64 --decode
```
