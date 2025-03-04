ARG REPO=mcr.microsoft.com/dotnet/runtime-deps

# Installer image
FROM amd64/buildpack-deps:bookworm-curl@sha256:d81b6a2595cdfa3826884a0cd74ce97e5398fdbc617720e9b1aa4d94189aa395 AS installer

# Retrieve .NET Runtime
RUN dotnet_version=9.0.2 \
    && curl -fSL --output dotnet.tar.gz https://builds.dotnet.microsoft.com/dotnet/Runtime/$dotnet_version/dotnet-runtime-$dotnet_version-linux-x64.tar.gz \
    && dotnet_sha512='dc4af24d5d298392fd53491a56c6d4e3d1e85e3e294cb4a848c7ac8f0dce287f11dcb274f18f95f7db7195681386e5c6b0d99500808c1b3e68b9c6097a3484d7' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /dotnet \
    && tar -oxzf dotnet.tar.gz -C /dotnet \
    && rm dotnet.tar.gz


# .NET runtime image
FROM $REPO:9.0.2-bookworm-slim-amd64@sha256:3a046c2cd6e3423b6255780f802872ba3381048200c6e8ab8976a983d0242945

# .NET Runtime version
ENV DOTNET_VERSION=9.0.2

COPY --from=installer ["/dotnet", "/usr/share/dotnet"]

RUN ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet
