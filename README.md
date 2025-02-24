# k-sharing

### check if private registry works

```sh
# pull: will fail
docker pull registry.fit2cloud.org/hello-world

# tag
docker pull hello-world
docker tag hello-world registry.fit2cloud.org/hello-world

# push
docker push  registry.fit2cloud.org/hello-world

# delete
docker rmi registry.fit2cloud.org/hello-world

# pull
docker pull registry.fit2cloud.org/hello-world
```

### trigger docker images to update

```sh
curl -H "Authorization: Bearer Calong@2015" watchtower:8080/v1/update -v
```

### demo app

build registry.fit2cloud.org/demo-app:latest
docker push registry.fit2cloud.org/demo-app:latest

```sh
docker build -t registry.fit2cloud.org/demo-app:latest .
```
