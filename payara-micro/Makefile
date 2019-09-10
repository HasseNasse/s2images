VERSION = 5.193
IMAGE_NAME = payara-micro
FROM_JDK8 = hassenasse/java8-centos7:latest
FROM_JDK11 = hassenasse/java11-centos7:latest

.PHONY: build
build-jdk8:
	docker build -t $(IMAGE_NAME)-jdk1.8 \
	--build-arg FROM_IMAGE=$(FROM_JDK8) \
	--build-arg VERSION=$(VERSION) .

build-jdk11:
	docker build -t $(IMAGE_NAME)-jdk11 \
	--build-arg FROM_IMAGE=$(FROM_JDK11) \
	--build-arg VERSION=$(VERSION) .

build:
	make build-jdk8
	make build-jdk11

.PHONY: deploy
deploy-jdk8:
	docker build -t $(IMAGE_NAME)-jdk1.8 \
	--build-arg FROM_IMAGE=$(FROM_JDK8) \
	--build-arg VERSION=$(VERSION) .
	
	docker tag $(IMAGE_NAME)-jdk1.8:latest hassenasse/$(IMAGE_NAME):$(VERSION)-jdk1.8
	docker push hassenasse/$(IMAGE_NAME):$(VERSION)-jdk1.8

deploy-jdk11:
	docker build -t $(IMAGE_NAME)-jdk11 \
	--build-arg FROM_IMAGE=$(FROM_JDK11) \
	--build-arg VERSION=$(VERSION) .

	docker tag $(IMAGE_NAME)-jdk11:latest hassenasse/$(IMAGE_NAME):$(VERSION)-jdk11
	docker push hassenasse/$(IMAGE_NAME):$(VERSION)-jdk11

deploy:
	make deploy-jdk11
	make deploy-jdk8

.PHONY: test
test:
	docker build -t $(IMAGE_NAME)-candidate \
	--build-arg FROM_IMAGE=$(FROM_JDK11) \
	--build-arg VERSION=$(VERSION) .

	IMAGE_NAME=$(IMAGE_NAME)-candidate test/run
