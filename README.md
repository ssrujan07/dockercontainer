# dockercontainer

**Docker – Container for NGINX**
Docker is an open-source platform that enables developers to easily develop, ship, and run applications. It packages an application along with its dependencies in an isolated virtual container which usually runs on a Linux system and is quite light as compared to a virtual machine. The reason being that a container shares the OS kernel with only its binaries/libraries loaded with it.

we will create and run a docker container for NGINX. After that, we will host a very simple and small website on the Nginx container.

**Docker Images **

A docker image is just like a snapshot in VM environments. It records information about the docker container at a specific time like all the libraries along with their particular versions needed for an application. It is immutable but can be easily duplicated and shared with others.
An image is usually shared with others in order to enable someone else to run the application in the same environment it is supposed to run and the image holds all the information about that environment.

**Docker Hub**
A Docker hub is one such platform where you can find and share container images with others. Some of the most common images are for Nginx, Nodejs, Mongo DB, and many more.

**NGiNX**

When a user requests a page from a web server, the web server takes the request and sends an appropriate response back to the user. Nginx can be that web server.

NGINX is an open-source web server that is also used for reverse proxy, HTTP load balancing, and email proxy. It is very efficient in using the system’s resources and can handle a huge number of simultaneous requests using event-driven and asynchronous architecture. That is the reason  why Nginx is an excellent choice for websites that deal with high load like e-commerce, cloud storage and search engines.

**Create a docker container for NGINX**
Download the official image for NGINX fromThis command will fetch the latest version of the nginx image but you can replace “latest” with the version you need and that will be downloaded. the docker hub using the following command.

**docker pull nginx:latest**

**This command will fetch the latest version of the nginx image but you can replace “latest” with the version you need and that will be downloaded.**

docker images

Now run the docker image using the following command.

**docker run -p 8000:80 nginx**

Using the above command you are running the nginx server where -p is the flag for mapping the port 8000 (local machine) to the port 80 in the container as 80 is the port on which nginx serves by default.

Verify the installation by visiting the localhost at the port 8000. We will get the default page for nginx web server as follows:

**Host a simple website on NGINX running on Docker container**
We will create two files one is index.html and aboutme.html and host these files on a new container running Nginx server.

1. The file structure after creating the two files will be as follows:

Use the following command to create a docker container that maps the local machine’s port to the port on which nginx serves and mount these two files to a location in the container that nginx uses to host files (/usr/share/nginx/html).

**docker run -d -p 8000:80 -v address_to_folder_with_files_locally:/usr/share/nginx/html --name my-nginx-server nginx**

Visit the address localhost:8000 in your browser and you will see the static website
And on clicking the About ME link takes you to the about me page. (localhost:8000/aboutme.html)

To stop the docker container you can use the following command.

**docker stop my-nginx-server**

**Conclusion:**

Here, we learned in brief about docker, container, images, and docker hub. Then we created a docker container using the official nginx image from the docker hub and ran it. We also learned how we can list all the images on our system and how to stop a docker container. After that, we hosted a small webpage on the nginx server running on a docker container.

**deploy an Nginx Container on to the cluster**

I have created these two .yml files, you can go and execute these files for deployment.

1.App.yml
2.Service.yml

These configuration files are used by the kubectl tool and based on the configuration of these files, The Kubernetes cluster will take all the contents of the file and do the deployment accordingly.

In this file I have given all the configurations related to deployment, basically, in this deployment, Kubernetes is going to pick the Nginx image from Docker Hub,

**Deployment file**

apiVersion: apps/v1
This section specifies the version of the deployment. When we update the deployment we can also update the version of deployment.
kind: Deployment
It specifies that this is a deployment that needs to be carried out on the Kubernetes cluster.
Specification
Replica specifies how many replicas are required to be created for this application/container since this is a demo, I have mentioned only 1. Selector specifies which application is going to be associated with this replica.

Another file that is required for this deployment is called the service.yml file. It acts as a load balancer behind that our Nginx container will run on the Kubernetes cluster. This will allow reaching the home page of the application of the Nginx container which is running on the Kubernetes cluster.

Then will execute the commands one by one.

**Kubectl apply -f app.yml**

**Kubectl apply -f service.yml**

**Kubectl apply -f service.yml**

Here we can see that our nginx-service has gotten an external-IP which will be used to reach the application deployed on a container behind this load balancer service. It has been associated with port 80.

So, If I copy this external IP and try to access my deployment nginx application. We will be able to see the Nginx page that means deployment has successfully been completed.

Here what we have done that we have deployed our application to a POD that is running on a container on the Kubernetes cluster then we have taken the external-IP/publicIP that has been assigned to our service during the deployment so that all http requests can be redirected to the backend application “nginx-app” hence “nginx-service” acts as a load balancer here.

**Kubetl delete services nginx-service**

**Conclusion**
We have successfully deployed an Nginx-app to the Kubernetes cluster with a load balancer service that is running on a container. We have also discussed how we create the deployment and service file in order to execute the deployment. We have also seen how we define the image of the application which further be pulled from either Docker Hub/ Azure Container Registry as both are used as a registry to store the images, again that can be private and public. The image which we have used is a public image provided by Nginx. I hope this demo is useful for the learning path of Azure Kubernetes service and about its important concepts.
