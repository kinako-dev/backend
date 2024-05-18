FROM golang:1.20

ENV GO111MODULE on
ENV PATH="${PATH}:/go/bin"

RUN apt-get update && \
    apt-get install -y git bash

RUN mkdir /go/src/app
WORKDIR /go/src/app

# Install air
RUN go install github.com/cosmtrek/air@v1.27.3

# Command to run the executable
CMD ["air","-c",".air.toml"]
