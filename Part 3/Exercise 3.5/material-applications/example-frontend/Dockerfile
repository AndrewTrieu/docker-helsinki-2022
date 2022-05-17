FROM alpine
EXPOSE 5000
WORKDIR /usr/src/app
ENV REACT_APP_BACKEND_URL=http://localhost:8080/ping 
COPY . .
RUN apk update && apk add --no-cache curl npm bash nodejs && \
    npm install && \
    npm run build && \
    npm install -g serve && \
    adduser -D appuser && \
    chown appuser .
USER appuser
CMD ["serve", "-s", "-l", "5000","build"]
