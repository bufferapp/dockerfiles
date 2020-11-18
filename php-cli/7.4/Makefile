IMG_NAME := bufferapp/php-cli
IMG_VERSION := 7.4.12-buster

.PHONY: build
build:
	docker build -t $(IMG_NAME):$(IMG_VERSION) -f Dockerfile .

.PHONY: run
run:
	docker run -it $(IMG_NAME):$(IMG_VERSION) /bin/bash

.PHONY: publish
publish:
	@echo 'publish $(VERSION) to $(IMG_NAME)'
	docker push $(IMG_NAME):$(IMG_VERSION)
