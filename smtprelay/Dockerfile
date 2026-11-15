FROM golang:1.27-alpine@sha256:eb61c3873c97416fff0116553fff65d67a3549f72e11d9758849a689cdd8b523 AS build

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

FROM alpine:3.24@sha256:f59abe656b692d901e184be3445e084786602108599d2a51d229c0f5a48c71bf AS runtime

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
