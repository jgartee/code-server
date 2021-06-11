FROM gradle:jre as gradle-base
FROM codercom/code-server:3.10.2

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

RUN apt-get update && apt-get -y install openjdk-11-jdk nginx wget \
    && wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb \
    &&  dpkg -i packages-microsoft-prod.deb \
    && apt-get -y install powershell \
    && apt-get clean \  
    && rm -rf /var/lib/apt/lists/*

COPY --from=gradle-base /opt/gradle /opt/gradle
ENV GRADLE_HOME=/opt/gradle
RUN ln -s "${GRADLE_HOME}/bin/gradle" /usr/bin/gradle

RUN  printf "server \n{ \nlisten 5000; \nroot /home/coder/project; \nautoindex on; \n}" > /etc/nginx/sites-available/nginx-site
RUN ln -s /etc/nginx/sites-available/nginx-site /etc/nginx/sites-enabled/nginx-site
RUN service nginx restart
RUN pwsh -Command Update-Help

USER 1000
ENV SHELL=/bin/bash
RUN code-server --install-extension ms-vscode.powershell
# RUN printf '{ "files.autoSave": "onFocusChange",\n "window.zoomLevel": 2,\n "editor.suggestSelection": "first",\n "vsintellicode.modify.editor.suggestSelection": "automaticallyOverrodeDefaultValue",\n "java.configuration.checkProjectSettingsExclusions": false,\n "java.project.importOnFirstTimeStartup": "automatic",\n "coverage-gutters.coverageFileNames": ["jacocoTestReport.xml"],\n "AllAutocomplete.dontContributeToSelf":true,\n "coverage-gutters.showLineCoverage": true,\n "coverage-gutters.showRulerCoverage": true,\n "editor.minimap.enabled": false,\n "diffEditor.ignoreTrimWhitespace": false,\n "AllAutocomplete.cycleOpenDocumentsOnLaunch": true,\n "java.semanticHighlighting.enabled": true }' > /home/coder/.local/share/code-server/User/settings.json
RUN printf "{ 'files.autoSave': 'onFocusChange',\n 'window.zoomLevel': 2,\n 'editor.suggestSelection': 'first',\n 'vsintellicode.modify.editor.suggestSelection': 'automaticallyOverrodeDefaultValue',\n 'java.configuration.checkProjectSettingsExclusions': false,\n 'java.project.importOnFirstTimeStartup': 'automatic',\n 'coverage-gutters.coverageFileNames': ['jacocoTestReport.xml'],\n 'AllAutocomplete.dontContributeToSelf':true,\n 'coverage-gutters.showLineCoverage': true,\n 'coverage-gutters.showRulerCoverage': true,\n 'editor.minimap.enabled': false,\n 'diffEditor.ignoreTrimWhitespace': false,\n 'AllAutocomplete.cycleOpenDocumentsOnLaunch': true,\n 'editor.minimap.enabled': false }" > /home/coder/.local/share/code-server/User/settings.json
RUN printf "[core]\nautocrlf = input\nsafecrlf = warn\neditor = 'code --wait'\nignorecase = false\n[alias]\nlg = log --oneline --graph --decorate --all\nfa = fetch --all\nco = checkout\nci = commit\nc = commit\n cm = commit -m \nst = status\nbr = branch\nhist = log --pretty=format:'%%C(green bold)%%h %%C(yellow)%%ad %%C(white bold)| %%C(green)%%s%%C(auto)%%d %%C(cyan bold)[%%an]' --graph --date=short --all\nr = remote -v\ntype = cat-file -t\ndump = cat-file -p\ngo = git checkout\ndc = duet-commit\ndr = duet-revert\ndm = duet-merge\ndrb = rebase -i --exec 'git duet-commit --amend'\n[user]\n[push]\ndefault = simple\n[filter \"lfs\"]\nrequired = true\nclean = git-lfs clean -- %%f\nsmudge = git-lfs smudge -- %%f\nprocess = git-lfs filter-process" > /home/coder/.gitconfig
