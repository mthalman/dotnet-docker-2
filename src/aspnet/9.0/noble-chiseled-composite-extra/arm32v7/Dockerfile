ARG REPO=mcr.microsoft.com/dotnet/runtime-deps

# Installer image
FROM arm32v7/buildpack-deps:jammy-curl@sha256:d44b9c225547b3b2ef90485327c66aeecd10469b9181c23ec31f3452ed41eb06 AS installer

# Retrieve ASP.NET Composite Runtime
RUN aspnetcore_version=9.0.2  \
    && curl -fSL --output dotnet.tar.gz https://builds.dotnet.microsoft.com/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-composite-$aspnetcore_version-linux-arm.tar.gz \
    && dotnet_sha512='9008fec9c3cc98b9608eb6635f73cb86d37cb721d6f4ba3dbb8eaee4d7adacf832cbef6c223320895656743a82b5a81209c78936973ba469e551dbf05bc40ce9' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /usr/share/dotnet \
    && tar -oxzf dotnet.tar.gz -C /usr/share/dotnet \
    && rm dotnet.tar.gz

RUN mkdir /dotnet-symlink \
    && ln -s /usr/share/dotnet/dotnet /dotnet-symlink/dotnet


# ASP.NET Composite Image
FROM $REPO:9.0.2-noble-chiseled-extra-arm32v7@sha256:7b4bd92e839056e3e03317581c535678e0cd6393fde621146e6e1f6b6c1e7383

ENV \
    # .NET Runtime version
    DOTNET_VERSION=9.0.2 \
    # ASP.NET Core version
    ASPNET_VERSION=9.0.2

COPY --from=installer ["/usr/share/dotnet", "/usr/share/dotnet"]
COPY --from=installer ["/dotnet-symlink", "/usr/bin"]

ENTRYPOINT ["/usr/bin/dotnet"]
CMD ["--info"]
