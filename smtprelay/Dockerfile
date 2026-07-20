FROM golang:1.26-alpine@sha256:f1ddd9fe14fffc091dd98cb4bfa999f32c5fc77d2f2305ea9f0e2595c5437c14 AS build

RUN apk add --no-cache ca-certificates make git patch wget

WORKDIR /go/src/github.com/grafana/smtprelay

# renovate: datasource=github-tags depName=grafana/smtprelay
ARG SMTPRELAY_VERSION=v2.5.3
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

FROM alpine:3.24@sha256:a2d49ea686c2adfe3c992e47dc3b5e7fa6e6b5055609400dc2acaeb241c829f4 AS runtime

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
