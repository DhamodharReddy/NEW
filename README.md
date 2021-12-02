<!-- Headings -->
# **k3d - k3s in docker**

## **Setup a 3 Node cluster(1 Master & 2 Workers).**

### **Step-1 : Creating a Virtual Machine**
<!-- UL -->
* Login to your GCP  Account.

* Click on Compute Engine from Dropdown menu & click on VM Instances.
* Then click on CREATE INSTANCE
* In Details,
    * Give Name field as Instance1
* In Machine configuration,
    * Select series type & machine type as required
* In Boot disk,
    * Click on CHANGE
    * In PUBLIC IMAGES Tab,
        * Select Operating system as Ubuntu
        * Select Version as Ubuntu 18.04 LTS
        * Leave Boot disk & Size(GB) as default
        * Click on SELECT
* Leave remaining part as default and click CREATE
* You can see notification that the instance is getting created from top right notifications icon.
* After the instance is created you will get notified. Wait until the VM gets to active state, then click in SSH in Connect tab.
* A new window pops out which connects to the VM we just created.

### **Step-2 : Connect to Instance**
Now you are connected to Ubuntu VM,
Switch to root user or use sudo while executing commands.

### **Step-3 : Installing Docker**
<!-- Code Blocks -->
```
 apt-get update  #Update the repository first
 apt install docker.io #To install docker from packages
 docker --version #Check the version of docker
 ```

### **Step-4 : Installing kubectl with curl**
<!-- Code Blocks -->
```
 curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" #Download the latest release with this command
 sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl #Install kubectl
 kubectl version --client #Test to ensure the version you installed is up-to-date
 ```

### **Step-5 : Installing Rancher/k3d**
<!-- Code Blocks -->
```
 wget -q -O - https://raw.githubusercontent.com/rancher/k3d/main/install.sh | bash #install latest version using wget
  OR
 curl -s https://raw.githubusercontent.com/rancher/k3d/main/install.sh | bash #install latest version using curl
 ```

### **Step-6 : Create a Cluster**
<!-- Code Blocks -->
```
 k3d cluster create mycluster #Create a cluster named "mycluster" with just a single server node
 kubectl get nodes #Use the new cluster with kubectl, this commands shows the nodes created
 ```

As of now we have created the cluster with only one node(Control-plane/master)
But we need 3 Node cluster(1 Master & 2 Workers)

### **Step-7 : Create 3Nodes**
Getting the cluster’s kubeconfig (included in k3d cluster create)
<!-- Code Blocks -->
```
 k3d kubeconfig merge mycluster --kubeconfig-switch-context #Get the new cluster’s connection details merged into your default kubeconfig (usually specified using the KUBECONFIG environment variable or the default path $HOME/.kube/config) and directly switch to the new context
 ```

Creating Multi-Server Clusters
<!-- Code Blocks -->
```
 k3d cluster create multinode --agents 2 --servers 1 #Creating a multi node cluster is as easy as passing the number of servers (control plane nodes) and workers (agents) with the k3d cluster create command
 kubectl get nodes #Shows the nodes which we have created
 kubectl get nodes -owide #Shows the nodes which we have created (Post this output to slack).
 ```
 <!-- Horizontal Rule -->
---
 <!-- Links -->
You can refer [this page](https://k3d.io/v5.1.0/) for k3d.

<!-- Horizontal Rule -->
---
---