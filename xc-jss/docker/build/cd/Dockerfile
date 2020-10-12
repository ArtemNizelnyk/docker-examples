# escape=`

ARG BASE_IMAGE
ARG TOOLING_IMAGE
ARG SOLUTION_IMAGE
ARG HEADLESS_RESOURCES_IMAGE

FROM ${SOLUTION_IMAGE} as solution
FROM ${TOOLING_IMAGE} as tooling
FROM ${HEADLESS_RESOURCES_IMAGE} as headless_services
FROM ${BASE_IMAGE}

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# Copy development tools and entrypoint
COPY --from=tooling \tools\ \tools\

# Install Node.js
#RUN Invoke-WebRequest -OutFile C:\nodejs.zip -UseBasicParsing "https://nodejs.org/dist/v14.7.0/node-v14.7.0-win-x64.zip"; `
 #   Expand-Archive C:\nodejs.zip -DestinationPath C:\; `
  #  Rename-Item "C:\\node-v14.7.0-win-x64" C:\nodejs; `
  #  Remove-Item C:\nodejs.zip;
#RUN setx /M PATH $($Env:PATH + ';C:\nodejs')
#RUN npm config set registry https://registry.npmjs.org/



WORKDIR C:\inetpub\wwwroot

#Copy JSS files
    COPY --from=headless_services C:\module\cd\content C:\inetpub\wwwroot
    COPY --from=headless_services C:\module\tools C:\module\tools

RUN C:\module\tools\Initialize-Content.ps1 -TargetPath C:\inetpub\wwwroot; `
    Remove-Item -Path C:\module -Recurse -Force;

# Copy solution website files
COPY --from=solution \artifacts\website\ .\

# Copy solution transforms
COPY --from=solution \artifacts\transforms\ \transforms\solution\

# Perform solution transforms
RUN C:\tools\scripts\Invoke-XdtTransform.ps1 -Path .\ -XdtPath C:\transforms\solution\DockerExamples.Website