# CICD with Docker and VSTS

## Introduction

In this lab you will automate building an app, a container image, push it and optionally deploy.

## Prerequisites

For this lab you should have access to a Windows developer workstation and access to Visual Studio Team Services.

Ideally you are reusing the VM you used to create the app.

## Integrate with version control

Create vsts account, project
In code copy git url

Git init
git remote add origin https://vstswedocker.visualstudio.com/_git/minilab
Git add .
git config user.email "you@example.com"
git config user.name "your name"

Git commit -m "commit message"
git push -u origin --all

Go to VSTS Code, see it is there

## Create the private build agent

https://docs.microsoft.com/en-us/vsts/build-release/actions/agents/v2-windows

https://vstswedocker.visualstudio.com/_admin/_AgentPool
Download
mkdir agent ; cd agent
Add-Type -AssemblyName System.IO.Compression.FileSystem ;[System.IO.Compression.ZipFile]::ExtractToDirecto
ry("$HOME\Downloads\vsts-agent-win-x64-2.127.0.zip", "$PWD")
.\Configure.ps1
<vsts fqdn> (https://<your account>.visualstudio.com)
Enter for pat
Paste token
Enter for default pool
Enter for computername
Enter for workfolder
Y for run as service
Enter admin userid
Enter password

## Create the build definition

Go to VSTS Build, new build definition using ASP.NET Core template
Enter default pool
Save & queue to test the build

Edit build def
After publish add 2 tasks: build image, push image
Replace imagename:<tag> with your dtr/image/repo:<tag>
Trigger: enable ci

## Run the build

Queue a new build to see it is working.

[Docker Enteprise Edition and Windows basics](./201EEWin.md)