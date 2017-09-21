# Image to repository

## Introduction
The simplest way to work with images and repositories is to pull a Docker image from Docker Hub, rename it using docker tag and push it back to Docker Hub. Although that is good if you haven't done it before, this lab actually focuses more on the enterprise scenario where source code is compiled, deployed to an image and pushed to your private repository. 

In this lab we will work with Visual Studio Team Services for code and build functionality and a Docker Trusted Registry. If your environment looks differently, you can use similar concepts in your own CICD system and container repository. 

## Prerequisites
We are assuming you already have source code of your app in VSTS version control and that you can build it with VSTS build. If you don't have that, do that first. This is very simple if you work with the Git and Dotnet command-line. 
* Create a new VSTS team project using GIT version control. Copy the git statement.
* Create a new dotnet mvc web app 
* Push the new app to the new team project using the git statement.  

## Build an image
Let's start by extending a default build 

## Push image to repository