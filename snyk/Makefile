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

sast:
	@echo "${docker_who}:x:${docker_uid}:${docker_gid}::/app:/sbin/nologin" > passwd
	@docker run --rm -u ${docker_uid}:${docker_gid} -v ${PWD}/passwd:/etc/passwd:ro -v ${PWD}/app:/app ${name}:snyk auth ${SNYK_TOKEN}
	@docker run --rm -u ${docker_uid}:${docker_gid} -v ${PWD}/passwd:/etc/passwd:ro -v ${PWD}/app:/app ${name}:snyk code test

container:
	@echo "${docker_who}:x:${docker_uid}:${docker_gid}::/app:/sbin/nologin" > passwd
	@docker run --rm -u ${docker_uid}:${docker_gid} -v ${PWD}/passwd:/etc/passwd:ro -v ${PWD}/app:/app ${name}:snyk auth ${SNYK_TOKEN}
	@docker run --rm -v /var/run/docker.sock:/var/run/docker.sock -v /usr/bin/docker:/usr/bin/docker -v ${PWD}/app:/root ${name}:snyk container test ${name}:npm

clean:
	@rm -rf app/.cache/
	@rm -rf app/.config/
	@rm -rf app/.npm/
	@rm -rf app/node_modules/
	@rm -rf app/.dccache
	@rm -rf app/package-lock.json
	@rm -rf passwd