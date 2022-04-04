# 08 Fundamental of Deployment

## 1. Deploy a static website with windows IIS

In this section we will show how to host a static website(based on HTML) on an IIS server.

> assuming the host OS is windows server 2019

- Install a windows server on a VM
  ![install window server](../pic/08_deployment/01-windows-server-installation.png)
  Choose the option "Windows server 2019 standard evaluation(Desktop Experience)" if you are first to use Windows server.

- Install IIS server
  ![Install IIS server](../pic/08_deployment/02-install-iis.png)
  ![Install IIS server](../pic/08_deployment/03-install-iis-1.png)

- Open IIS management
  ![Open IIS management](../pic/08_deployment/05-iis-manager.png)
- Click the Host name > delete the default site
  ![delete the default site](../pic/08_deployment/06-delete-all-sites.png)
- Create a new folder named **Sites** in the user's home folder.
- Create a new folder named **MyHtmlPages** in Sites folder.
  ![creating new folders](../pic/08_deployment/07-creating-new-folder.png)
- Create a new HTML file **index.html**
  
  ```html
  <!doctype html>
  <html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Hello, My Website</title>
    <meta name="description" content="A simple HTML5 Template for new projects.">
    <meta name="author" content="SitePoint">
  </head>

  <body>
    Hi, this is my first web page I have deployed.
  </body>
  </html>

  ```

- Go to IIS management, right click **Sites** > **Add Website**
  ![add website dialogue](../pic/08_deployment/09-add-website-dialogue.png)
- When you go to browser to view <http://localhost/>, you probably will get a 401.3 Unauthorized Error page. that means we have to manually change the permission of the folder.
  ![add-permission-to-folder](../pic/08_deployment/10-add-permission-1.png)
  ![add-permission-to-folder-2](../pic/08_deployment/11-add-permission-2.png)
  **IIS_IUSRS** and **IUSR** account you **must** authorize.
- open your browser, to visit <http://localhost/>
  ![the result](../pic/08_deployment/12-result.png)

## 2. Deploy a dynamic web application with IIS(.Net 6)

- open VS 2022, and click **Build** > **Publish MyWebSite**
  ![pubulish](../pic/08_deployment/13-publish.png)
- copy the path of Folder
  ![copy the path](../pic/08_deployment/14-copy-path-folder.png)
- Click **Publish**
  ![click publish](../pic/08_deployment/15-click-publish.png)
  
- Copy the fils of Folder *bin\Release\net6.0\publish\ (or your path)* to the server. Do not forget to copy your **database** folder to publish.
  ![copy file to server](../pic/08_deployment/16-mywebsit-folder-on-server.png)
- open remote desktop to connect to the server
- Install [Microsoft .NET 6.0.3 Windows server Hosting](https://dotnet.microsoft.com/en-us/download/dotnet/6.0)
- ![dotnet runtime 6.0](../pic/08_deployment/17-dowload-dotnet-6-runtime.png)
- open IIS management to **add** a new website
- Configure the website
  ![add dotnet6 site](../pic/08_deployment/18-add-dotnet6-site.png
  )
- See the result.(configure your firewall if you can not access 8080 port)
  ![the result of website](../pic/08_deployment/19-the-result.png)
  
## 3. Deploy a dynamic web application with Nginx in Linux(Ubuntu 20.04)

- install dotnet 6 sdk on ubuntu
  https://docs.microsoft.com/en-us/dotnet/core/install/linux-ubuntu#2004-
- copy the file to server(by using FTP tools) 

- Run the command

```bash
dotnet mywebsite.dll
``` 

## 4. Build a web application with Docker Desktop on Windows

### There are two ways to generate docker files

1. Using VS2022 to build a docker image
2. Using the command line

### 4.1. Using VS2022 to depoly a docker image to Docker desktop

- Open VS2022, configure the project with following setup
  ![configure project](../pic/08_deployment/20-configure-project.png)
- Press F5 to start debugging
  
  you will get a docker image and automatically install to your docker desktop.
  ![docker desktop](../pic/08_deployment/21-docker-desktop.png)

### 4.2. Using the command line to build a docker image

Dockerfile for the project

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY . .
WORKDIR "/src/MyWebSite"
RUN ls
RUN dotnet build "MyWebSite.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "MyWebSite.csproj" -c Release -o /app/publish
# copy database to the image
COPY "/MyWebSite/db" "/app/publish/db"

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MyWebSite.dll"]
```

Run following cmd in Terminal

```powershell
cd [your project dockerfile path]
docker build -t mywebsiteimg ..
docker run -d --name mywebsiteimg -p 9000:80 mywebsiteimg
```

## 5. Publish  a Docker image to Docker Hub

- register a account on hub.docker.com
- open VS2022 and Click **Build** > **publish** [Project Name]
- select **Docker Container Registry** and Next
- select **Docker Hub** and Next
- Input username and pwd for logining Docker Hub
- click **Publish**

## 6. Deploy .Net 6 application  to a private linux server with Docker

- Install docker on the server
- pull docker image from Docker Hub

```bash
docker pull maxazure/mywebsite:latest
docker run -d --name mywebsite -p 9000:80 mywebsite
```

- configure a reverse proxy in Nginx
  
  <https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/linux-nginx?view=aspnetcore-6.0>

/etc/nginx/sites-enabled/mywebsite.709.co.nz

```nginx
server {
        server_name mywebsite.709.co.nz;
        index index.html index.htm;

        location / {
        include proxy_params;
        proxy_pass http://127.0.0.1:8190;
        add_header Access-Control-Allow-Origin *;
        add_header Access-Control-Allow-Methods 'GET, POST, OPTIONS, PUT, DELETE';
        }
}
```

- config HTTPS by using Certbot

## 7. Deploy .Net 6 application to Azure Cloud with Docker

- Go to https://portal.azure.com/#home to register an account
- find and read the docs of App service

### Deploy a docker image with App service on Azure Cloud

- Select **App Services**
  ![azure 1](../pic/08_deployment/41-azure1.png)
- click **Create**
  ![azure 2](../pic/08_deployment/42-azure2.png)
- Fill the form of **Create Web App**
  ![azure 3](../pic/08_deployment/43-azure3.png)
