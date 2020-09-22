# escape=`

ARG BASE_IMAGE
ARG SXA_RESOURCES_IMAGE
ARG SOLUTION_IMAGE
ARG HEADLESS_SERVICES_IMAGE


FROM ${SOLUTION_IMAGE} as solution

FROM ${SXA_RESOURCES_IMAGE} AS sxa_resources

FROM ${HEADLESS_SERVICES_IMAGE} as headless_services

FROM ${BASE_IMAGE}

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# Install Node.js
RUN Invoke-WebRequest -OutFile C:\nodejs.zip -UseBasicParsing "https://nodejs.org/dist/v14.7.0/node-v14.7.0-win-x64.zip"; `
    Expand-Archive C:\nodejs.zip -DestinationPath C:\; `
    Rename-Item "C:\\node-v14.7.0-win-x64" C:\nodejs; `
    Remove-Item C:\nodejs.zip;
RUN setx /M PATH $($Env:PATH + ';C:\nodejs')
RUN npm config set registry https://registry.npmjs.org/

WORKDIR C:\inetpub\wwwroot

COPY --from=sxa_resources C:\module\cd\content C:\inetpub\wwwroot
COPY --from=sxa_resources C:\module\tools C:\module\tools

COPY wdp/ C:/inetpub/wwwroot
COPY xdts/ C:/inetpub/wwwroot/App_Data/SxaXdt

#Copy JSS files
    COPY --from=headless_services_image C:\module\cd\content C:\inetpub\wwwroot
    COPY --from=headless_services_image C:\module\tools C:\module\tools

RUN C:\module\tools\Initialize-Content.ps1 -TargetPath C:\inetpub\wwwroot; `
    Remove-Item -Path C:\module -Recurse -Force;

     # Copy solution JSS files. The distribution folder should match the "sitecoreDistPath" in package.json.
COPY --from=solution \artifacts\jss\ .\dist\${JSS_APP_NAME}\

    # Copy solution website files
COPY --from=solution \artifacts\website\ .\

# Copy solution transforms
COPY --from=solution \artifacts\transforms\ \transforms\solution\

# Perform solution transforms
RUN C:\tools\scripts\Invoke-XdtTransform.ps1 -Path .\ -XdtPath C:\transforms\solution\DockerExamples.Website
   