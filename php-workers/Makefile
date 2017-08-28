IMG_NAME := bufferapp/php56-worker
IMG_VERSION := 0.1

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
