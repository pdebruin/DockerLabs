# ASP.NET Core app on Windows

## Introduction

In this lab you will build a new ASP.NET Core project and package it in a container.

## Prerequisites

For this lab you should have access to a Windows developer workstation. When working with containers it is easiest to use a VM in the cloud, so that you get started quickly, have plenty network bandwidth and stop whenever you are done. For instance use the Azure marketplace to create a Windows Server 2016 VM with Visual Studio 2017 installed.

RDP into VSTS Private Agent
Server manager do not start
IESC off

In this lab you will need the .NET Core SDK and the Docker engine. Run the below commands to check that software is available on your machine.

```powershell
dotnet --version
```

If this is not available, download it. 

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

## Build the app

The first step is to build the app:

```powershell
cd\
md _projects; cd _projects
md aspcoremvc; cd aspcoremvc
dotnet new mvc
dotnet publish
```

You now have standard ASP.NET Core MVC application. Next you will add a Dockerfile with deployment instructions to create a Docker container image. In the root of your webapp create a new file called Dockerfile (no extension) with Notepad, VS Code, Visual Studio or any other editor.

```Dockerfile
#Make sure this tag matches your version of ASP.NET Core
FROM microsoft/aspnetcore:1.1
ARG source
WORKDIR /app
EXPOSE 80
#Make sure this case-sensitive path contains your published app
COPY ${source:-bin/Debug/netcoreapp1.1/publish} .
#Make sure this dll matches the name of your app
ENTRYPOINT ["dotnet", "aspcoremvc.dll"]
```

In the folder where your webapp and Dockerfile live, run this command to build the image.

```powershell
docker build . -t aspcoremvc
```

With `docker images` you can see what images you have. There should now at least be the aspnetcore base image as well as your new aspcoremvc image. And with `docker ps` you can see that containers are running. There should be none, but please check because you can only bind one port to one container. If all is good, run the container locally with:

```powershell
docker run -d -p 80:80 aspcoremvc
```

With `docker ps` you can see if the container is actually running. And with `docker inspect <container id>` you can get the ip address of the container (near the end of long output). Browse to the ip address and you should see your application running.

You can now go back to the main lab to push your image to DTR and deploy it to UCP.

[Docker Enteprise Edition and Windows basics](./201EEWin.md)