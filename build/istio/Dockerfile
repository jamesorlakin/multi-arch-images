FROM golang:1.13-alpine as build

ARG TARGETPLATFORM

ENV GO111MODULE=on \
    CGO_ENABLED=0 \
    STATIC=0 

RUN apk add --no-cache git make bash

WORKDIR /go/src/github.com/istio/istio

RUN export GOOS=$(echo ${TARGETPLATFORM} | cut -d / -f1) && \
    export GOARCH=$(echo ${TARGETPLATFORM} | cut -d / -f2) && \
    GOARM=$(echo ${TARGETPLATFORM} | cut -d / -f3); export GOARM=${GOARM:1} && \
    git clone --depth 1 ${VERSION} https://github.com/xunholy/istio.git . && \
    TARGET_ARCH=${GOARCH} make docker
