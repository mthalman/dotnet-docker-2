ARG REPO=mcr.microsoft.com/dotnet/runtime-deps

# Installer image
FROM arm32v7/buildpack-deps:jammy-curl@sha256:d44b9c225547b3b2ef90485327c66aeecd10469b9181c23ec31f3452ed41eb06 AS installer

# Retrieve .NET Runtime
RUN dotnet_version=9.0.2 \
    && curl -fSL --output dotnet.tar.gz https://builds.dotnet.microsoft.com/dotnet/Runtime/$dotnet_version/dotnet-runtime-$dotnet_version-linux-arm.tar.gz \
    && dotnet_sha512='be31b790c8a5347c1a7fb75ecbbab16c675ff5e6352561631220b94d417f19eed07220d5c3972905123704dd6bd67d00138ff42484815e0def7d8ad916a30d94' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /dotnet \
    && tar -oxzf dotnet.tar.gz -C /dotnet \
    && rm dotnet.tar.gz


# .NET runtime image
FROM $REPO:9.0.2-noble-arm32v7@sha256:8833ca9f0a205c06474ca955ff565a77641df637d79c7dc6bbdc178472bdb0c0

# .NET Runtime version
ENV DOTNET_VERSION=9.0.2

COPY --from=installer ["/dotnet", "/usr/share/dotnet"]

RUN ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet
