FROM microsoft/aspnetcore:2.0.0-nanoserver
ARG source
WORKDIR /app
EXPOSE 80
COPY ${source:-bin/Debug/netcoreapp2.0/publish} .
ENTRYPOINT ["dotnet", "dotnetreact.dll"]
