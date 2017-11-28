# escape=`

FROM microsoft/windowsservercore:latest

SHELL ["powershell", "-command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# TODO: gpg signature verification for SHASUMS256.txt
RUN $NODE_VERSION = $(($(curl 'https://nodejs.org/dist/index.json' -UseBasicParsing | ConvertFrom-Json) | ? lts -EQ "Carbon").version | sort -Descending)[0]; `
    curl 'https://nodejs.org/dist/latest-carbon/SHASUMS256.txt' -OutFile 'SHASUMS256.txt' -UseBasicParsing; `
    curl $('https://nodejs.org/dist/latest-carbon/node-{0}-x64.msi' -f $NODE_VERSION) -OutFile 'node.msi' -UseBasicParsing; `
    $sum = $($(cat SHASUMS256.txt | sls $('node-{0}-x64.msi' -f $NODE_VERSION)) -Split ' ')[0]; `
    if ((Get-FileHash node.msi -Algorithm sha256).Hash -ne $sum) { throw 'SHA checksum verification failed' }

SHELL ["cmd", "/S", "/C"]

RUN msiexec /i C:\node.msi /quiet /qn /norestart /log C:\node-install.log && del C:\node.msi && del C:\SHASUMS256.txt
