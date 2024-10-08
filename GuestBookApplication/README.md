# Horizontal Pod Scaling of Kubernetes Guestbook Application with Rolling Updates and Roll back.

## Description,
   * In this project, we will build and deploy a simple Guestbook application. the application consists of a web front end which will have a text input where you can enter any text and submit.
   * For all of these we will create Kubernetes Deployments and Pods.Then we will apply Horizontal pod Scaling to the Guestbook application and finally work on Rolling updates and Rollbacks

## Objective,
  * Build and Deploy simple Guestbook application
  * Autoscale the Guestbook application using Horizontal pod Autoscaler
  * Performing Rolling Updates and Rollback

## Pre-requisite,
- Multi Stage Docker build
- NameSpace


## Environment Setup
  * clone the repo
  ``` bash
    [ ! -d K8S-Deployment-Strategies ] && git clone https://github.com/luchichang/K8S-Deployment-Strategies.git
  ```
  * Navigate to the Project Directory
  ```bash
    cd K8S-Deployment-Strategies/GuestBookApplication
  ```
  * view the Project Structure
  ```bash
    ls
  ```
## Steps
- switching to the v1 of the guestbook app
```bash
    cd 
```
- Exporting the NameSpace
```bash
    export My_NAMESPACE=<namespace name>
```
- Building the image from __docker file__
```bash
    docker build . -t us.icr.io/$MY_NAMESPACE/guestbook:v1
```
- push the image to the container registry
```bash
    docker push us.icr.io/$MY_NAMESPACE/guestbook:v1
```
- once the image pushed to the Image Container Registry(ICR). verify the image is build sucessfully
```bash
    ibmcloud cr images
```
(INFO:  to restrict output for the specific Name space
```bash
  ibmcloud cr images --restrict $MY_NAMESPACE
```
)

- Apply the Deployment using
```bash
    kubectl apply -f deployment.yml
``` 
- open a new terminal and enter the below command to view the application
```bash
    kubectl port-forward deployment.apps/guestbook 3000:3000
```

------------Auto Scaling the Deployment------------------
- now autoscale the Guestbook application deployment
```bash
   kubectl autoscale deployment guestbook --cpu-percent=5 --min=1 --max=10
```
- check the current status of the newly created Horizontal pod Auto scaler by running the command in terminal
```bash
    kubectl get hpa guestbook
```
- now increase the load to the pod 
```bash
  kubectl run -i --tty load-generator --rm --image=busybox:1.36.0 --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- <your app URL>; done"
```
- now watch the surge in the replicas
```bash
  kubectl get hpd guestbook --watch
```
-

--------------Rolling Update & Rollback-------------

## Docker Steps


## Deployment Steps

