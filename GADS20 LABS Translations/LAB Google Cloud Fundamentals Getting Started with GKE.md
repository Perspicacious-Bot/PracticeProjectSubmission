LAB: Google Cloud Fundamentals: Getting Started with GKE.
DURATION: 35 minutes.
OBJECTIVES: In this lab, you learn how to perform the following tasks:

- Provision a Kubernetes cluster using [Kubernetes Engine.](https://cloud.google.com/container-engine)
- Deploy and manage Docker containers using `kubectl`.

Task 1: 

​       Confirm that needed APIs are enabled
​    
​       Kubernetes Engine API

​       Container Registry API

```
  gcloud services list --available
```

Task 2: 

 Start a Kubernetes Engine cluster

Steps:

1. For convenience, place the zone that Qwiklabs assigned you to into an environment variable called MY_ZONE.  Type this partial command:

```
 export MY_ZONE=us-central1-a
```



2. Start a Kubernetes cluster managed by Kubernetes Engine. Name the cluster *webfrontend* and configure it to run 2 nodes:

```
 gcloud container clusters create webfrontend --zone $MY_ZONE --num-nodes 2
```



3. Check your installed version of Kubernetes using the kubectl version command:

```
  kubectl version
```

The `gcloud container clusters create` command automatically authenticated `kubectl` for you.

4. View your running nodes 

```
gcloud container node-pools list
```



Task 3: 
Run and deploy a container

Steps:

1. From your Cloud Shell prompt, launch a single instance of the nginx container.

```
kubectl create deploy nginx --image=nginx:1.17.10
```

*In Kubernetes, all containers run in pods. This use of the `kubectl create` command caused Kubernetes to create a deployment consisting of a single pod containing the nginx container. A Kubernetes deployment keeps a given number of pods up and running even in the event of failures among the nodes on which they run. In this command, we launched the default number of pods, which is 1.*

2. To view the pod running the nginx container, enter the command:

```
kubectl get pods
```

3. To expose the nginx container to the Internet, run:

```
kubectl expose deployment nginx --port 80 --type LoadBalancer
```

*Kubernetes created a service and an external load balancer with a public IP address attached to it. The IP address remains the same for the life of the service. Any network traffic to that public IP address is routed to pods behind the service: in this case, the nginx pod.*

4. View the new service:

```
kubectl get services
```

*You can use the displayed external IP address to test and contact the nginx container remotely.*

*It may take a few seconds before the **External-IP** field is populated for your service. This is normal. Just re-run the `kubectl get services` command every few seconds until the field is populated.*

5. We scale up the number of pods running on our service by running:

```
kubectl scale deployment nginx --replicas 3
```

*Scaling up a deployment is useful when you want to increase available resources for an application that is becoming more popular.*

6. Confirm that Kubernetes has updated the number of pods:

```
kubectl get pods
```

//7. Confirm that your external IP address has not changed:

```
kubectl get services
```

*Return to the web browser tab in which you viewed your cluster's external IP address. Refresh the page to confirm that the nginx web server is still responding.*