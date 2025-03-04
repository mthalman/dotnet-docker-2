ARG REPO=mcr.microsoft.com/dotnet/runtime-deps

# Installer image
FROM amd64/buildpack-deps:noble-curl@sha256:7216a1c4658eecde0bf28e7f6856dbd6669ac76c7d019a380f932f00ae61c8e0 AS installer

# Retrieve ASP.NET Composite Runtime
RUN aspnetcore_version=9.0.2  \
    && curl -fSL --output dotnet.tar.gz https://builds.dotnet.microsoft.com/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-composite-$aspnetcore_version-linux-x64.tar.gz \
    && dotnet_sha512='b819c4421c1110ecadfa4f1e790efb14d9788e7456be016d26023b993dda40c47f22e356483c5c6f00d0941e0daea8cf2c5fe6aa405c7422ec911a46b81b552c' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /usr/share/dotnet \
    && tar -oxzf dotnet.tar.gz -C /usr/share/dotnet \
    && rm dotnet.tar.gz

RUN mkdir /dotnet-symlink \
    && ln -s /usr/share/dotnet/dotnet /dotnet-symlink/dotnet


# ASP.NET Composite Image
FROM $REPO:9.0.2-noble-chiseled-amd64@sha256:b193469e633bf799262d3a81cdf0c6205c9bf00e5953ca86d5cbd90fd6f7c94d

ENV \
    # .NET Runtime version
    DOTNET_VERSION=9.0.2 \
    # ASP.NET Core version
    ASPNET_VERSION=9.0.2

COPY --from=installer ["/usr/share/dotnet", "/usr/share/dotnet"]
COPY --from=installer ["/dotnet-symlink", "/usr/bin"]

ENTRYPOINT ["/usr/bin/dotnet"]
CMD ["--info"]
