FROM alpine:3.12 AS build
RUN apk update && apk add --update alpine-sdk
RUN mkdir /app
WORKDIR /app
COPY . /app
RUN mkdir bin
RUN gcc -Wall hello.c -o bin/hello

FROM alpine:3.12
COPY --from=build /app/bin/hello /app/hello
CMD /app/hello