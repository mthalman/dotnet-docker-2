ARG REPO=mcr.microsoft.com/dotnet/runtime

# Installer image
FROM arm64v8/buildpack-deps:noble-curl@sha256:f84751aa458fc85130c723b28ca8f9d2d41b251a670788ba92c7c8ffe031c03b AS installer

# Retrieve ASP.NET Core
RUN aspnetcore_version=8.0.13 \
    && curl -fSL --output aspnetcore.tar.gz https://builds.dotnet.microsoft.com/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-$aspnetcore_version-linux-arm64.tar.gz \
    && aspnetcore_sha512='d67130310e81f727f1d4806463f49af18e012d6dc766c940838854922b3a3e7f7171c87d595c4dc09e1c63470fae3017b54d51be98618985129ff37d6a5ac0b3' \
    && echo "$aspnetcore_sha512  aspnetcore.tar.gz" | sha512sum -c - \
    && mkdir -p /dotnet \
    && tar -oxzf aspnetcore.tar.gz -C /dotnet ./shared/Microsoft.AspNetCore.App \
    && rm aspnetcore.tar.gz


# ASP.NET Core image
FROM $REPO:8.0.13-noble-chiseled-extra-arm64v8@sha256:08192a23bc7b92da322bbfb146b9b9646f193c8a4fd8bb3cc4b469ee1b693394

# ASP.NET Core version
ENV ASPNET_VERSION=8.0.13

COPY --from=installer ["/dotnet", "/usr/share/dotnet"]
