FROM golang:1.12.1-alpine as builder

RUN apk update && apk upgrade && \
    apk add --no-cache bash git openssh

RUN git clone git@gitlab.com:dr-code/payment-repository.git

WORKDIR payment-repository

RUN go build main.go

FROM alpine:3.9

WORKDIR /app

RUN apk add --no-cache ca-certificates

COPY --from=builder /go/bin/payment-repository /app/app
COPY --from=builder /go/src/payment-repository/migrations /app/migrations

CMD ["/app/app"]

EXPOSE 9111