#
# Compile:
# docker build --build-arg UNAME=$(id -u -n) --build-arg UID=$(id -u) --build-arg GID=$(id -g) -t asunix/terraform:1.4.2 .
#
FROM golang:bullseye as builder

LABEL maintainer "Giovanni Moroni <asunix@gmail.com>"

ENV PATH /go/bin:/usr/local/go/bin:$PATH
ENV GOPATH /go

ENV TERRAFORM_VERSION v1.4.2

RUN git clone --depth 1 --branch ${TERRAFORM_VERSION} https://github.com/hashicorp/terraform.git /go/src/github.com/hashicorp/terraform

RUN go install github.com/mitchellh/gox@latest

RUN cd /go/src/github.com/hashicorp/terraform \
    && export CGO_ENABLED=0 \
    && export GOFLAGS="-mod=readonly" \
    && go mod download \
    && gox \
    -os="linux" \
    -arch="amd64" \
    -osarch="!darwin/arm !darwin/386" \
    -ldflags "-s -w" \
    -output "pkg/{{.OS}}_{{.Arch}}/terraform" \
    .

FROM debian:11

ARG UNAME=testuser
ARG UID=1000
ARG GID=1000

COPY --from=builder /go/src/github.com/hashicorp/terraform/pkg/linux_amd64/terraform /usr/bin/terraform

RUN groupadd -g $GID -o $UNAME \
    && useradd -m -u $UID -g $GID -o -s /bin/bash $UNAME

USER $UNAME

WORKDIR /work
