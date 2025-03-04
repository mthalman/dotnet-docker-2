ARG REPO=mcr.microsoft.com/dotnet/runtime

# Installer image
FROM mcr.microsoft.com/azurelinux/base/core:3.0@sha256:7caf6bede8a3fa62340581a6e5409ede9564b6866d18143ffd0b691c8f2cfcfc AS installer

RUN tdnf install -y \
        ca-certificates \
        gzip \
        tar \
    && tdnf clean all

# Retrieve ASP.NET Core
RUN aspnetcore_version=8.0.13 \
    && curl -fSL --output aspnetcore.tar.gz https://builds.dotnet.microsoft.com/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-$aspnetcore_version-linux-x64.tar.gz \
    && aspnetcore_sha512='7b21aff45c3ca7ccdc0527c6de05c209d58a56a15cf10e656522261f884cf272a92be13696b1a0f1ae2baaa0d825ffda58d954871a17b3c3a8659a9f3a36c7e6' \
    && echo "$aspnetcore_sha512  aspnetcore.tar.gz" | sha512sum -c - \
    && mkdir -p /dotnet \
    && tar -oxzf aspnetcore.tar.gz -C /dotnet ./shared/Microsoft.AspNetCore.App \
    && rm aspnetcore.tar.gz


# ASP.NET Core image
FROM $REPO:8.0.13-azurelinux3.0-distroless-extra-amd64@sha256:d98176dd88fdc04ed5fd00a81f1fb2f83709387f5a0efdda2c5c8d5f09541187

# ASP.NET Core version
ENV ASPNET_VERSION=8.0.13

COPY --from=installer ["/dotnet", "/usr/share/dotnet"]
