DOCKER_IMAGE_VERSION=0.1.0
DOCKER_IMAGE_NAME=hypriot/rpi-nano-httpd
DOCKER_IMAGE_TAGNAME=$(DOCKER_IMAGE_NAME):$(DOCKER_IMAGE_VERSION)

default: build

build:
	docker build -t $(DOCKER_IMAGE_TAGNAME) .
	docker tag -f $(DOCKER_IMAGE_TAGNAME) $(DOCKER_IMAGE_NAME):latest

minimal:
	docker build -t $(DOCKER_IMAGE_TAGNAME) -f Dockerfile.minimal .
	docker tag -f $(DOCKER_IMAGE_TAGNAME) $(DOCKER_IMAGE_NAME):minimal

push:
	docker push $(DOCKER_IMAGE_NAME)

run:
	docker run -d -p 80:80 -d hypriot/rpi-nano-httpd
