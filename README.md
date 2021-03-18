# Running your first helloworld

## Getting Started with Minikube on Mac

### Setup Prerequisities

#### Docker:
* Download links for Docker for Mac: https://www.docker.com/docker-mac

Command to test: `docker version`

#### Virtualbox

* Download link: https://www.virtualbox.org/

Command to test: `virtualbox`

### Setup Kubectl:
* Download link: https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-binary-via-curl

Command to test: `kubectl version`


### Setup Minikube:
* General Download Instructions: https://kubernetes.io/docs/tasks/tools/install-minikube/
* Download link: https://github.com/kubernetes/minikube/releases

Command to test: `minikube version`


### Docker Image:
* [karthequian/helloworld:latest](https://hub.docker.com/r/karthequian/helloworld/)


### Starting up minikube

First, get minikube up and running with the command `minikube start`. This command sets up a Kubernetes dev environment for you via VirtualBox.

The last statement in the output states that kubectl can talk to minikube. We can verify this by running the command `kubectl get nodes`

This will show you that minikube is ready to use.

### Set up your helloworld
Make sure `helloworld.yaml` is in your directory. Then, go to the directory and type:
```
kubectl create -f helloworld.yaml
```
This command creates a deployment resource from the file helloworld.yaml, which, in this case, contains a deployment called "hellworld", pulling from the image karthequian/helloworld, and exposes port 80 of the container to the pod.

Running this command will give you this output, stating that the deployment "hw" was created.

```
$ kubectl create -f helloworld.yaml 
deployment.apps/helloworld created
```

We can run the command `kubectl get all` to see all our resources running.

### Setting up a load balancer
You'll notice that in the `kubectl get all` command, the service has a port mapping defined; however, when you try to hit that port via your web browser, you won't be able to reach the service.

This is because by default, the pod is only accessible by its internal IP address within the cluster. To make the helloworld container accessible from outside the Kubernetes virtual network, you have to expose the pod as a Kubernetes service.

To do this, we can expose the pod to the public internet using the kubectl expose command 
`kubectl expose deployment helloworld --type=NodePort`

The `--type=NodePort` flag exposes the deployment outside of the cluster. If you're using this on a cloud provider, you can use a `--type=LoadBalancer` that will provision an external IP address would be provisioned to access the service.

To view the final user interface, use the minikube service command.

`minikube service helloworld`

This will open your web browser to your application that is running in Kubernetes!