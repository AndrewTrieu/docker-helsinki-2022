FROM golang:alpine
EXPOSE 8080
WORKDIR /usr/src/app
ENV REQUEST_ORIGIN=http://localhost
COPY . .
RUN go build && \
    adduser -D appuser && \
    chown appuser .
USER appuser
CMD ["./server"]
