ARG REPO=mcr.microsoft.com/dotnet/runtime

# Installer image
FROM arm64v8/buildpack-deps:bookworm-curl@sha256:a1be32243a395f7d75930f6dbff84188d8823f38d87ee68e92eb696cb4b6f5ed AS installer

# Retrieve ASP.NET Core
RUN aspnetcore_version=9.0.2 \
    && curl -fSL --output aspnetcore.tar.gz https://builds.dotnet.microsoft.com/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-$aspnetcore_version-linux-arm64.tar.gz \
    && aspnetcore_sha512='aa95ed396e5012cb7815db25f07b196261b91e4ca2e7ba07352896e1ab351a96232fdb692fbde1d1ddd1c916987353d2d3382e9e16bd7a97ce4b411c6426e0f6' \
    && echo "$aspnetcore_sha512  aspnetcore.tar.gz" | sha512sum -c - \
    && tar -oxzf aspnetcore.tar.gz ./shared/Microsoft.AspNetCore.App \
    && rm aspnetcore.tar.gz


# ASP.NET Core image
FROM $REPO:9.0.2-bookworm-slim-arm64v8@sha256:4f424e663bfc295b029b1eb9ddae19f2e4d4f823add24b39bcb22c0be612845f

# ASP.NET Core version
ENV ASPNET_VERSION=9.0.2

COPY --from=installer ["/shared/Microsoft.AspNetCore.App", "/usr/share/dotnet/shared/Microsoft.AspNetCore.App"]
