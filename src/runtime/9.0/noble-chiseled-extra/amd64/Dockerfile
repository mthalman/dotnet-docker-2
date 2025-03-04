ARG REPO=mcr.microsoft.com/dotnet/runtime-deps

# Installer image
FROM amd64/buildpack-deps:noble-curl@sha256:7216a1c4658eecde0bf28e7f6856dbd6669ac76c7d019a380f932f00ae61c8e0 AS installer

# Retrieve .NET Runtime
RUN dotnet_version=9.0.2 \
    && curl -fSL --output dotnet.tar.gz https://builds.dotnet.microsoft.com/dotnet/Runtime/$dotnet_version/dotnet-runtime-$dotnet_version-linux-x64.tar.gz \
    && dotnet_sha512='dc4af24d5d298392fd53491a56c6d4e3d1e85e3e294cb4a848c7ac8f0dce287f11dcb274f18f95f7db7195681386e5c6b0d99500808c1b3e68b9c6097a3484d7' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /usr/share/dotnet \
    && tar -oxzf dotnet.tar.gz -C /usr/share/dotnet \
    && rm dotnet.tar.gz

RUN mkdir /dotnet-symlink \
    && ln -s /usr/share/dotnet/dotnet /dotnet-symlink/dotnet


# .NET runtime image
FROM $REPO:9.0.2-noble-chiseled-extra-amd64@sha256:707f124be975d9cbedda09d2ddebb1dfe29f4581fe4adcd2f422e375daaa807e

# .NET Runtime version
ENV DOTNET_VERSION=9.0.2

COPY --from=installer ["/usr/share/dotnet", "/usr/share/dotnet"]
COPY --from=installer ["/dotnet-symlink", "/usr/bin"]

ENTRYPOINT ["/usr/bin/dotnet"]
CMD ["--info"]
