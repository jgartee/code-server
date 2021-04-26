FROM gradle:jre as gradle-base
FROM codercom/code-server:3.9.0

USER root

RUN apt-get update && apt-get -y install openjdk-11-jdk nginx
COPY --from=gradle-base /opt/gradle /opt/gradle
ENV GRADLE_HOME=/opt/gradle
RUN ln -s "${GRADLE_HOME}/bin/gradle" /usr/bin/gradle

RUN  printf "server \n{ \nlisten 5000; \nroot /home/coder/project; \nautoindex on; \n}" > /etc/nginx/sites-available/nginx-site
RUN ln -s /etc/nginx/sites-available/nginx-site /etc/nginx/sites-enabled/nginx-site
RUN service nginx restart

USER 1000
ENV SHELL=/bin/bash
RUN code-server --install-extension vscjava.vscode-java-pack
RUN code-server --install-extension ryanluker.vscode-coverage-gutters
RUN printf '{ "files.autoSave": "onFocusChange",\n "window.zoomLevel": 2,\n "editor.suggestSelection": "first",\n "vsintellicode.modify.editor.suggestSelection": "automaticallyOverrodeDefaultValue",\n "java.configuration.checkProjectSettingsExclusions": false,\n "java.project.importOnFirstTimeStartup": "automatic",\n "coverage-gutters.coverageFileNames": ["jacocoTestReport.xml"],\n "AllAutocomplete.dontContributeToSelf":true,\n "coverage-gutters.showLineCoverage": true,\n "coverage-gutters.showRulerCoverage": true,\n "editor.minimap.enabled": false,\n "diffEditor.ignoreTrimWhitespace": false,\n "AllAutocomplete.cycleOpenDocumentsOnLaunch": true }' > /home/coder/.local/share/code-server/User/settings.json