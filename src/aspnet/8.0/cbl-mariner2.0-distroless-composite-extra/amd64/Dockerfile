ARG REPO=mcr.microsoft.com/dotnet/runtime-deps

# Installer image
FROM mcr.microsoft.com/cbl-mariner/base/core:2.0@sha256:2a5d3461de4c082b1ced83a491c0d83b80221311dbee1b6f0a98271cefe57b00 AS installer

RUN tdnf install -y \
        ca-certificates \
        gzip \
        tar \
    && tdnf clean all

# Retrieve ASP.NET Composite Runtime
RUN aspnetcore_version=8.0.13  \
    && curl -fSL --output dotnet.tar.gz https://builds.dotnet.microsoft.com/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-composite-$aspnetcore_version-linux-x64.tar.gz \
    && dotnet_sha512='671182c89277327368b739dff04d897cdd82a9f915f0d2d6b2b68eea5952d1d741c4edf413d8e668cc2d9c83b62b01ca86db0587d755904bf02f25d7b7a6e3da' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /usr/share/dotnet \
    && tar -oxzf dotnet.tar.gz -C /usr/share/dotnet \
    && rm dotnet.tar.gz

RUN mkdir /dotnet-symlink \
    && ln -s /usr/share/dotnet/dotnet /dotnet-symlink/dotnet


# ASP.NET Composite Image
FROM $REPO:8.0.13-cbl-mariner2.0-distroless-extra-amd64@sha256:7119e75c041efdf9162b409783771588c17e8acfc7c1399f6838be69d697f26d

ENV \
    # .NET Runtime version
    DOTNET_VERSION=8.0.13 \
    # ASP.NET Core version
    ASPNET_VERSION=8.0.13

COPY --from=installer ["/usr/share/dotnet", "/usr/share/dotnet"]
COPY --from=installer ["/dotnet-symlink", "/usr/bin"]

ENTRYPOINT ["/usr/bin/dotnet"]
CMD ["--info"]
