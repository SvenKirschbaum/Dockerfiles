FROM golang:1.27-alpine@sha256:b8c9b82affbbb82a020bbcf998dad2ba9516944ac7e883524642a481a9505e9c AS build

RUN apk add --no-cache ca-certificates make git patch wget

WORKDIR /go/src/github.com/grafana/smtprelay

# renovate: datasource=github-tags depName=grafana/smtprelay
ARG SMTPRELAY_VERSION=v2.5.4
ARG TARGETOS
ARG TARGETARCH
ARG TARGETVARIANT
ENV GOOS=${TARGETOS} GOARCH=${TARGETARCH} GOARM=${TARGETVARIANT}

ENV CGO_ENABLED=0

RUN wget -O /tmp/smtprelay.tar.gz "https://github.com/grafana/smtprelay/archive/refs/tags/${SMTPRELAY_VERSION}.tar.gz" \
    && tar -xzf /tmp/smtprelay.tar.gz --strip-components=1 \
    && rm /tmp/smtprelay.tar.gz

RUN go mod download -x

ARG GO_LDFLAGS

COPY remote-helo.patch ./
RUN patch -p1 < remote-helo.patch

RUN make build
# sanity check - make sure the binary runs and is executable
RUN bin/smtprelay --version

FROM alpine:3.24@sha256:be2656061b536c1883c4034790e21d0a09c5aaaf51968b89368a3c0371ea2c9a AS runtime

RUN apk --no-cache upgrade zlib

COPY --from=build /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt
COPY --from=build /go/src/github.com/grafana/smtprelay/bin/smtprelay /usr/local/bin/smtprelay

ARG GIT_REVISION

LABEL org.opencontainers.image.revision=$GIT_REVISION \
        org.opencontainers.image.vendor="Grafana Labs" \
        org.opencontainers.image.title="smtprelay" \
        org.opencontainers.image.source="https://github.com/grafana/smtprelay"

# users need to mount config file at /usr/local/smtprelay.ini
ENTRYPOINT ["/usr/local/bin/smtprelay"]
CMD ["--help"]
