ARG REPO=mcr.microsoft.com/dotnet/runtime

# Installer image
FROM amd64/buildpack-deps:bookworm-curl@sha256:d81b6a2595cdfa3826884a0cd74ce97e5398fdbc617720e9b1aa4d94189aa395 AS installer

# Retrieve ASP.NET Core
RUN aspnetcore_version=9.0.2 \
    && curl -fSL --output aspnetcore.tar.gz https://builds.dotnet.microsoft.com/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-$aspnetcore_version-linux-x64.tar.gz \
    && aspnetcore_sha512='48a39dd4bee3e719273a4e4404b556e2e59e7f659def49f89745ae575fc976f7ebf121d9263c3d009db6081976f8261ca82de34aef69b0776a28fd0485bb6d3b' \
    && echo "$aspnetcore_sha512  aspnetcore.tar.gz" | sha512sum -c - \
    && tar -oxzf aspnetcore.tar.gz ./shared/Microsoft.AspNetCore.App \
    && rm aspnetcore.tar.gz


# ASP.NET Core image
FROM $REPO:9.0.2-bookworm-slim-amd64@sha256:e0ac714c1c99cf2b30e655ebd0f4bd6736b1d960a21b2c061711e43474aa5632

# ASP.NET Core version
ENV ASPNET_VERSION=9.0.2

COPY --from=installer ["/shared/Microsoft.AspNetCore.App", "/usr/share/dotnet/shared/Microsoft.AspNetCore.App"]
