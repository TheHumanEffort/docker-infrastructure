# repo should look like: username/repo

REPO:=__REPLACEME!!__
prepare:
	cp Gemfile* docker/base/
	docker build -t $(REPO):base docker/base
#	docker push $(REPO):base

release:
	rm -f Dockerfile
	ln -s docker/Dockerfile-prod Dockerfile
	docker build -t $(REPO):production .
	rm -f Dockerfile
	ln -s docker/Dockerfile-dev Dockerfile
	docker push $(REPO):production

