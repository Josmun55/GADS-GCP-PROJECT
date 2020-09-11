# Google Cloud Fundamentals: Getting Started with GKE

> ## Objectives:
## In this lab, you learn how to perform the following tasks:
- Provision a Kubernetes cluster using Kubernetes Engine.
- Deploy and manage Docker containers using kubectl.

> ## Steps:

### 1. Confirm that needed APIs are enabled
`gcloud services list --available`

    Scroll down in the list of enabled APIs, and confirm that the below APIs are enabled:
        - Kubernetes Engine API
        - Container Registry API

-  - ### If either API is missing,Enable them using the following command:
- -    `gcloud services enable cloudapis.googleapis.com`
        


### 2. Start a Kubernetes Engine cluster

- For convenience, place the zone that Qwiklabs assigned you to into an environment variable called MY_ZONE followed by the zone that Qwiklabs assigned to you:

    `export MY_ZONE=us-central1-a`

- Start a Kubernetes cluster managed by Kubernetes Engine. Name the cluster webfrontend and configure it to run 2 nodes:

    `gcloud container clusters create webfrontend --zone $MY_ZONE --num-nodes 2`

- After the cluster is created, check your installed version of Kubernetes using the kubectl version command:

    `kubectl version`
- View your running Kubernetes Instance nodes:

    `gcloud compute instances list`

### 3. Run and Deploy a Container

- Launch a single instance of the nginx container. (Nginx is a popular web server.)

    `kubectl create deploy nginx --image=nginx:1.17.10`

        In Kubernetes, all containers run in pods. This use of the kubectl create command caused Kubernetes to create a deployment consisting of a single pod containing the nginx container. A Kubernetes deployment keeps a given number of pods up and running even in the event of failures among the nodes on which they run. In this command, you launched the default number of pods, which is 1.

     ___
        NOTE: If you see any deprecation warning about future version you can simply ignore it for now and can proceed further.
     ___
- View the pod running the nginx container:

    `kubectl get pods`

- Expose the nginx container to the Internet:

    `kubectl expose deployment nginx --port 80 --type LoadBalancer`

- View the new service:

    `kubectl get services`

        It may take a few seconds before the External-IP field is populated for your service. This is normal. Just re-run the kubectl get services command every few seconds until the field is populated.

- Open a new web browser tab and paste your cluster's external IP address into the address bar:

    > Result

        The default home page of the Nginx browser is displayed.

- To Scale up the number of pods running on your service:

    `kubectl scale deployment nginx --replicas 3`

- Confirm that Kubernetes has updated the number of pods:
    
    `kubectl get pods`

- Confirm that your external IP address has not changed:
    
    `kubectl get services`

- Return to the web browser tab in which you viewed your cluster's external IP address. Refresh the page to confirm that the nginx web server is still responding.
    >Result

         The default home page of the Nginx browser will still be displayed.



