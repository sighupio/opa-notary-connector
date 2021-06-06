FROM golang:1.16.5-alpine AS builder

ARG PROJECT_NAME="opa-notary-connector"
ARG VERSION="dev"
ARG COMMIT="none"
ENV CGO_ENABLED=0
ENV GOPROXY=https://proxy.golang.org

COPY . /src/
WORKDIR /src
RUN go mod vendor
RUN DATE=$(date -u +'%Y-%m-%dT%H:%M:%SZ') go build -ldflags="-s -w -X github.com/sighupio/opa-notary-connector/cmd.version=${VERSION} -X github.com/sighupio/opa-notary-connector/cmd.commit=${COMMIT} -X github.com/sighupio/opa-notary-connector/cmd.date=${DATE}" -mod vendor -o /opa-notary-connector

FROM alpine:3.13

LABEL org.opencontainers.image.name=${PROJECT_NAME}
LABEL org.opencontainers.image.revision=${COMMIT}
LABEL org.opencontainers.image.version=${VERSION}

COPY --from=builder /opa-notary-connector /

RUN mkdir /etc/opa-notary-connector && \
    chown 1001:1001 /etc/opa-notary-connector

USER 1001
CMD ["/opa-notary-connector", "--help"]
