all: build

TAG?=v0.0.1
REGISTRY?=ytaka23
FLAGS=
ENVVAR=

deps:
	go get github.com/tools/godep

build: clean deps
	$(ENVVAR) GOOS=linux GOARCH=arm GOARM=7 godep go build ./...
	$(ENVVAR) GOOS=linux GOARCH=arm GOARM=7 godep go build -o updater

test-unit: clean deps build
	$(ENVVAR) godep go test --test.short -race ./... $(FLAGS)

docker:
ifndef REGISTRY
	ERR = $(error REGISTRY is undefined)
	$(ERR)
endif
	docker build --pull -t ${REGISTRY}/vpa-updater-armv7:${TAG} .
	docker push ${REGISTRY}/vpa-updater-armv7:${TAG}

release: build docker

clean:
	rm -f updater

format:
	test -z "$$(find . -path ./vendor -prune -type f -o -name '*.go' -exec gofmt -s -d {} + | tee /dev/stderr)" || \
	test -z "$$(find . -path ./vendor -prune -type f -o -name '*.go' -exec gofmt -s -w {} + | tee /dev/stderr)"

.PHONY: all deps build test-unit clean format release
