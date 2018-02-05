# Docker Enterprise Edition and Windows

## Introduction

Since Windows Server 2016 you can run containers in Windows and since Docker EE 17.06 your can manage Windows workers in the same way you already could manage Linux workers. [More info of Docker Enterprise Edition](http://www.docker.com/enterprise). In this lab the focus is on getting to know the Universal Control Plane (UCP), the management portal that helps manage a large cluster of containers hosts and apps, and Docker Trusted Registry (DTR), the private registry that helps store and secure your images.

In this lab you will use a container image to push it to DTR and deploy it in UCP.

## Prerequisites

For this lab you should have access to a cluster running Docker EE with Windows workers.

Before getting started, let's make sure you have a good developer workstation. When working with containers it is easiest to use a VM in the cloud, so that you get started quickly, have plenty of network bandwidth and stop whenever you are done. For instance use the Azure marketplace to create a Windows Server 2016 VM with Visual Studio 2017 installed.

RDP into VSTS Private Agent
Server manager do not start
IESC off

In this lab you will need the Docker engine. Run the below command to check that software is available on your machine.

```powershell
docker version
```

If this is not available, run

```powershell
install-windowsfeature containers
Install-Module -Name DockerMsftProvider -Repository PSGallery -Force
Install-Package -Name docker -ProviderName DockerMsftProvider
```

And then restart.

## Get the app

For this lab you can either use an existing app in a container image or create a new one. 

[ASP.NET Core app on Windows](./202AspNetCore.md)

To just reuse an existing image, run the following command:

```powershell
docker pull pdebruin/aspcoremvc
docker tag pdebruin/aspcoremvc aspcoremvc
```

With `docker images` you can see what images you have. There should now at least be the aspnetcore base image as well as your new aspcoremvc image. And with `docker ps` you can see that containers are running. There should be none, but please check because you can only bind one port to one container. If all is good, run the container locally with:

```powershell
docker run -d -p 80:80 aspcoremvc
```

With `docker ps` you can see if the container is actually running. And with `docker inspect <container id>` you can get the ip address of the container (near the end of long output). Browse to the ip address and you should see your application running. Now you can stop your local container using `docker stop <container id>`.

## Push your container image

You can use push and pull commands to upload and download images to and from a registry. By default the Docker daemon will use Docker Hub as registry. You can change this by renaming the container image. To find your DTR and user id first browse to UCP. In UCP go to admin settings and DTR to find the DTR URL.

In the Docker Trusted Registry you will create a repository. Go to DTR, click Repositories and click New Repository. Note your userid is set as default and enter aspcoremvc as repository name. Next click Save. When the repository is created, you can see the fully qualified domain name + userid + image name in the docker pull textbox on the info tab. You will use this full string (without docker pull) next to give your local image the appropriate name.

Note: This fqdn is very long, which is because you are using a generated name for DNS of DTR. Normally you would have a user-friendly URL like dtr.yourcompany.com.

Optional: In DTR check to see if image scanning is enabled. Go to Settings and then the Security tab to see details about image scanning. If it is disabled, you can enable it here. This will update the CVE database of your DTR to contain the latest vulnerability definitions. 

```powershell
docker tag aspcoremvc <dtr fqdn>/<user id>/aspcoremvc
```

Note: Docker Enterprise Edition uses self-signed certificates, so that https is configured by default, and can easily be updated with a certificate authority your own or from a third party. The Docker client will actually see the self-signed certs and refuse to work with it unless you tell it to. To register your DTR with self-signed certs, run:

```powershell
stop-service docker
dockerd --unregister-service
dockerd --register-service --insecure-registry <dtr fqdn>
start-service docker
```

Now sign in to your DTR

```powershell
docker login <dtr fqdn>
```

and push the image

```powershell
docker push <dtr fqdn>/<user id>/aspcoremvc
```

## Deploy your container image

In the UCP go to homepage, click Services and Create Service. Enter aspcoremvc as name, as image use image name (<dtr fqdn>/<user id>/aspcoremvc). Next go to Network, select DNSRR, enter 80 for internal port, select Host mode and enter 8082 for public port, click Confirm. And click create. 

Now you should be able to access the containerized app through the Windows worker's load-balancer's DNS or IP and port 8082.

You have now pushed an image to DTR and deployed a service to UCP. The next step is to automate most of this as part of your CI/CD pipeline.

[CICD with Docker and VSTS](./203VSTS.md)

Finally you can set up monitoring for your app and/or cluster

[Monitoring with Docker and OMS](./204OMS.md)
