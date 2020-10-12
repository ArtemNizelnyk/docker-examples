# escape=`

ARG BASE_IMAGE
ARG HEADLESS_RESOURCES_IMAGE

FROM ${HEADLESS_RESOURCES_IMAGE} as headless_services_image

FROM ${BASE_IMAGE} AS spe

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# Add JSS module
COPY --from=headless_services_image C:\module\db C:\jss_data
RUN C:\DeployDatabases.ps1 -ResourcesDirectory C:\jss_data; `
    Remove-Item -Path C:\jss_data -Recurse -Force;