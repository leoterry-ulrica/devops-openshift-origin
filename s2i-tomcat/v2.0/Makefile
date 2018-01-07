
IMAGE_NAME = s2i-tomcat
VERSION = 1.0

.PHONY: build
build:
	docker build -t $(IMAGE_NAME):$(VERSION) .

.PHONY: test
test:
	docker build -t $(IMAGE_NAME):$(VERSION)-candidate .
	IMAGE_NAME=$(IMAGE_NAME):$(VERSION)-candidate test/run
