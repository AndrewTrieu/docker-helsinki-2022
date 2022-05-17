FROM golang:alpine as build-stage
EXPOSE 8080
WORKDIR /usr/src/app
ENV REQUEST_ORIGIN=http://localhost
COPY . .
RUN go build
FROM alpine
WORKDIR /root/
COPY --from=build-stage /usr/src/app/server ./server
RUN adduser -D appuser && \
    chown appuser .
USER appuser
ENTRYPOINT ["./server"]
