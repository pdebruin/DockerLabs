# Dockerize a new ASP.NET core application

**From the commandline**

Create new dir 

dotnet new mvc

dotnet restore

dotnet build

dotnet publish

dockerfile
FROM microsoft/aspnetcore:1.1.1-nanoserver
ARG source
WORKDIR /app
ENV ASPNETCORE_URLS 
http://+:80
EXPOSE 80
COPY ${source:-obj/Docker/publish} .
ENTRYPOINT ["dotnet", "WebApplication1.dll"]

docker build



**Using Visual Studio**
