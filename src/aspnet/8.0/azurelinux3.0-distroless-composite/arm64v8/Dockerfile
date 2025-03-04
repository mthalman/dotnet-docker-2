ARG REPO=mcr.microsoft.com/dotnet/runtime-deps

# Installer image
FROM mcr.microsoft.com/azurelinux/base/core:3.0@sha256:7caf6bede8a3fa62340581a6e5409ede9564b6866d18143ffd0b691c8f2cfcfc AS installer

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
FROM $REPO:8.0.13-azurelinux3.0-distroless-arm64v8@sha256:29659f341ca411584624bc83bba5cb01c39fc222bb62774450ef9274b8ef673d

ENV \
    # .NET Runtime version
    DOTNET_VERSION=8.0.13 \
    # ASP.NET Core version
    ASPNET_VERSION=8.0.13

COPY --from=installer ["/usr/share/dotnet", "/usr/share/dotnet"]
COPY --from=installer ["/dotnet-symlink", "/usr/bin"]

ENTRYPOINT ["/usr/bin/dotnet"]
CMD ["--info"]
