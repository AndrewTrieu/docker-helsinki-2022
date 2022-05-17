FROM golang:1.16
EXPOSE 8080
WORKDIR /usr/src/app
ENV REQUEST_ORIGIN=http://localhost
COPY . .
RUN go build && \
    go test ./...&& \
    useradd -m appuser && \
    chown appuser .
USER appuser
CMD ["./server"]
