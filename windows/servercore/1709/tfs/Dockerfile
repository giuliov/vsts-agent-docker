FROM microsoft/windowsservercore:1709
ENV WINDOWS_IMAGE_VERSION=1709

RUN mkdir C:\BuildAgent
WORKDIR C:/BuildAgent

RUN powershell -NoProfile -Command \
        $ErrorActionPreference = 'Stop'; \
        Invoke-WebRequest -Uri https://go.microsoft.com/fwlink/?linkid=858950 -OutFile C:\BuildAgent\agent.zip; \
	Expand-Archive -Path C:\BuildAgent\agent.zip -DestinationPath C:\BuildAgent; \
	Remove-Item -Path C:\BuildAgent\agent.zip

COPY ./Start.* ./

CMD ["Start.cmd"]