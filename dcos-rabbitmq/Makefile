OWNER=deepcortex
IMAGE_NAME=dcos-rabbitmq
VCS_REF=`git rev-parse --short HEAD`
IMAGE_VERSION=3.6.9.$(TRAVIS_BUILD_NUMBER)
BUILD_DATE=`date -u +"%Y-%m-%dT%H:%M:%SZ"`
QNAME=$(OWNER)/$(IMAGE_NAME)

GIT_TAG=$(QNAME):$(VCS_REF)
BUILD_TAG=$(QNAME):$(IMAGE_VERSION)
LATEST_TAG=$(QNAME):latest

default: build tag

debug:
	docker run --rm -it $(LATEST_TAG) /bin/sh	

test:	
    docker run --rm -it $(LATEST_TAG) pwd

run:
	docker run --rm -it -v ~/projects:/projects $(LATEST_TAG) /bin/sh
	
lint:
	docker run -it --rm -v "$(PWD)/Dockerfile:/Dockerfile:ro" redcoolbeans/dockerlint

build:
	docker build \
		--build-arg VCS_REF=$(VCS_REF) \
		--build-arg IMAGE_VERSION=$(IMAGE_VERSION) \
		--build-arg BUILD_DATE=$(BUILD_DATE) \
		-t $(GIT_TAG) .
		
tag:
	docker tag $(GIT_TAG) $(BUILD_TAG)
	docker tag $(GIT_TAG) $(LATEST_TAG)

push:
	docker push $(GIT_TAG)
	docker push $(BUILD_TAG)
	docker push $(LATEST_TAG)
	
pre-release: lint build tag test

release: pre-release push
