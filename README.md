GO in Docker - simplest possible example!
-----------------------------------------

This app just go to google.com webpage and count bytes on main page. Nothing else!

You can run the program locally of course:

```
go run server.go
```

But we want to create a Docker image, which can be deployed anywhere later. We could build our Go app inside container, but that will add unnecessary size to our image. So let's add one more step - local build - in exchange for super minimalistic image.

Run local build:
```
CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main .
```

Then we have to provide few things.

Dockerfile:
```
# scratch is minimalistic (empty) base image for building Docker images.
FROM scratch

# Because there is SSL communication, you need to provide SSL certificate.
ADD ca-certificates.crt /etc/ssl/certs/

# You need now add main file with compiled Go code with all dependencies.
ADD main /

# And finally you need to run main executable!
CMD ["/main"]
```

Now you can finally build Docker image:

```
docker build -t do-team/gometa .
```

And of course you can simply run it:

```
docker run do-team/gometa
```

That is all!
