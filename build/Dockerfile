FROM golang:1.12-alpine3.9

RUN apk add --no-cache \
      curl \
      git \
      make \
      gcc \
      musl-dev \
      && true

ENV CGO_ENABLED=0

ENV DEP_VERSION v0.5.0

# Install dep to avoid vendoring all the things
RUN curl --silent --location "https://github.com/golang/dep/releases/download/${DEP_VERSION}/dep-linux-amd64" --output /usr/local/bin/dep \
    && chmod +x /usr/local/bin/dep

ENV EKSCTL_BUILD $GOPATH/src/github.com/weaveworks/eksctl/build
RUN mkdir -p "$(dirname ${EKSCTL_BUILD})"
COPY . $EKSCTL_BUILD

WORKDIR $EKSCTL_BUILD

RUN dep ensure

RUN ./install.sh

WORKDIR $EKSCTL_BUILD/vendor/github.com/goreleaser/goreleaser
RUN go build -tags netgo && go install -tags netgo

WORKDIR $GOPATH
