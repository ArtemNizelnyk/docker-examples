# escape=`

ARG BASE_IMAGE
ARG SPE_RESOURCES_IMAGE
ARG SXA_RESOURCES_IMAGE
ARG HEADLESS_SERVICES_IMAGE

FROM ${SPE_RESOURCES_IMAGE} AS spe_resources

FROM ${SXA_RESOURCES_IMAGE} AS sxa_resources

FROM ${HEADLESS_SERVICES_IMAGE} as headless_services

FROM ${BASE_IMAGE} AS spe

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

COPY --from=spe_resources C:\module\db C:\spe_data

RUN C:\DeployDatabases.ps1 -ResourcesDirectory C:\spe_data; `
    Remove-Item -Path C:\spe_data -Recurse -Force;

FROM spe AS sxa

COPY --from=sxa_resources C:\module\db C:\sxa_data

RUN C:\DeployDatabases.ps1 -ResourcesDirectory C:\sxa_data; `
    Remove-Item -Path C:\sxa_data -Recurse -Force;

COPY dacpacs/ C:/commerce_data

RUN C:\DeployDatabases.ps1 -ResourcesDirectory C:\commerce_data; `
    Remove-Item -Path C:\commerce_data -Recurse -Force;

    # Copy JSS data
    COPY --from=headless_services_image C:\module\db C:\jss_data
RUN C:\DeployDatabases.ps1 -ResourcesDirectory C:\jss_data; `
    Remove-Item -Path C:\jss_data -Recurse -Force;