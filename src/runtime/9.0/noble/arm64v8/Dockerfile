ARG REPO=mcr.microsoft.com/dotnet/runtime-deps

# Installer image
FROM arm64v8/buildpack-deps:noble-curl@sha256:f84751aa458fc85130c723b28ca8f9d2d41b251a670788ba92c7c8ffe031c03b AS installer

# Retrieve .NET Runtime
RUN dotnet_version=9.0.2 \
    && curl -fSL --output dotnet.tar.gz https://builds.dotnet.microsoft.com/dotnet/Runtime/$dotnet_version/dotnet-runtime-$dotnet_version-linux-arm64.tar.gz \
    && dotnet_sha512='460133ddc2582a209bd80673721e7b9add2b9b2967ef1503a7dc29be2777870870990ee7549351e8207c3e3e84dabeca6d5bdbbdea75e5e3e749eb16bd13e7ef' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /dotnet \
    && tar -oxzf dotnet.tar.gz -C /dotnet \
    && rm dotnet.tar.gz


# .NET runtime image
FROM $REPO:9.0.2-noble-arm64v8@sha256:a5d3818ddee888d2bb0549b46d5766694a5ac8b1d418a4a923573ae26a868687

# .NET Runtime version
ENV DOTNET_VERSION=9.0.2

COPY --from=installer ["/dotnet", "/usr/share/dotnet"]

RUN ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet
