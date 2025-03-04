FROM amd64/alpine:3.20@sha256:5e4725f15afdfb14b32e8d80b8d6b7f924b60b43864362a13850b10aaadc1e2a

ENV \
    # UID of the non-root user 'app'
    APP_UID=1654 \
    # Configure web servers to bind to port 8080 when present
    ASPNETCORE_HTTP_PORTS=8080 \
    # Enable detection of running in a container
    DOTNET_RUNNING_IN_CONTAINER=true

RUN apk add --upgrade --no-cache \
        ca-certificates-bundle \
        \
        # .NET dependencies
        icu-data-full \
        icu-libs \
        libgcc \
        libssl3 \
        libstdc++ \
        tzdata \
        zlib

# Create a non-root user and group
RUN addgroup \
        --gid=$APP_UID \
        app \
    && adduser \
        --uid=$APP_UID \
        --ingroup=app \
        --disabled-password \
        app
