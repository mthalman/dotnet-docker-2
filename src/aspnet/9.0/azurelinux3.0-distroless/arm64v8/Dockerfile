ARG REPO=mcr.microsoft.com/dotnet/runtime

# Installer image
FROM mcr.microsoft.com/azurelinux/base/core:3.0@sha256:7caf6bede8a3fa62340581a6e5409ede9564b6866d18143ffd0b691c8f2cfcfc AS installer

RUN tdnf install -y \
        ca-certificates \
        gzip \
        tar \
    && tdnf clean all

# Retrieve ASP.NET Core
RUN aspnetcore_version=9.0.2 \
    && curl -fSL --output aspnetcore.tar.gz https://builds.dotnet.microsoft.com/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-$aspnetcore_version-linux-arm64.tar.gz \
    && aspnetcore_sha512='aa95ed396e5012cb7815db25f07b196261b91e4ca2e7ba07352896e1ab351a96232fdb692fbde1d1ddd1c916987353d2d3382e9e16bd7a97ce4b411c6426e0f6' \
    && echo "$aspnetcore_sha512  aspnetcore.tar.gz" | sha512sum -c - \
    && mkdir -p /dotnet \
    && tar -oxzf aspnetcore.tar.gz -C /dotnet ./shared/Microsoft.AspNetCore.App \
    && rm aspnetcore.tar.gz


# ASP.NET Core image
FROM $REPO:9.0.2-azurelinux3.0-distroless-arm64v8@sha256:0ea9ecfc790856c18e9041a3d90d7d946852f9d20ef1266f3ac4f4be16c241a2

# ASP.NET Core version
ENV ASPNET_VERSION=9.0.2

COPY --from=installer ["/dotnet", "/usr/share/dotnet"]
