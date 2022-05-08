FROM ubuntu:20.04
WORKDIR /usr/src/app
COPY search.sh .
RUN chmod +x search.sh
RUN apt update && apt install -y curl
CMD ./search.sh