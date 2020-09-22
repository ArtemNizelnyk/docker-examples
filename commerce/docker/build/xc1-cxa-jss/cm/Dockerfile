# escape=`

ARG BASE_IMAGE
ARG SPE_RESOURCES_IMAGE
ARG SXA_RESOURCES_IMAGE
ARG HEADLESS_SERVICES_IMAGE
ARG TOOLING_IMAGE
ARG MANAGEMENT_SERVICES_IMAGE
ARG SOLUTION_IMAGE


FROM ${SOLUTION_IMAGE} as solution

FROM ${TOOLING_IMAGE} AS tooling

FROM ${SPE_RESOURCES_IMAGE} AS spe_resources

FROM ${SXA_RESOURCES_IMAGE} AS sxa_resources

FROM ${HEADLESS_SERVICES_IMAGE} as headless_services

FROM ${MANAGEMENT_SERVICES_IMAGE} as management_services

FROM ${BASE_IMAGE} AS spe

COPY --from=tooling \tools\ \tools\
COPY --from=spe_resources C:\module\cm\content C:\inetpub\wwwroot

#COPY wdp/ C:/inetpub/wwwroot
#Copy JSS files

COPY --from=headless_services C:\module\cm\content C:\inetpub\wwwroot
COPY --from=headless_services C:\module\tools C:\module\tools

# Copy solution website files
COPY --from=solution \artifacts\website\ .\

# Copy solution transforms
COPY --from=solution \artifacts\transforms\ \transforms\solution\

# Copy role transforms
#COPY .\transforms\ \transforms\role\

# Perform solution transforms
RUN C:\tools\scripts\Invoke-XdtTransform.ps1 -Path .\ -XdtPath C:\transforms\solution\DockerExamples.Website

# Perform role transforms
#RUN C:\tools\scripts\Invoke-XdtTransform.ps1 -Path .\ -XdtPath C:\transforms\role
