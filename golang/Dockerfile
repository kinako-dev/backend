FROM golang:1.20

ENV GO111MODULE on

RUN apt-get update && \
    apt-get install -y git bash

RUN mkdir /go/src/app
WORKDIR /go/src/app

CMD ["go", "run", "main.go"]
