FROM gradle:jre as gradle-base
FROM codercom/code-server:3.9.0

USER root

ARG LABEL_VERSION="3.1.5.0"
ARG INSTALL_VERSION="dotnet-sdk-3.1 dotnet-sdk-5.0"

# Enable .NET detection of running in a container
# See: https://github.com/dotnet/dotnet-docker/blob/master/3.0/sdk/bionic/amd64/Dockerfile
ENV DOTNET_RUNNING_IN_CONTAINER=true \
    # Enable correct mode for dotnet watch (only mode supported in a container)
    DOTNET_USE_POLLING_FILE_WATCHER=true \
    # Skip extraction of XML docs - generally not useful within an image/container - helps performance
    NUGET_XMLDOC_MODE=skip \
    # No installer frontend interaction
    DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get -y install wget nginx \
    # Register the Microsoft repository, make sure it matches the platform
    && wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb \
    && dpkg -i packages-microsoft-prod.deb \
    && apt-get update \
    # Install .NET SDK and PowerShell
    && apt-get install -y dotnet-sdk-3.1 \
    # https://docs.microsoft.com/en-us/dotnet/core/install/linux-ubuntu
    && apt-get install -y ${INSTALL_VERSION} powershell \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
    
RUN  printf "server \n{ \nlisten 5000; \nroot /home/coder/project; \nautoindex on; \n}" > /etc/nginx/sites-available/nginx-site
RUN ln -s /etc/nginx/sites-available/nginx-site /etc/nginx/sites-enabled/nginx-site
RUN service nginx restart

USER 1000
ENV SHELL=/bin/bash
RUN code-server --install-extension ms-dotnettools.csharp
RUN code-server --install-extension ryanluker.vscode-coverage-gutters
RUN code-server --install-extension formulahendry.dotnet-test-explorer
# RUN code-server --install-extension josefpihrt-vscode.roslynator
RUN code-server --install-extension lacroixdavid1.vscode-format-context-menu
# RUN code-server --install-extension jmrog.vscode-nuget-package-manager
