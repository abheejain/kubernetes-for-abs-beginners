# kubernetes-for-abs-beginners

## Overview

## Setup Kubernetes
Local

- MiniKube
- Microk8s
- kubeadm

Cloud
- AWS
- GCP
- MS Azure

### Minikube

REF: https://kubernetes.io/docs/tasks/tools/
 

hyperwisor
kubectl
minikube
    download ISO
-- 
```
sysctl -a | grep -E --color 'machdep.cpu.features|VMX'

machdep.cpu.features: FPU VME DE PSE TSC MSR PAE MCE CX8 APIC SEP MTRR PGE MCA CMOV PAT PSE36 CLFSH DS ACPI MMX FXSR SSE SSE2 SS HTT TM PBE SSE3 PCLMULQDQ DTES64 MON DSCPL VMX SMX EST TM2 SSSE3 FMA CX16 TPR PDCM SSE4.1 SSE4.2 x2APIC MOVBE POPCNT AES PCID XSAVE OSXSAVE SEGLIM64 TSCTMR AVX1.0 RDRAND F16C
```
if you see `VMX` in the output (should be COLORED), then the VT-x feature is enabled in your machine

2. install a hypervisor
Option 1: with Docker
Using Minikube with Docker as Hypervisor in stead of Virtual Box 

https://minikube.sigs.k8s.io/docs/start/

Make sure you have Docler running 

Option 2: With Virtual Box


```
minikube status
```
Output should look like this
```
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```


TEST the Minikube environment with a sample web application 
REF: https://kubernetes.io/docs/tutorials/hello-minikube/

```
minikube dashboard
```
This will llaunch the dashboard in local browser 
http://127.0.0.1:50283/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/#/workloads?namespace=default


### Create Deployment
1
2
3
4
5
6
kubectl config view
```
apiVersion: v1
clusters:
- cluster:
    certificate-authority: /Users/abheejain/.minikube/ca.crt
    extensions:
    - extension:
        last-update: Mon, 24 Jan 2022 18:37:13 +07
        provider: minikube.sigs.k8s.io
        version: v1.25.1
      name: cluster_info
    server: https://127.0.0.1:49841
  name: minikube
contexts:
- context:
    cluster: minikube
    extensions:
    - extension:
        last-update: Mon, 24 Jan 2022 18:37:13 +07
        provider: minikube.sigs.k8s.io
        version: v1.25.1
      name: context_info
    namespace: default
    user: minikube
  name: minikube
current-context: minikube
kind: Config
preferences: {}
users:
- name: minikube
  user:
    client-certificate: /Users/abheejain/.minikube/profiles/minikube/client.crt
    client-key: /Users/abheejain/.minikube/profiles/minikube/client.key
```

