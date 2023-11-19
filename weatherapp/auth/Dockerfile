#build stage
FROM golang:alpine AS builder
RUN apk add git
WORKDIR /go/src/app
RUN ls -latrh
#COPY . .
RUN ls -latrh
RUN git clone https://github.com/DEL-ORG/s5-weather-emile.git
WORKDIR /go/src/app/s5-weather-emile/weatherapp/auth/src/main
RUN ls -latrh
RUN go build -o /go/bin/app -v ./...

#final stage
FROM alpine:latest
RUN apk --no-cache add ca-certificates
COPY --from=builder /go/bin/app /app
ENTRYPOINT /app
LABEL Name=auth Version=0.0.1
EXPOSE 8080
