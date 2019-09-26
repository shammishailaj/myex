This page explains how to generate smallest possible docker images for your go code should you choose to dockerize your container. Theoretically, this applies to all compiled languages that can compile into dependency-free self-executing binaries but, we are specifically targeting **Go** here.

1. Build the binary for your OS and architecture. A list of all architectures and OS(es) supported by the go compiler is available [here](https://gist.github.com/shammishailaj/fac95b0eb5c2293fd41224bb2c6502bb). The build command will look something like:

```
CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -a -installsuffix cgo -o ../path/to/executable .
```
Here, were have specifically chosen to build for 64-bit linux machines.

2. Now change your directory to ```../path/to/executable```

```
cd ../path/to
```

3. Create a ```Dockerfile``` with the following contents

```
FROM scratch
ADD executable /usr/local/bin/
ENV PORT 1234
EXPOSE 1234
ENTRYPOINT ["executable"]
```

You may add more instructions in the Dockerfile as per your requirements. This one is the bare-minimum.

4. Build the docker image

```
docker build -t my_image:my_version .
```

5. Check the size of your docker image

```
docker images
```

You will see that the size of this image is way smaller than the ones that do not build ```FROM scratch```.

---
Original idea from: https://blog.kloia.com/micro-docker-images-for-go-applications-8a8701130c01