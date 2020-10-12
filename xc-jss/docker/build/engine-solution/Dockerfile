#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

ARG BASE_IMAGE
ARG COMMERCE_BUILD_IMAGE

FROM ${COMMERCE_BUILD_IMAGE} as build


WORKDIR /app
COPY ["src/Sitecore.Commerce.Engine/Sitecore.Commerce.Engine.csproj", "src/Sitecore.Commerce.Engine/"]
COPY ["src/Plugin.Sample.ContentHub/Plugin.Sample.ContentHub.csproj", "src/Plugin.Sample.ContentHub/"]
COPY ["src/Plugin.Sample.Habitat/Plugin.Sample.Habitat.csproj", "src/Plugin.Sample.Habitat/"]
COPY ["src/Plugin.Sample.AdventureWorks/Plugin.Sample.AdventureWorks.csproj", "src/Plugin.Sample.AdventureWorks/"]
COPY ["src/Plugin.Sample.Payments.Braintree/Plugin.Sample.Payments.Braintree.csproj", "src/Plugin.Sample.Payments.Braintree/"]
COPY ["nuget.config", "."]
COPY ["Customer.Sample.Solution.sln", "."]
RUN dotnet restore "Customer.Sample.Solution.sln"


COPY . .

RUN dotnet publish "Customer.Sample.Solution.sln" -c Release -o /app/out

FROM ${BASE_IMAGE}

WORKDIR C:/artifacts

# Copy final build artifacts
COPY --from=build /app/out ./engine