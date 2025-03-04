ARG REPO=mcr.microsoft.com/dotnet/runtime

# Installer image
FROM amd64/buildpack-deps:noble-curl@sha256:7216a1c4658eecde0bf28e7f6856dbd6669ac76c7d019a380f932f00ae61c8e0 AS installer

# Retrieve ASP.NET Core
RUN aspnetcore_version=8.0.13 \
    && curl -fSL --output aspnetcore.tar.gz https://builds.dotnet.microsoft.com/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-$aspnetcore_version-linux-x64.tar.gz \
    && aspnetcore_sha512='7b21aff45c3ca7ccdc0527c6de05c209d58a56a15cf10e656522261f884cf272a92be13696b1a0f1ae2baaa0d825ffda58d954871a17b3c3a8659a9f3a36c7e6' \
    && echo "$aspnetcore_sha512  aspnetcore.tar.gz" | sha512sum -c - \
    && tar -oxzf aspnetcore.tar.gz ./shared/Microsoft.AspNetCore.App \
    && rm aspnetcore.tar.gz


# ASP.NET Core image
FROM $REPO:8.0.13-noble-amd64@sha256:abc48968f615ac1080c1252890511a1222b50b386de4ab8c2315a3b683dd48f4

# ASP.NET Core version
ENV ASPNET_VERSION=8.0.13

COPY --from=installer ["/shared/Microsoft.AspNetCore.App", "/usr/share/dotnet/shared/Microsoft.AspNetCore.App"]
