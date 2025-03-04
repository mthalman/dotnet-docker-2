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
    && curl -fSL --output dotnet.tar.gz https://builds.dotnet.microsoft.com/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-composite-$aspnetcore_version-linux-arm64.tar.gz \
    && dotnet_sha512='942ff8c727eae0f0caca541706cab338ed16f6b14470c626ec2c54ad877189d85425761d8529d700172633aaf45c2dae5e29e262ce0339da20e6d00fbb914591' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /usr/share/dotnet \
    && tar -oxzf dotnet.tar.gz -C /usr/share/dotnet \
    && rm dotnet.tar.gz

RUN mkdir /dotnet-symlink \
    && ln -s /usr/share/dotnet/dotnet /dotnet-symlink/dotnet


# ASP.NET Composite Image
FROM $REPO:8.0.13-cbl-mariner2.0-distroless-extra-arm64v8@sha256:6a810f78a43ac918d4f3f45a570cbe8109d1f718f8e09534a9ca8f0899cdbba7

ENV \
    # .NET Runtime version
    DOTNET_VERSION=8.0.13 \
    # ASP.NET Core version
    ASPNET_VERSION=8.0.13

COPY --from=installer ["/usr/share/dotnet", "/usr/share/dotnet"]
COPY --from=installer ["/dotnet-symlink", "/usr/bin"]

ENTRYPOINT ["/usr/bin/dotnet"]
CMD ["--info"]
