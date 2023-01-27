# Mock Service

## Prerequisites

1. Create github personal access token to push images to [github container registry](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry).
2. Save your personal access token in a file `~/secrets/ghub_pat.sh`. Give the file executable permissions. `chmod +x ~/secrets/ghub_pat.sh`

    ```bash
    $ cat ~/secrets/ghub_pat.sh
    # Output
    export CR_PAT='<your personal access token>'
    ```

## Build and test locally

```bash

# Clone this repo and go to mockservice folder.
cd mockservice
# Build the docker image and test locally.
docker build -t mockservice:latest .
# Run service in background
docker run -d --name mockservice_instance --rm -p 8080:3030 mockservice:latest
# Test it
$ curl localhost:8080/v1/users
# Output
[{"id":1,"name":"John"},{"id":2,"name":"Marry"}]
$ curl localhost:8080/v1/cars
# Output
[{"id":1,"name":"Mercedes"},{"id":2,"name":"Lexus"}]
$ curl localhost:8080/v1/books
# Output
[{"id":1,"name":"Angels"},{"id":2,"name":"Daemons"}]

# Remove container
$ docker stop mockservice_instance

```

## Push image to Github Container Registry

1. Here replace `yabha-isomap` with your github user-name

### Login to github container registry

```bash
# Login to github container registry using your personal access token
# Replace yabha-isomap with your github user-name
$ echo $CR_PAT | docker login ghcr.io -u yabha-isomap --password-stdin
# Output
WARNING! Your password will be stored unencrypted in ~/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
```  

### Push image to container registry

```bash
# tag image to registry and push it
docker tag mockservice:latest ghcr.io/yabha-isomap/mockservice:latest
docker push ghcr.io/yabha-isomap/mockservice:latest
```

### Test image from github container registry

```bash
# Logout to make sure we are testing as unauthenticated user
docker logout ghcr.io
# Run service in background
docker run -d --name mockservice_instance --rm -p 8080:3030 ghcr.io/yabha-isomap/mockservice:latest
# Test it
$ curl localhost:8080/v1/users
# Output
[{"id":1,"name":"John"},{"id":2,"name":"Marry"}]
$ curl localhost:8080/v1/cars
# Output
[{"id":1,"name":"Mercedes"},{"id":2,"name":"Lexus"}]
$ curl localhost:8080/v1/books
# Output
[{"id":1,"name":"Angels"},{"id":2,"name":"Daemons"}]

# Remove container
$ docker stop mockservice_instance
```

## Use this image in kubernetes

Now you can use this image in kubernetes