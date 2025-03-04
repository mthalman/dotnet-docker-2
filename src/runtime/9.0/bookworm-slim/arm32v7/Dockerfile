ARG REPO=mcr.microsoft.com/dotnet/runtime-deps

# Installer image
FROM arm32v7/buildpack-deps:bookworm-curl@sha256:c57b7632471adfc5619e43d62c3b5cac4a9e0097677dcac98d96ca7bbc9cfb1f AS installer

# Retrieve .NET Runtime
RUN dotnet_version=9.0.2 \
    && curl -fSL --output dotnet.tar.gz https://builds.dotnet.microsoft.com/dotnet/Runtime/$dotnet_version/dotnet-runtime-$dotnet_version-linux-arm.tar.gz \
    && dotnet_sha512='be31b790c8a5347c1a7fb75ecbbab16c675ff5e6352561631220b94d417f19eed07220d5c3972905123704dd6bd67d00138ff42484815e0def7d8ad916a30d94' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /dotnet \
    && tar -oxzf dotnet.tar.gz -C /dotnet \
    && rm dotnet.tar.gz


# .NET runtime image
FROM $REPO:9.0.2-bookworm-slim-arm32v7@sha256:2bcc38bf64b3458e01612ddc5e3bf55880e26c63ae1521be99563700ca191d67

# .NET Runtime version
ENV DOTNET_VERSION=9.0.2

COPY --from=installer ["/dotnet", "/usr/share/dotnet"]

RUN ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet
