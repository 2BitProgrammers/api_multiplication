# 2bitprogrammers/api_multiplication

This is a simple API example which multiplies two values (a and b).  It is meant to be used for instructional purposes only.

The API listens on port:  1234

**<u>API Enpoints</u>**:
* **/status** (GET) - this states whether the app is up and healthy
* **/multiply** (POST) - this returns the results of multiplying the provided variables. This requires a json body to be included with the request.
  * <u>Example JSON Body:</u>   _{ "a": 11, "b": 22 }_

## Run as Standalone GoLang App
This will run the application with the go application.  It assumes that you have installed your goLang environment correctly.

```bash
$ cd src
$ go run main.go

2bitprogrammers/api_multiplication v2007.21a
www.2BitProgrammers.com
Copyright (C) 2020. All Rights Reserved.

2020/07/10 11:10:55 Starting App on Port 1234

CTRL+C
```

## Run with Docker
This will run the components on your local system without using minikube or kubernetes.

### Building the Docker Image
```bash
$ docker build . -t 2bitprogrammers/api_multiplication

Sending build context to Docker daemon  13.31kB
Step 1/11 : FROM golang:alpine AS builder
 ---> b3bc898ad092
Step 2/11 : ENV GO111MODULE=on     CGO_ENABLED=0     GOOS=linux     GOARCH=amd64
 ---> Using cache
 ---> 8462443c0070
Step 3/11 : WORKDIR /build
 ---> Using cache
 ---> 99600623930c
Step 4/11 : COPY $PWD/src/go.mod .
 ---> Using cache
 ---> 3a3d381aa5a4
Step 5/11 : COPY $PWD/src/main.go .
 ---> Using cache
 ---> c33942efc96c
Step 6/11 : RUN go mod download
 ---> Using cache
 ---> a3ededc97e06
Step 7/11 : RUN go build -o api_multiplication .
 ---> Using cache
 ---> 9dda28002911
Step 8/11 : FROM scratch
 --->
Step 9/11 : WORKDIR /
 ---> Using cache
 ---> a66c59ea194a
Step 10/11 : COPY --from=builder /build/api_multiplication .
 ---> 95d5f920eee8
Step 11/11 : ENTRYPOINT [ "/api_multiplication" ]
 ---> Running in 4d5285094cd8
Removing intermediate container 4d5285094cd8
 ---> d769fe8f0434
Successfully built d769fe8f0434
Successfully tagged 2bitprogrammers/api_multiplication:latest
SECURITY WARNING: You are building a Docker image from Windows against a non-Windows Docker host. All files and directories added to build context will have '-rwxr-xr-x' permissions. It is recommended to double check and reset permissions for sensitive files and directories.
```

### Image Status
```bash
$ docker images

REPOSITORY                                    TAG                                              IMAGE ID            CREATED             SIZE
2bit/k8s_exp_microservices_bemultiplication   latest                                           157907ed1817        47 seconds ago      6.67MB
```

### Running the Container
```bash
$ docker run --rm --name "api_multiplication" -p 1234:1234 2bitprogrammers/api_multiplication

2bitprogrammers/api_multiplication v2007.21a
www.2BitProgrammers.com
Copyright (C) 2020. All Rights Reserved.

2020/07/10 11:10:55 Starting App on Port 1234

CTRL+C
```

### Check the Container Status
```bash
$ docker ps

CONTAINER ID     IMAGE                                COMMAND                 CREATED              STATUS              PORTS                    NAMES
79b974f60450     2bitprogrammers/api_multiplication   "/api_multiplication"   About a minute ago   Up About a minute   0.0.0.0:1234->1234/tcp   api_multiplication
```

### Check API Status (health check):
```bash
$ curl http://localhost:1234/status

{"date":"2020-07-10T03:17:02.9034438-07:00","statusCode":200,"statusText":"OK","data":"{ \"healthy\": true}","errors":"","request":{"uri":"/status","method":"GET","payload":""}}
```

### Watch Container Logs
```bash
$ docker logs -f beMultiplication

2020/07/10 11:07:22 Starting App on Port 1234
2bit_exp_k8s_microservices_beMultiplication v2007.21a
www.2BitProgrammers.com
Copyright (C) 2020. All Rights Reserved.

2020/07/10 11:08:22 POST /multiply 200 { "a": 2, "b": 3 }

CTRL+C
```

### Stopping the Container
```bash
$ docker stop beMultiplication
```

## Using the API
For the below examples, we will assume the following:
* Server:  locahost (127.0.0.1)
* Bind Port: 1234
* Method: POST
* URI: /multiply
* Body Data:   _{ "a": 2, "b": 3 }_

For Linux:
```bash
$ curl -X POST -H "Content-Type: application/json" -d '{ "a": 2, "b": 3 }' http://127.0.0.1:1234/multiply

{"date":"2020-07-10T03:17:02.9034438-07:00","statusCode":200,"statusText":"OK","data":"{ \"value\": 6 }","errors":"","request":{"uri":"/multiply","method":"POST","payload":"{\"a\": 2, \"b\": 3 }"}}
```

For Windows (powershell):
```powershell
C:\> curl -X POST -H "Content-Type: application/json" -d "{ """a""": 2, """b""": 3 }" http://127.0.0.1:1234/multiply

{"date":"2020-07-10T03:17:02.9034438-07:00","statusCode":200,"statusText":"OK","data":"{ \"value\": 6 }","errors":"","request":{"uri":"/multiply","method":"POST","payload":"{\"a\": 2, \"b\": 3 }"}}
```

