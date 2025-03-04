ARG REPO=mcr.microsoft.com/dotnet/runtime

# Installer image
FROM amd64/buildpack-deps:noble-curl@sha256:7216a1c4658eecde0bf28e7f6856dbd6669ac76c7d019a380f932f00ae61c8e0 AS installer

# Retrieve ASP.NET Core
RUN aspnetcore_version=9.0.2 \
    && curl -fSL --output aspnetcore.tar.gz https://builds.dotnet.microsoft.com/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-$aspnetcore_version-linux-x64.tar.gz \
    && aspnetcore_sha512='48a39dd4bee3e719273a4e4404b556e2e59e7f659def49f89745ae575fc976f7ebf121d9263c3d009db6081976f8261ca82de34aef69b0776a28fd0485bb6d3b' \
    && echo "$aspnetcore_sha512  aspnetcore.tar.gz" | sha512sum -c - \
    && mkdir -p /dotnet \
    && tar -oxzf aspnetcore.tar.gz -C /dotnet ./shared/Microsoft.AspNetCore.App \
    && rm aspnetcore.tar.gz


# ASP.NET Core image
FROM $REPO:9.0.2-noble-chiseled-extra-amd64@sha256:2d5560ad55285d67b9fed641da232f91eb2c046332ab0d3681e1511199de0342

# ASP.NET Core version
ENV ASPNET_VERSION=9.0.2

COPY --from=installer ["/dotnet", "/usr/share/dotnet"]
