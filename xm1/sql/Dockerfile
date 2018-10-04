# escape=`
FROM microsoft/mssql-server-windows-developer

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# Add files
ADD ./install /install

# Set workdir
WORKDIR /install

# Set defaults for required (when running) env variables
ENV ACCEPT_EULA=Y
ENV sa_password=HASH-epsom-sunset-cost7!

RUN Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1')); choco install sql2017-dacframework -Y

# Install databases
ARG SQL_DB_PREFIX="Sitecore"
ENV SQL_PACKAGE_EXE='C:\Program Files\Microsoft SQL Server\*\DAC\bin\SqlPackage.exe'
RUN $name = '{0}_Core' -f $env:SQL_DB_PREFIX; & $env:SQL_PACKAGE_EXE /a:Publish /sf:'./Sitecore.Core.dacpac' /tdn:$name /tsn:$env:COMPUTERNAME; `
    $name = '{0}_Master' -f $env:SQL_DB_PREFIX; & $env:SQL_PACKAGE_EXE /a:Publish /sf:'./Sitecore.Master.dacpac' /tdn:$name /tsn:$env:COMPUTERNAME; `
    $name = '{0}_Web' -f $env:SQL_DB_PREFIX; & $env:SQL_PACKAGE_EXE /a:Publish /sf:'./Sitecore.Web.dacpac' /tdn:$name /tsn:$env:COMPUTERNAME; `
    $name = '{0}_ExperienceForms' -f $env:SQL_DB_PREFIX; & $env:SQL_PACKAGE_EXE /a:Publish /sf:'./Sitecore.ExperienceForms.dacpac' /tdn:$name /tsn:$env:COMPUTERNAME

# Reset workdir
WORKDIR /

# Cleanup
RUN Remove-Item 'C:/install' -Force -Recurse