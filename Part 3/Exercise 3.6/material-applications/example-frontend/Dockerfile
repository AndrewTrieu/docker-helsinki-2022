FROM node:alpine as build-stage
EXPOSE 5000
ENV REACT_APP_BACKEND_URL=http://localhost:8080/ping 
WORKDIR /usr/src/app
COPY . .
RUN npm install && \
    npm run build
FROM alpine
WORKDIR /root/
COPY --from=build-stage /usr/src/app/build ./build
RUN apk add --update nodejs npm && \
    npm install -g serve && \
    adduser -D appuser && \
    chown appuser build
USER appuser
ENTRYPOINT ["npx", "serve", "-s", "-l", "5000", "build"] 
