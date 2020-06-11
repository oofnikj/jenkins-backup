# jenkins/pipeline/docker/git/Dockerfile
FROM alpine:3.11
RUN apk update && apk add git openssh
RUN adduser -D -u1000 jenkins
RUN ssh-keyscan github.com > /etc/ssh/ssh_known_hosts
USER jenkins
