+++
title = "Reverse Proxy"
description = "Using Echo as a reverse proxy server"
[menu.main]
  name = "Reverse Proxy"
  parent = "cookbook"
+++

## How to use Echo as a reverse proxy server?

This recipe demonstrates how you can use Echo as a reverse proxy server and load balancer in front of your favorite applications like WordPress, Node.js, Java, Python, Ruby or even Go. For simplicity, I will use Go upstream servers with WebSocket.

### Step 1: Identify upstream target URLs

```go
url1, err := url.Parse("http://localhost:8081")
if err != nil {
  e.Logger.Fatal(err)
}
url2, err := url.Parse("http://localhost:8082")
if err != nil {
  e.Logger.Fatal(err)
}
```

### Step 2: Setup proxy middleware with upstream targets

In the following code snippet we are using round-robin load balancing technique. You may also use `middleware.RandomBalancer`.

```go
e.Use(middleware.Proxy(middleware.ProxyConfig{
  Balancer: &middleware.RoundRobinBalancer{
    Targets: []*middleware.ProxyTarget{
      &middleware.ProxyTarget{
        URL: url1,
      },
      &middleware.ProxyTarget{
        URL: url2,
      },
    },
  },
}))
```

### Step 3: Start upstream servers

- `cd upstream`
- `go run server.go server1 :8081`
- `go run server.go server2 :8082`

### Step 3: Start the proxy server

```sh
go run server.go
```

### Step 4: Browse to https://localhost:1323

You should see a webpage with HTTP request being served from "server 1" and WebSocket request from "server 2".

```sh
HTTP

Hello from upstream server server1

WebSocket

Hello from upstream server server2!
Hello from upstream server server2!
Hello from upstream server server2!
```

## [Source Code]({{< source "reverse-proxy" >}})

`upstream/server.go`

{{< embed "reverse-proxy/upstream/server.go" >}}

`server.go`

{{< embed "reverse-proxy/server.go" >}}

## Maintainers

- [vishr](https://github.com/vishr)