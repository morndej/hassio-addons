ARG GOLANG_VERSION=1.15
ARG ALPINE_VERSION=3.13
ARG BUILD_FROM=alpine:3.13

FROM golang:${GOLANG_VERSION}-alpine${ALPINE_VERSION} as gobuild
ARG GOLANG_VERSION
ARG ALPINE_VERSION

RUN echo -e "http://nl.alpinelinux.org/alpine/v3.13/main\nhttp://nl.alpinelinux.org/alpine/v3.13/community" > /etc/apk/repositories
RUN apk add --no-cache git gcc build-base alpine-sdk; \
  go get -v github.com/cloudflare/cloudflared/cmd/cloudflared

WORKDIR /go/src/github.com/cloudflare/cloudflared/cmd/cloudflared

RUN go build ./

FROM $BUILD_FROM

ARG GOLANG_VERSION
ARG ALPINE_VERSION

RUN apk add --no-cache ca-certificates bind-tools libcap; \
  rm -rf /var/cache/apk/*; \
  mkdir /etc/cloudflared;

COPY --from=gobuild /go/src/github.com/cloudflare/cloudflared/cmd/cloudflared/cloudflared /usr/local/bin/cloudflared


COPY run.sh /
RUN chmod a+x /run.sh

CMD [ "/run.sh" ]
