FROM giuliov/vsts-agent:1709

ENV chocolateyUseWindowsCompression=false
RUN @powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"

RUN choco config set cachelocation C:\chococache

RUN choco install \
    git  \
    nodejs \
    jdk8 \
    curl \
    maven \
    gradle \
    ant \
    docker \
    ruby \
    --confirm \
    --limit-output \
    --timeout 216000 \
    && rmdir /S /Q C:\chococache

RUN choco install \
    dotnet4.6.1 \
    --confirm \
    --limit-output \
    && rmdir /S /Q C:\chococache

RUN choco install \
    visualstudio2017enterprise \
    --package-parameters "--passive --locale en-US --includeOptional" \
    --confirm \
    --limit-output \
    --timeout 216000 \
    && rmdir /S /Q C:\chococache

RUN choco install \
    visualstudio2017-workload-azure \
    visualstudio2017-workload-netcoretools \
    visualstudio2017-workload-netweb \
    --confirm \
    --limit-output \
    --timeout 21600 \
    --package-parameters "--includeOptional" \
    && rmdir /S /Q C:\chococache

# common node tools
RUN npm install gulp -g && npm install grunt -g && npm install -g less && npm install phantomjs-prebuilt -g

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]


# Install .NET Core
ENV DOTNET_VERSION 2.0.5
ENV DOTNET_DOWNLOAD_URL https://download.microsoft.com/download/1/1/0/11046135-4207-40D3-A795-13ECEA741B32/dotnet-runtime-${DOTNET_VERSION}-win-x64.zip

RUN Invoke-WebRequest $Env:DOTNET_DOWNLOAD_URL -OutFile dotnet.zip; \
    Expand-Archive dotnet.zip -DestinationPath $Env:ProgramFiles\dotnet -Force; \
    Remove-Item -Force dotnet.zip
    
# Install .NET Core SDK
ENV DOTNET_SDK_VERSION 2.1.4
ENV DOTNET_SDK_DOWNLOAD_URL https://download.microsoft.com/download/1/1/5/115B762D-2B41-4AF3-9A63-92D9680B9409/dotnet-sdk-${DOTNET_SDK_VERSION}-win-x64.zip

RUN Invoke-WebRequest $Env:DOTNET_SDK_DOWNLOAD_URL -OutFile dotnet.zip; \
    Expand-Archive dotnet.zip -DestinationPath $Env:ProgramFiles\dotnet -Force; \
    Remove-Item -Force dotnet.zip

SHELL ["cmd", "/S", "/C"]

RUN setx /M PATH "%PATH%;%ProgramFiles%\dotnet"

# Trigger the population of the local package cache
ENV NUGET_XMLDOC_MODE skip

RUN mkdir C:\warmup \
    && cd C:\warmup \
    && dotnet new \
    && cd .. \
    && rmdir /S /Q C:\warmup 

