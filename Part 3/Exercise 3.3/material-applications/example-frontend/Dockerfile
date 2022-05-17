FROM ubuntu:18.04
EXPOSE 5000
WORKDIR /usr/src/app
ENV REACT_APP_BACKEND_URL=http://localhost:8080/ping 
COPY . .
RUN apt-get update && apt-get install -y curl 
RUN curl -sL https://deb.nodesource.com/setup_16.x | bash
RUN apt-get install -y nodejs && apt-get purge -y --auto-remove curl 
RUN npm install 
RUN npm run build 
RUN npm install -g serve 
RUN useradd -m appuser 
RUN chown appuser .
USER appuser
CMD ["serve", "-s", "-l", "5000","build"]
