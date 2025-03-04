FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/nightly/sdk:9.0-noble-aot@sha256:6db08aea7e94ed442f4f5defc744878b45c4b4509e1e3e63a406810c91241a90 AS build
ARG TARGETARCH
WORKDIR /source

# Copy project file and restore as distinct layers
COPY --link *.csproj NuGet.config .
RUN dotnet restore -r linux-$TARGETARCH

# Copy source code and publish app
COPY --link . .
RUN dotnet publish -r linux-$TARGETARCH --no-restore -o /app releasesapi.csproj
RUN rm /app/*.dbg /app/*.Development.json


# Final stage/image
FROM mcr.microsoft.com/dotnet/nightly/runtime-deps:9.0-noble-chiseled-aot@sha256:716f5a59534a1a93edced745be5b2ba78f5a3a29caabf510cae923166dd2496c
WORKDIR /app
COPY --link --from=build /app .
USER $APP_UID
ENTRYPOINT ["./releasesapi"]
