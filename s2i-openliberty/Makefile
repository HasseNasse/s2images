VERSION = 19.0.0.8
IMAGE_NAME = s2i-openliberty
FROM_JDK8 = hassenasse/openliberty:$(VERSION)-jdk8
FROM_JDK11 = hassenasse/openliberty:$(VERSION)-jdk11

.PHONY: build
build-jdk8:
	docker build -t $(IMAGE_NAME)-jdk8 \
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
	docker build -t $(IMAGE_NAME)-jdk8 \
	--build-arg FROM_IMAGE=$(FROM_JDK8) \
	--build-arg VERSION=$(VERSION) .
	
	docker tag $(IMAGE_NAME)-jdk8:latest hassenasse/$(IMAGE_NAME):$(VERSION)-jdk8
	docker push hassenasse/$(IMAGE_NAME):$(VERSION)-jdk8

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
