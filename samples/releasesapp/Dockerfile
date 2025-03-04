# Learn about building .NET container images:
# https://github.com/dotnet/dotnet-docker/blob/main/samples/README.md
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:9.0@sha256:1025bed126a7b85c56b960215ab42a99db97a319a72b5d902383ebf6c6e62bbe AS build
ARG TARGETARCH
WORKDIR /source

# Copy project file and restore as distinct layers
COPY --link *.csproj .
RUN dotnet restore -a $TARGETARCH

# Copy source code and publish app
COPY --link . .
RUN dotnet publish -a $TARGETARCH --self-contained --no-restore -o /app


# Final stage
# Since the app is self-contained, there's no dependency on the .NET Runtime.
FROM mcr.microsoft.com/dotnet/runtime-deps:9.0@sha256:8affebec88628680a6fd905d093491cd5932138becf24621ff84367717d9e335
WORKDIR /app
COPY --link --from=build /app .
USER $APP_UID
ENTRYPOINT ["./releasesapp"]
