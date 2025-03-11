FROM ubuntu:22.04
RUN apt update && apt install -y iputils-ping
ENTRYPOINT [ "ping" ]
CMD [ "127.0.0.1" ]