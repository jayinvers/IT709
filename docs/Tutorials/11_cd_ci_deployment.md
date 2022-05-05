# 10 CI(Continuous Integration) & CD(Continuous Delivery)

## Intro

When software is developed in a team, conflicts often arise due to many people contributing code to the same project. Therefore, the development and release of Internet software has formed a set of standard processes, and the most important component is continuous integration (CI).

![CI](/pic/10_cd_ci/00.png)

Today we will introduce the concepts of continuous integration and continuous delivery, and how they are implemented in **Github** and **Teamcity**.

[Video: What is Continuous Integration?](https://www.youtube.com/watch?v=1er2cjUq1UI)

## Popular Software for CI/CD

- Jenkins
- TeamCity
- CircleCI
- GitLab
- Github Actions

## CI/CD in Teamcity

[Install Teamcity with a docker image](https://hub.docker.com/r/jetbrains/teamcity-server)

[Install and Start TeamCity Agents](https://www.jetbrains.com/help/teamcity/install-and-start-teamcity-agents.html)

[Integrating TeamCity with Docker](https://www.jetbrains.com/help/teamcity/integrating-teamcity-with-docker.html)

[Building .NET Projects Using TeamCity](https://www.jetbrains.com/teamcity/tutorials/dotnet-build-configure-test/)

## Deploy a .NET Web Application with TeamCity

![.Net Web App on Linux](/pic/10_cd_ci/02.png)

### TASKs

1. Deploy a .NET web app on Linux with TeamCity
2. Deploy a .NET web app as a docker image on Linux with Teamcity

## Useful config file regarding .net core app on Linux

This configuration file is for configuring a service for .net core web app.

sudo vim /etc/systemd/system/username709conz.service

```bash
[Unit]
Description=username709conz

[Service]
WorkingDirectory=/home/ubuntu/sites/username.709.co.nz/app
ExecStart=/usr/bin/dotnet /home/ubuntu/sites/username.709.co.nz/app/Portfolio.dll --urls "http://localhost:7001"
Restart=always
# If the .net core service crashes, try restarting after 10 seconds
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-username709conz
User=ubuntu
Group=ubuntu
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

Enable and start the service

```bash
sudo vim /etc/systemd/system/username709conz.service
sudo systemctl enable username709conz.service
sudo service username709conz restart
journalctl -fu username709conz.service
```

Nginx with proxy
[Host ASP.NET Core on Linux with Nginx](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/linux-nginx?view=aspnetcore-6.0)