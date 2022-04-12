# Create local light weight k3d environment

**Prerequisite:** installed Docker on local machine or virtual machine

## Install kubectl

Download the latest release with curl command:

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```

Install kubectl

```bash
sudo install -o root -g root -m 07555 kubectl /usr/local/bin/kubectl
```

Get version of installed kubectl

```bash
kubectl version --short
```

Set autocompletion for kubectl

```bash
echo 'source <(kubectl completion bash)' >>~/.bashrc
```

This settings will be actived if we run ~/.bashrc or do logout / login process for terminal.

Check the autocomplete

```bash
kubectl [TAB]
```

## Install base k3d

Download the latest realese of k3d with curl

```bash
curl -s https://raw.githubusercontent.com/rancher/k3d/main/install.sh | bash
```

Check the installed k3d version

```bash
k3d version
```

Check and inspect the dev-cluster-config YAMLs

```bash
vim dev-cluster-config.yaml
```

Create new dev-cluster with previous config file

```bash
k3d cluster create dev-cluster --config dev-cluster-config.yaml
```

SAMPLE OUTPUT

```bash
INFO[0000] Using config file cluster-config.yaml (k3d.io/v1alpha2#simple) 
WARN[0000] Default config apiVersion is 'k3d.io/v1alpha4', but you're using 'k3d.io/v1alpha2': consider migrating. 
INFO[0000] Prep: Network                                
INFO[0000] Re-using existing network 'k3d-dev-cluster' (49f813999daaafb443d993541929e39e191d49fa1aa62bad40ffdfcad4128a78) 
INFO[0000] Created image volume k3d-dev-cluster-images  
INFO[0000] Starting new tools node...                   
INFO[0000] Creating initializing server node            
INFO[0000] Creating node 'k3d-dev-cluster-server-0'     
INFO[0002] Pulling image 'docker.io/rancher/k3d-tools:5.3.0' 
INFO[0002] Pulling image 'docker.io/rancher/k3s:v1.22.6-k3s1' 
INFO[0004] Starting Node 'k3d-dev-cluster-tools'        
INFO[0007] Creating node 'k3d-dev-cluster-server-1'     
INFO[0009] Creating node 'k3d-dev-cluster-server-2'     
INFO[0009] Creating node 'k3d-dev-cluster-agent-0'      
INFO[0009] Creating node 'k3d-dev-cluster-agent-1'      
INFO[0009] Creating node 'k3d-dev-cluster-agent-2'      
INFO[0009] Creating LoadBalancer 'k3d-dev-cluster-serverlb' 
INFO[0011] Pulling image 'docker.io/rancher/k3d-proxy:5.3.0' 
INFO[0014] Using the k3d-tools node to gather environment information 
INFO[0014] HostIP: using network gateway 172.18.0.1 address 
INFO[0014] Starting cluster 'dev-cluster'               
INFO[0014] Starting the initializing server...          
INFO[0014] Starting Node 'k3d-dev-cluster-server-0'     
INFO[0016] Starting servers...                          
INFO[0016] Starting Node 'k3d-dev-cluster-server-1'     
INFO[0039] Starting Node 'k3d-dev-cluster-server-2'     
INFO[0052] Starting agents...                           
INFO[0052] Starting Node 'k3d-dev-cluster-agent-2'      
INFO[0052] Starting Node 'k3d-dev-cluster-agent-1'      
INFO[0052] Starting Node 'k3d-dev-cluster-agent-0'      
INFO[0060] Starting helpers...                          
INFO[0060] Starting Node 'k3d-dev-cluster-serverlb'     
INFO[0067] Injecting records for hostAliases (incl. host.k3d.internal) and for 7 network members into CoreDNS configmap... 
INFO[0070] Cluster 'dev-cluster' created successfully!  
INFO[0070] You can now use it like this:                
kubectl cluster-info

```

Get the created Kubernetes cluster info

```bash
kubectl cluster-info
```

Get the created nodes

```bash
kubectl get nodes -o wide
```

SAMPLE OUTPUT

```bash
NAME                       STATUS   ROLES                       AGE   VERSION
k3d-dev-cluster-agent-0    Ready    <none>                      26s   v1.22.6+k3s1
k3d-dev-cluster-agent-1    Ready    <none>                      26s   v1.22.6+k3s1
k3d-dev-cluster-agent-2    Ready    <none>                      26s   v1.22.6+k3s1
k3d-dev-cluster-server-0   Ready    control-plane,etcd,master   59s   v1.22.6+k3s1
k3d-dev-cluster-server-1   Ready    control-plane,etcd,master   42s   v1.22.6+k3s1
k3d-dev-cluster-server-2   Ready    control-plane,etcd,master   30s   v1.22.6+k3s1
```

We also can check the downloaded images that are used for k3d

```bash
docker images
```

And of course you can check the existing containers in Docker because the k3d uses Docker in Docker solution

```bash
CONTAINER ID   IMAGE                      COMMAND                  CREATED              STATUS              PORTS                                                              NAMES
a2d2d3bbe3fc   rancher/k3d-proxy:5.3.0    "/bin/sh -c nginx-pr…"   About a minute ago   Up 52 seconds       0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp, 0.0.0.0:6443->6443/tcp   k3d-dev-serverlb
c251878d8c57   rancher/k3s:v1.22.6-k3s1   "/bin/k3s agent"         About a minute ago   Up 59 seconds                                                                          k3d-dev-agent-1
e44a354d6dc8   rancher/k3s:v1.22.6-k3s1   "/bin/k3s agent"         About a minute ago   Up About a minute                                                                      k3d-dev-agent-0
346c2d6f3939   rancher/k3s:v1.22.6-k3s1   "/bin/k3s server --t…"   About a minute ago   Up About a minute                                                                      k3d-dev-server-1
e7ae22502b94   rancher/k3s:v1.22.6-k3s1   "/bin/k3s server --c…"   About a minute ago   Up About a minute                                                                      k3d-dev-server-0
```

Also check the newly created or updated Kubernetes config file

```bash
cat ~/.kube/config
```

## Remove k3d cluster

If you don not need the cluster further than you can delete it easily

Check the existing cluster(s)

```bash
k3d cluster list
```

SAMPLE OUTPUT

```bash
NAME          SERVERS   AGENTS   LOADBALANCER
dev-cluster   3/3       3/3      true
```

Delete the existing cluster

```bash
k3d cluster delete dev-cluster
```

SAMPLE OUTPUT

```bash
INFO[0000] Deleting cluster 'dev-cluster'               
INFO[0002] Deleting 2 attached volumes...               
INFO[0002] Removing cluster details from default kubeconfig... 
INFO[0002] Removing standalone kubeconfig file (if there is one)... 
INFO[0002] Successfully deleted cluster dev-cluster! 
```

[Full command list for k3d](https://k3d.io/v5.2.0/usage/commands/)
