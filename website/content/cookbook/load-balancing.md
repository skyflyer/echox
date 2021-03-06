+++
title = "Load Balancing Recipe"
description = "Load balancing multiple Echo servers using a reverse proxy server like Nginx, Armor."
[menu.main]
  name = "Load Balancing"
  parent = "cookbook"
+++

This recipe demonstrates how you can use Nginx or Armor as a reverse proxy server and load balance between multiple Echo servers.

## How to setup Nginx proxy server wth Echo?

### Step 1: Install Nginx

https://www.nginx.com/resources/wiki/start/topics/tutorials/install

### Step 2: Configure Nginx

Create a file `/etc/nginx/sites-enabled/localhost` with the following content:

{{< embed "load-balancing/nginx.conf" >}}

> Change listen and server_name per your need.

### Step 3: Restart Nginx

`service nginx restart`

### Step 4: Start upstream servers

- `cd upstream`
- `go run server.go server1 :8081`
- `go run server.go server2 :8082` 

### Step 5: Browse to https://localhost:1323

You should see a webpage being served from "server 1" or "server 2".

```sh
Hello from upstream server server1
```

## How to setup Armor proxy server with Echo?

### Step 1: Install Armor

https://armor.labstack.com/guide

### Step 2: Configure Armor

Create a file `/etc/armor/config.yaml` with the following content:

{{< embed "load-balancing/armor.yaml" >}}

### Step 3: Start Armor

`armor -c /etc/armor/config.yaml`

> Change address and hosts per your need.

### Step 4 & 5: Follow Nginx recipe

## [Source Code]({{< source "load-balancing" >}})

`upstream/server.go`

{{< embed "load-balancing/upstream/server.go" >}}

## Maintainers

- [vishr](https://github.com/vishr)
