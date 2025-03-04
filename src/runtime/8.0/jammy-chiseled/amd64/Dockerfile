ARG REPO=mcr.microsoft.com/dotnet/runtime-deps

# Installer image
FROM amd64/buildpack-deps:jammy-curl@sha256:552e33e44f19a79219858ab0e7789e5345c502bede991fbf435f51640d4c7e2d AS installer

# Retrieve .NET Runtime
RUN dotnet_version=8.0.13 \
    && curl -fSL --output dotnet.tar.gz https://builds.dotnet.microsoft.com/dotnet/Runtime/$dotnet_version/dotnet-runtime-$dotnet_version-linux-x64.tar.gz \
    && dotnet_sha512='8649eda14e8bc993f8cb3d421d44a4abb218acf29996ac1ec939686bf657e75b60f2b690180a4654ee15ebb7b95386eabc969ea98209e31e2801620ff90fce43' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /usr/share/dotnet \
    && tar -oxzf dotnet.tar.gz -C /usr/share/dotnet \
    && rm dotnet.tar.gz

RUN mkdir /dotnet-symlink \
    && ln -s /usr/share/dotnet/dotnet /dotnet-symlink/dotnet


# .NET runtime image
FROM $REPO:8.0.13-jammy-chiseled-amd64@sha256:705134e5c434e9dc317d0fa3d33130756668fe806a395cf4c0876ae951cf9832

# .NET Runtime version
ENV DOTNET_VERSION=8.0.13

COPY --from=installer ["/usr/share/dotnet", "/usr/share/dotnet"]
COPY --from=installer ["/dotnet-symlink", "/usr/bin"]

ENTRYPOINT ["/usr/bin/dotnet"]
CMD ["--info"]
