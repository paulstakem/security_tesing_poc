APP_NAME=dummy-container
BUILD_NUM?="unknown"
IMAGE = quay.io/$(REGISTRY)/$(APP_NAME):$(VERSION)
REGISTRY_PASSWORD?="unknown"
REGISTRY=acme
VERSION=1.1.$(BUILD_NUM)

##----------------------------------------------------------------------------##

build:
	docker build \
	--build-arg git_repository=`git config --get remote.origin.url` \
	--build-arg git_branch=`git rev-parse --abbrev-ref HEAD` \
	--build-arg git_commit=`git rev-parse HEAD` \
	--build-arg built_on=`date -u +"%Y-%m-%dT%H:%M:%SZ"` \
	-t $(IMAGE) .

scan/container:
	trivy image --severity HIGH,CRITICAL --ignore-unfixed --ignore-policy example-policy.rego quay.io/acme/dummy-container:1.1.unknown

scan/dockerfile:
	trivy config ./

##----------------------------------------------------------------------------##
# Registry and

upstream/get:
	cat list.txt | xargs -n1 docker pull

upstream/scan-trivy:
	cat list.txt | xargs -n1 trivy image --severity HIGH,CRITICAL --ignore-unfixed --ignore-policy example-policy.rego

upstream/scan-docker:
	cat list.txt | xargs -n1 docker scan --severity high
