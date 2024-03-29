FROM golang:1.12.1-alpine as builder

RUN apk update && apk upgrade && \
    apk add --no-cache bash git openssh

ADD ./ssh_key ssh_key

RUN mkdir -p ~/.ssh && \
    cp ./ssh_key/* ~/.ssh && \
    chmod 0600 -R ~/.ssh && \
    echo "StrictHostKeyChecking no " > ~/.ssh/config && \
    echo -e "[url \"git@github.com:\"]\n\tinsteadOf = https://github.com/" > ~/.gitconfig

RUN git clone git@gitlab.com:dr-code/user-repository.git

WORKDIR user-repository

RUN go build -o main

FROM alpine:3.9

WORKDIR /app

RUN apk add --no-cache ca-certificates

COPY --from=builder /go/user-repository/main /app/app
COPY --from=builder /go/user-repository/migrations /app/migrations

CMD ["/app/app"]