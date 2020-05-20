include .env

DOCKER_TEST_IMAGE_TAG ?= test
GOSS_FILE             ?= spec.yml

default: help

.PHONY: build
build: ## [CI] Build the Docker image
	@docker image build . --tag ${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG} \
                          --tag ${DOCKER_IMAGE_NAME}:latest

build-test:
	@docker image build . --tag ${DOCKER_IMAGE_NAME}:${DOCKER_TEST_IMAGE_TAG} \
                          --build-arg DOCKER_IMAGE_NAME=${DOCKER_IMAGE_NAME} \
                          --build-arg DOCKER_IMAGE_TAG=${DOCKER_IMAGE_TAG} \
                          --file Dockerfile.test

.PHONY: test
test: build-test ## [TEST] Run test
	@docker container run --detach --rm --name ${DOCKER_IMAGE_NAME}-test ${DOCKER_IMAGE_NAME}:${DOCKER_TEST_IMAGE_TAG}
	@cat ./test/${GOSS_FILE}
	@docker container exec --interactive --tty --env GOSS_FILE=${GOSS_FILE} ${DOCKER_IMAGE_NAME}-test goss validate
	@docker container stop ${DOCKER_IMAGE_NAME}-test

.PHONY: clean
clean: ## [TEST] Clean-up test environment
	@echo ${DOCKER_IMAGE_NAME}
	@docker image rm ${DOCKER_IMAGE_NAME}:test

.PHONY: help
help:
	@echo "\033[36mUsage:\033[0m\n" >&2
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' Makefile | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "  \033[36mmake %-20s\033[0m %s\n", $$1, $$2}' >&2

