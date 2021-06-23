# Dockerfile for Nano Server + IIS Role + PHP.

FROM nanoserver/iis

MAINTAINER nanoserver.es@gmail.com
# Install .NET
ENV DOTNET_VERSION=5.0.0

ADD https://windows.php.net/downloads/releases/php-7.1.19-nts-Win32-VC14-x64.zip php.zip
ADD https://jbit.es/files/vcruntime140.dll C:\\Windows\\System32\\vcruntime140.dll
ADD https://jbit.es/files/iisfcgi.dll C:\\Windows\\System32\\inetsrv\\iisfcgi.dll
ADD https://jbit.es/files/info.dll C:\\inetpub\\wwwroot\\info.php

ENV PHP C:\\php

RUN powershell -command Expand-Archive -Path c:\php.zip -DestinationPath C:\php

RUN setx PATH /M %PATH%;C:\php 

ADD https://jbit.es/files/php.ini C:\\php\\php.ini

RUN powershell -command \

    rm C:\Windows\System32\inetsrv\config\Applicationhost.config ; \
    Invoke-WebRequest -uri https://jbit.es/files/ApplicationHost.dll -outfile C:\\Windows\\System32\\inetsrv\\config\\Applicationhost.config ; \
    Remove-Item c:\php.zip -Force

RUN powershell -Command \
        $ErrorActionPreference = 'Stop'; \
        $ProgressPreference = 'SilentlyContinue'; \
        Invoke-WebRequest -OutFile dotnet.zip https://dotnetcli.azureedge.net/dotnet/Runtime/$Env:DOTNET_VERSION/dotnet-runtime-$Env:DOTNET_VERSION-win-x64.zip; \
        $dotnet_sha512 = '968f2aff18aabc8f9ef0e1a6a3c0c3704d7a010ed453f0d56ded804513ccff913032461f1dbe0ce89522e85fbe5593d444f4310e8fdf7de7c7b72d01912086f1'; \
        if ((Get-FileHash dotnet.zip -Algorithm sha512).Hash -ne $dotnet_sha512) { \
            Write-Host 'CHECKSUM VERIFICATION FAILED!'; \
            exit 1; \
        }; \
        \
        Expand-Archive dotnet.zip -DestinationPath dotnet; \
        Remove-Item -Force dotnet.zip
