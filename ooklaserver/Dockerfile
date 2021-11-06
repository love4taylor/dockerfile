FROM debian:stable-slim

ARG GIT_COMMIT=unspecified
LABEL git_commit=$GIT_COMMIT

EXPOSE 5060 5060/udp 8080 8080/udp 

COPY ./OoklaServer /OoklaServer
COPY ./OoklaServer.properties /OoklaServer.properties

ENTRYPOINT ["/OoklaServer"]
