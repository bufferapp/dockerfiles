IMG_NAME := bufferapp/php56-worker

.PHONY: build
build:
	docker build -t $(IMG_NAME) -f Dockerfile .

.PHONY: run
run:
	docker run -it $(IMG_NAME) /bin/bash

.PHONY: all
all:
	build
	run






