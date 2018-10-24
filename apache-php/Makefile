IMG_NAME := bufferapp/apache-php
IMG_VERSION := 5.6.36-apache-stretch-igbinary-mongodb-1.5.3

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
