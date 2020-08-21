[Alpine Linux](../alpine-linux/) | [Docker](../docker/) | [S3CMD](../s3cmd/) | [Go](../go/) | [System](../system/) | [Security](../security/) | [MySQL](../mysql/) | [InfluxDB](../influxdb/) | [Go:Time](time/) | [Dockerize](docker/)


# Why use Go modules

Go modules can be used to:
- version lock your APIs and SDKs that are being used in your project
- ensure that your code compiles at all times (even when one of your imported projects suddenly vanishes or introduces a bug)
- successfully compile your code outside of GOPATH *(To use inside GOPATH set ```GO111MODULES=on``` in your environment variables)*

You get the idea.

How to use Go modules

Please select a project directory that is outside of GOPATH. Say, you have project at the path ```/var/www/example``` and you wish to use go modules inside this project. Also, for the purposes of simplicity let's assume that we just have one single file inside this project called ```example.go``` with the following contents:

```
package main

import (
	"log"
	"net/http"
	"io/ioutil"
	"strings"
	"github.com/tidwall/gjson"
)

func main() {
	rm := "GET"
	url := "https://jsonplaceholder.typicode.com/todos/1"
	payload := ""
	realPayload := strings.NewReader(payload)

	log.Printf("Requesting URL: %s", url)
	log.Printf("Request Method: %s", rm)
	log.Printf("Request Payload: %s", payload)
	req, err := http.NewRequest(rm, url, realPayload)

	req.Header.Add("User-Agent", "HL Cronicle Script/0.0.1")

	res, err := http.DefaultClient.Do(req)
	if err != nil {
		log.Printf("Error sending request. Details: %#v", err)
	} else {
		log.Printf("Result Status: %s", res.Status)
		defer res.Body.Close()
		resp, err := ioutil.ReadAll(res.Body)
		if err != nil {
			log.Printf("Error retrieving response. Details: %#v", err)
		} else {
			userID := gjson.Get(string(resp), "userId")
			log.Printf("User ID: %s", userID.String())
		}
	}
}
```

Just follow the steps below:

1. Go to the project directory

```$ cd /var/www/example```

2. Initialize a new go module

```$ go mod init example```

3. Add the dependencies into ```go.mod``` file. This also creates a ```go.sum``` checksum file.

```$ go get -u ./...```

4. Add these dependencies inside a vendor directory

```$ go mod vendor```


- In order to update a dependency

```$ go get -u <repo_url>```

- And vendor the updates

```$ go mod vendor```

This is what has been learned from this [blog](https://www.kablamo.com.au/blog/2018/12/10/just-tell-me-how-to-use-go-modules)

# Get name of struct field using reflection

See: https://stackoverflow.com/questions/24337145/get-name-of-struct-field-using-reflection
See: https://play.golang.org/p/Al_m3GYl5j

#### [Using Go as a scripting language in Linux](https://dev.to/ignatk/using-go-as-a-scripting-language-in-linux-4c8c?utm_source=dormosheio&utm_campaign=dormosheio)

### [Making HTTP Requests in Golang](https://medium.com/@masnun/making-http-requests-in-golang-dd123379efe7)

### [HTTPS for free in Go, with little help of Let's Encrypt](https://blog.kowalczyk.info/article/Jl3G/https-for-free-in-go-with-little-help-of-lets-encrypt.html)
