ARG REPO=mcr.microsoft.com/dotnet/runtime-deps

# Installer image
FROM mcr.microsoft.com/azurelinux/base/core:3.0@sha256:7caf6bede8a3fa62340581a6e5409ede9564b6866d18143ffd0b691c8f2cfcfc AS installer

RUN tdnf install -y \
        ca-certificates \
        gzip \
        tar \
    && tdnf clean all

# Retrieve .NET Runtime
RUN dotnet_version=9.0.2 \
    && curl -fSL --output dotnet.tar.gz https://builds.dotnet.microsoft.com/dotnet/Runtime/$dotnet_version/dotnet-runtime-$dotnet_version-linux-arm64.tar.gz \
    && dotnet_sha512='460133ddc2582a209bd80673721e7b9add2b9b2967ef1503a7dc29be2777870870990ee7549351e8207c3e3e84dabeca6d5bdbbdea75e5e3e749eb16bd13e7ef' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /usr/share/dotnet \
    && tar -oxzf dotnet.tar.gz -C /usr/share/dotnet \
    && rm dotnet.tar.gz

RUN mkdir /dotnet-symlink \
    && ln -s /usr/share/dotnet/dotnet /dotnet-symlink/dotnet


# .NET runtime image
FROM $REPO:9.0.2-azurelinux3.0-distroless-extra-arm64v8@sha256:ba40483d1b1f81f5ba240e949623aa3f4b53f8c8afa0ffae2a26ba2808e4c6be

# .NET Runtime version
ENV DOTNET_VERSION=9.0.2

COPY --from=installer ["/usr/share/dotnet", "/usr/share/dotnet"]
COPY --from=installer ["/dotnet-symlink", "/usr/bin"]

ENTRYPOINT ["/usr/bin/dotnet"]
CMD ["--info"]