Note: For more information about kubectl commands, see the kubectl overview. [https://kubernetes.io/docs/reference/kubectl/overview/]


### Create a Service

kubectl expose deployment hello-node --type=LoadBalancer --port=8080
o/p
service/hello-node exposed


```
abheejain@SystemAdmins-Mac-mini kubernetes-for-abs-beginners % minikube service hello-node
|-----------|------------|-------------|---------------------------|
| NAMESPACE |    NAME    | TARGET PORT |            URL            |
|-----------|------------|-------------|---------------------------|
| default   | hello-node |        8080 | http://192.168.49.2:30196 |
|-----------|------------|-------------|---------------------------|
🏃  Starting tunnel for service hello-node.
|-----------|------------|-------------|------------------------|
| NAMESPACE |    NAME    | TARGET PORT |          URL           |
|-----------|------------|-------------|------------------------|
| default   | hello-node |             | http://127.0.0.1:50500 |
|-----------|------------|-------------|------------------------|
🎉  Opening service default/hello-node in default browser...
❗  Because you are using a Docker driver on darwin, the terminal needs to be open to run it.

```

It will be launching ther browswe as mentioned at `http://127.0.0.1:50500/`

and output in the browser with the applicaiton as

```
CLIENT VALUES:
client_address=172.17.0.1
command=GET
real path=/
query=nil
request_version=1.1
request_uri=http://127.0.0.1:8080/

SERVER VALUES:
server_version=nginx: 1.10.0 - lua: 10001

HEADERS RECEIVED:
accept=text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
accept-encoding=gzip, deflate, br
accept-language=en-US,en;q=0.9
connection=keep-alive
host=127.0.0.1:50500
sec-ch-ua=" Not;A Brand";v="99", "Google Chrome";v="97", "Chromium";v="97"
sec-ch-ua-mobile=?0
sec-ch-ua-platform="macOS"
sec-fetch-dest=document
sec-fetch-mode=navigate
sec-fetch-site=none
sec-fetch-user=?1
upgrade-insecure-requests=1
user-agent=Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/97.0.4692.99 Safari/537.36
BODY:
-no body in request-
```


### Enable addons 

abheejain@SystemAdmins-Mac-mini ~ % minikube addons list
|-----------------------------|----------|--------------|--------------------------------|
|         ADDON NAME          | PROFILE  |    STATUS    |           MAINTAINER           |
|-----------------------------|----------|--------------|--------------------------------|
| ambassador                  | minikube | disabled     | third-party (ambassador)       |
| auto-pause                  | minikube | disabled     | google                         |
| csi-hostpath-driver         | minikube | disabled     | kubernetes                     |
| dashboard                   | minikube | enabled ✅   | kubernetes                     |
| default-storageclass        | minikube | enabled ✅   | kubernetes                     |
| efk                         | minikube | disabled     | third-party (elastic)          |
| freshpod                    | minikube | disabled     | google                         |
| gcp-auth                    | minikube | disabled     | google                         |
| gvisor                      | minikube | disabled     | google                         |
| helm-tiller                 | minikube | disabled     | third-party (helm)             |
| ingress                     | minikube | disabled     | unknown (third-party)          |
| ingress-dns                 | minikube | disabled     | google                         |
| istio                       | minikube | disabled     | third-party (istio)            |
| istio-provisioner           | minikube | disabled     | third-party (istio)            |
| kubevirt                    | minikube | disabled     | third-party (kubevirt)         |
| logviewer                   | minikube | disabled     | unknown (third-party)          |
| metallb                     | minikube | disabled     | third-party (metallb)          |
| metrics-server              | minikube | disabled     | kubernetes                     |
| nvidia-driver-installer     | minikube | disabled     | google                         |
| nvidia-gpu-device-plugin    | minikube | disabled     | third-party (nvidia)           |
| olm                         | minikube | disabled     | third-party (operator          |
|                             |          |              | framework)                     |
| pod-security-policy         | minikube | disabled     | unknown (third-party)          |
| portainer                   | minikube | disabled     | portainer.io                   |
| registry                    | minikube | disabled     | google                         |
| registry-aliases            | minikube | disabled     | unknown (third-party)          |
| registry-creds              | minikube | disabled     | third-party (upmc enterprises) |
| storage-provisioner         | minikube | enabled ✅   | google                         |
| storage-provisioner-gluster | minikube | disabled     | unknown (third-party)          |
| volumesnapshots             | minikube | disabled     | kubernetes                     |
|-----------------------------|----------|--------------|--------------------------------|



Rollback the installed service and deployment

kubectl delete service hello-node
kubectl delete deployment hello-node

```
abheejain@SystemAdmins-Mac-mini ~ % kubectl delete service hello-node
service "hello-node" deleted
abheejain@SystemAdmins-Mac-mini ~ % delete deployment hello-node
zsh: command not found: delete
abheejain@SystemAdmins-Mac-mini ~ % kubectl delete deployment hello-node
deployment.apps "hello-node" deleted
abheejain@SystemAdmins-Mac-mini ~ % minikube stop
✋  Stopping node "minikube"  ...
🛑  Powering off "minikube" via SSH ...
🛑  1 node stopped.
abheejain@SystemAdmins-Mac-mini ~ %

```

And the browser window opened with `minikube dashboard` willupdate based onteh above commands. 
lastly, when `node stopped`

`dial tcp 127.0.0.1:49841: connect: connection refused`  

This is because the `minicube stop`ed



## YAML Introduciton