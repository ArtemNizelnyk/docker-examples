# escape=`

ARG BASE_IMAGE
ARG HEADLESS_SERVICES_IMAGE
ARG MANAGEMENT_SERVICES_IMAGE
ARG TOOLS_IMAGE
ARG SOLUTION_IMAGE

FROM ${SOLUTION_IMAGE} as solution
FROM ${TOOLS_IMAGE} as tools
FROM ${MANAGEMENT_SERVICES_IMAGE} as management_services
FROM ${HEADLESS_SERVICES_IMAGE} as headless_services
FROM ${BASE_IMAGE}

ARG JSS_APP_NAME

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# Copy development tools and entrypoint
COPY --from=tools \tools\ \tools\

# Install Node.js
RUN Invoke-WebRequest -OutFile C:\nodejs.zip -UseBasicParsing "https://nodejs.org/dist/v14.7.0/node-v14.7.0-win-x64.zip"; `
    Expand-Archive C:\nodejs.zip -DestinationPath C:\; `
    Rename-Item "C:\\node-v14.7.0-win-x64" C:\nodejs; `
    Remove-Item C:\nodejs.zip;
RUN setx /M PATH $($Env:PATH + ';C:\nodejs')
RUN npm config set registry https://registry.npmjs.org/

WORKDIR C:\inetpub\wwwroot

# Copy the Sitecore Management Services Module
COPY --from=management_services \module\cm\content .\

# Copy and init the JSS / Headless Services Module
COPY --from=headless_services \module\cm\content .\
COPY --from=headless_services \module\tools \module\tools
RUN C:\module\tools\Initialize-Content.ps1 -TargetPath .\; `
    Remove-Item -Path C:\module -Recurse -Force;

# Copy solution JSS files. The distribution folder should match the "sitecoreDistPath" in package.json.
COPY --from=solution \artifacts\jss\ .\dist\${JSS_APP_NAME}\

# Copy solution platform files
COPY --from=solution \artifacts\platform\ .\