ARG REPO=mcr.microsoft.com/dotnet/runtime

# Installer image
FROM amd64/buildpack-deps:jammy-curl@sha256:552e33e44f19a79219858ab0e7789e5345c502bede991fbf435f51640d4c7e2d AS installer

# Retrieve ASP.NET Core
RUN aspnetcore_version=8.0.13 \
    && curl -fSL --output aspnetcore.tar.gz https://builds.dotnet.microsoft.com/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-$aspnetcore_version-linux-x64.tar.gz \
    && aspnetcore_sha512='7b21aff45c3ca7ccdc0527c6de05c209d58a56a15cf10e656522261f884cf272a92be13696b1a0f1ae2baaa0d825ffda58d954871a17b3c3a8659a9f3a36c7e6' \
    && echo "$aspnetcore_sha512  aspnetcore.tar.gz" | sha512sum -c - \
    && mkdir -p /dotnet \
    && tar -oxzf aspnetcore.tar.gz -C /dotnet ./shared/Microsoft.AspNetCore.App \
    && rm aspnetcore.tar.gz


# ASP.NET Core image
FROM $REPO:8.0.13-jammy-chiseled-extra-amd64@sha256:fbe2d562fba26a242c9d205265ae066a90f0995514ae3874f1fd42d75e193aee

# ASP.NET Core version
ENV ASPNET_VERSION=8.0.13

COPY --from=installer ["/dotnet", "/usr/share/dotnet"]
