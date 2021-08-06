FROM golang:alpine as builder
ARG LDFLAGS=""

RUN apk --update --no-cache add git build-base gcc

COPY . /build
WORKDIR /build

RUN go build -ldflags "${LDFLAGS}" -o k8s-enrich

FROM netsampler/goflow2:latest

COPY --from=builder /build/k8s-enrich /

ENTRYPOINT ["./goflow2"]
