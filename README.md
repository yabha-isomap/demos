# Mock Service


## Quick Start

```bash
# Run service in background
docker run -d --name mockservice_instance --rm -p 8080:3030 yabhaisomap/mockservice:latest
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

## How to customize it

Refer [./mockservice/README.md](./mockservice/README.md)

