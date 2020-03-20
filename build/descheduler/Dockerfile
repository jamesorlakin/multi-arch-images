FROM golang:1.13-alpine as build

ARG VERSION=v0.10.0
ARG TARGETPLATFORM

ENV GO111MODULE=on \
    CGO_ENABLED=0

RUN apk add --no-cache git

WORKDIR /go/src/github.com/kubernetes-sigs/descheduler

RUN export GOOS=$(echo ${TARGETPLATFORM} | cut -d / -f1) && \
    export GOARCH=$(echo ${TARGETPLATFORM} | cut -d / -f2) && \
    GOARM=$(echo ${TARGETPLATFORM} | cut -d / -f3); export GOARM=${GOARM:1} && \
    git clone --depth 1 -b ${VERSION} https://github.com/kubernetes-sigs/descheduler.git . && \
    go build -o descheduler ./cmd/descheduler

# Use distroless as minimal base image to package the manager binary
# Refer to https://github.com/GoogleContainerTools/distroless for more details
FROM gcr.io/distroless/static:nonroot

COPY --from=build /go/src/github.com/kubernetes-sigs/descheduler/descheduler /usr/local/bin/descheduler

USER nonroot:nonroot

CMD ["/bin/descheduler", "--help"]
