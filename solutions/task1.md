# Task1 solution
#### This file contains all the necessary information’s about my decisions regarding to task1 that is in [instructions.md](instructions.md) file.
#### Answers by ["Mohammad Taghadosi"](https://mtaghadosi.ir)

# Provided solution for task 1:
For the first task, several conditions can be considered, all of which depend on many factors, such as product management strategies, the level of security required, the number of programmers involved and the number of rollouts in a period of time, the range of customers, and the product update and rollback strategy and even the considered budget for the entire project. I will try to present my ideal solution first without considering the factors I mentioned above and at the end I will mention other solutions as well. What is clear is that we need a web server! And according to the given instructions, we should use nginX. As a DevOps, I suggest setting up this web server on the cloud. This cloud can be ECS on AWS or GKE on GCP or maybe we have an excellent data center in our company that we have already prepared with the best [data-center-design-standards](https://github.com/mtaghadosi/Data-Center-Design-Standards). If so, we welcome to use the server-farm of that data center as the force of our on-premises cloud. Suppose we use our own local data center.
First we need to start the cloud, I want to use Kubernetes (It's good to mention I can use Openshift with GitOps and Argo CD for IaC -Infrastraucture as Code- too). To start the cloud we need at least two Master nodes and at least several Worker nodes. So let's set them up (I'll try to be very brief):
 - Installing operating systems (I prefer to work with Ubuntu 22)
 - Setting up internal network with DNS.
 - Verify if the servers can communicate by ping (either by DNS name or IP)
 - Several technical tasks such as configuring firewalls and swapoff and many other tasks.
 - In order to not prolong this step, I have already put all the necessary information for setting up an on-premisses cloud at [this address](https://github.com/mtaghadosi/kubernetes-scripts/blob/main/install-kubernetes-on-ubuntu-22.sh), please check for more information (please note: that is not a shell-script! ;) ).
Now that we have the cloud up and running, we can use the command like this to investigate that:
```
kubectl get node --all-namespaces
```
Now, first and before even install the nginx we need to figure out how to store or static website and all relative data, in other language we need to host our static JavaScript files (that written in TypeScript and React). So, we need a persistence storage. For that storage we can use Amazon S3 or NFS or other solutions for storage backend and then combine all those solutions into some YAML files and then deploy them. This will let us have a persistence volume (called PV). Let assume that we decide to use a secure NFS server. The YAML files will look like something like bellow, we first bind the storage, then we claim the storage with PVC and finally we assign it to the pods that used for our nginx.
 - [Create a Per-Volume](/YAML/create-PV-nfs.yaml)
 - [Create a Per-Volume claim](/YAML/create-PVC.yaml).

We also have a namespace for our nginX. Our persistence volume claim and all of our pods are in that namespace, let's assume that the name is `nginx-exposed`. We know that the PVs don't have namespaces right? yeah! So let's Apply them using the commands bellow:

```
kubectl apply -f create-PV-nfs.yaml
kubectl apply -f create-PVC.yaml
```

Let’s copy the static and all relevant files into that storage. Now we have to make a deployment which auto scales horizontally, so I wrote a YAML file that creates a deployment. Also, our deployment must connect to the storage that we made earlier. The YAML file will be something like bellow: 
- [Create a NginX Deployment](/YAML/dep-nginx-exposed-with-nfs-pvc.yaml)
Now delploy and autoscale:
```shell
kubectl apply -f dep-nginx-exposed-with-nfs-pvc.yaml
kubectl autoscale deployment nginx-external-deployment -n nginx-exposed --cpu-percent=70 --min=2 --max=100
kubectl get hpa --all-namespaces
```
Let’s create some services to expose our nginx (I want to keep it so simple in real work we can use ingress and have DNS and https but I just use IP address). I created a YAML file for LoadBalancer service that is a good fit for autoscaling feature that we implemented above, it distributes all traffics to all the nodes using round-robin algorithm.
- [Create a LoadBalancer Service for nginX Deployment](/YAML/creat-loadbalancer-nginx.yaml)

NOW! If we check the port 30001 from outside network, we should see the static website. It can auto scale to maintain itself with the input load (making more PODs when it is necessary and deleting additional PODs when it is not necessary).

# CI/CD automations stage:

As long as we use two different git repositories for our application and we need to continuously update and push the code to git and deploy and integrate our product, we need some automation methods. I use Jenkins for this solution. With it we can pull the code (even the pull can be automated using web-hooks) and after every commit we can test and build and release (our artifact) and deploy our product. I wrote a Jenkins file and tried to be as realistic as possible but in real industrial level definitely it should be more sophisticated. I use decelerative scripted method for my pipeline. I will describe that in more details in next steps. 

- [Jenkins file for CI/CD automation.](/Jenkinsfile)

![Jenkinsfile-demo-1](/assets/jenkins-demo-1.jpg)

### Answers:
- [Task-1](/solutions/task1.md)
- [Task-2](/solutions/task2.md)
- [Task-3](/solutions/task3.md)
- [Task-4](/solutions/task4.md)
- [Task-5](/solutions/task5.md)

