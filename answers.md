# DevOps Answers 
#### This file contains all the necessary information’s about my decisions regarding to 5 questions that is in [instructions.md](instructions.md) file.
#### Answers by Mohammad Taghadosi ( Whole projects uploaded in [my GitHub](https://github.com/mtaghadosi/DevOps-Tutorial) )
#### This is not a complete solution, I tried to visualize the hole picture.

# Answers: (Every number refers to task numbers)
1.  For the first task, several conditions can be considered, all of which depend on many factors, such as product management strategies, the level of security required, the number of programmers involved and the number of rollouts in a period of time, the range of customers, and the product update and rollback strategy and even the considered budget for the entire project. I will try to present my ideal solution first without considering the factors I mentioned above and at the end I will mention other solutions as well. What is clear is that we need a web server! And according to the given instructions, we should use nginX. As a DevOps, I suggest setting up this web server on the cloud. This cloud can be ECS on AWS or GKE on GCP or maybe we have an excellent data center in our company that we have already prepared with the best [data-center-design-standards](https://github.com/mtaghadosi/Data-Center-Design-Standards). If so, we welcome to use the server-farm of that data center as the force of our on-premises cloud. Suppose we use our own local data center.
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
 - [Create a Per-Volume claim](/YAML/create-PVC.yaml)

We also have a namespace for our nginX. Our persistence volume claim and all of our pods are in that namespace, let's assume that the name is `nginx-exposed`. We know that the PVs don't have namespaces right? yeah! So let's Apply them using the commands bellow.
