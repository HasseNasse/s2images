V = 19.0.0.8
IMAGE_NAME = s2i-openliberty
FROM_JDK8 = hassenasse/java8-centos7:latest
FROM_JDK11 = hassenasse/java11-centos7:latest

.PHONY: build
build-jdk8:
	docker build -t $(IMAGE_NAME)-jdk8 --build-arg FROM_IMAGE=$(FROM_JDK8) .

build-jdk11:
	docker build -t $(IMAGE_NAME)-jdk11 --build-arg FROM_IMAGE=$(FROM_JDK11) .

.PHONY: deploy
deploy-jdk8:
	docker build -t $(IMAGE_NAME)-jdk8 --build-arg FROM_IMAGE=$(FROM_JDK8) .
	docker tag $(IMAGE_NAME)-jdk8:latest hassenasse/$(IMAGE_NAME):$(V)-jdk8
	docker push hassenasse/$(IMAGE_NAME):$(V)-jdk8

deploy-jdk11:
	docker build -t $(IMAGE_NAME)-jdk11 --build-arg FROM_IMAGE=$(FROM_JDK11) .
	docker tag $(IMAGE_NAME)-jdk11:latest hassenasse/$(IMAGE_NAME):$(V)-jdk11
	docker push hassenasse/$(IMAGE_NAME):$(V)-jdk11

deploy:
	make deploy-jdk11
	make deploy-jdk8

.PHONY: test
test:
	docker build -t $(IMAGE_NAME)-candidate .
	IMAGE_NAME=$(IMAGE_NAME)-candidate test/run
