FROM --platform=$BUILDPLATFORM golang:1.24-alpine AS build
RUN apk --no-cache add \
  bash

WORKDIR /src
COPY . .

ARG TARGETOS 
ARG TARGETARCH 
ARG VERSION
RUN GOOS=$TARGETOS GOARCH=$TARGETARCH VERSION=$VERSION ./scripts/build.sh

#FROM alpine:latest
FROM reg.navercorp.com/base/alpine:3.21
LABEL maintainer="n3r.co <dl_n3r_co@navercorp.com>"
LABEL com.navercorp.image.author="kr17709"

USER root:root

RUN apk --no-cache add \
  ca-certificates
COPY --from=build --chown=irteam:irteam --chmod=0755 /src/bin/redis-operator /usr/local/bin

USER irteam:irteam

ENTRYPOINT ["/usr/local/bin/redis-operator"]
