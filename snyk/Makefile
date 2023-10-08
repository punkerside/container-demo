#!/bin/bash

name = container-demo
docker_uid = $(shell id -u)
docker_gid = $(shell id -g)
docker_who = $(shell whoami)
export DOCKER_BUILDKIT=0

base:
	@docker build -t ${name}:snyk -f docker/Dockerfile.snyk .
	@docker build -t ${name}:npm -f docker/Dockerfile.npm .

build:
	@echo "${docker_who}:x:${docker_uid}:${docker_gid}::/app:/sbin/nologin" > passwd
	@docker run --rm -u ${docker_uid}:${docker_gid} -v ${PWD}/passwd:/etc/passwd:ro -v ${PWD}/app:/app ${name}:npm

sca:
	@echo "${docker_who}:x:${docker_uid}:${docker_gid}::/app:/sbin/nologin" > passwd
	@docker run --rm -u ${docker_uid}:${docker_gid} -v ${PWD}/passwd:/etc/passwd:ro -v ${PWD}/app:/app ${name}:snyk auth ${SNYK_TOKEN}
	@docker run --rm -u ${docker_uid}:${docker_gid} -v ${PWD}/passwd:/etc/passwd:ro -v ${PWD}/app:/app ${name}:snyk test


# 
# DOCKER_UID=
# DOCKER_GID=
# DOCKER_USER=

# security () {
#     
#     echo "${DOCKER_USER}:x:${DOCKER_UID}:${DOCKER_GID}::/app:/sbin/nologin" > passwd
#     docker run --rm -u ${DOCKER_UID}:${DOCKER_GID} -v ${PWD}/passwd:/etc/passwd:ro -v ${PWD}/docker:/app ${name}:security
# }

# "$@"