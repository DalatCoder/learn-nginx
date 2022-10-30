# NGINX Fundamentals: High-Performance Servers from Scratch

[Learn more](https://www.udemy.com/course/nginx-fundamentals/)

## Overview

Nginx

- Web server
- Reverse proxy server (core)

Nginx vs Apache

Apache:

- Prefork mode: spawns several processes, each can serve a single request at a time such as PHP request, images, videos,...
- Limit by pre-config number of processes (concurrency)

Nginx:

- Deals with requests asynchronously: single engine process can serve multiple requests concurrently
- Can't embed programming language into its processes
- Dynamic content must be dealt with by a completely separate process like `PHP FPM` and then reverse proxy
  back to the client via `Nginx`
- `Nginx` can serve static resource without `PHP` ever knowing about it.
- Limit by system resources (concurrency)

Nginx is faster than Apache

- Faster static resources
- Higher concurrency

Configuration

- Nginx: URI location first
- Apache: Filesystem location (`.htaccess` file)

## Installation

### Prepare ubuntu server

- Via `docker`
- `docker pull ubuntu:18.04`
- `docker run --name ubuntu -p 8000:80 -d ubuntu:18.04`
- `docker exec -it ubuntu /bin/bash`

- Using docker images with `systemd` enable: [https://hub.docker.com/r/jrei/systemd-ubuntu](https://hub.docker.com/r/jrei/systemd-ubuntu)

### Installing via a Package manager

Overview:

- Quick & easy solution
- Limited install options
- No support for additional modules

> Suitable for the most basic web servers or testing & development

Install via `apt`

- `apt-get update`
- `apt-get install nginx`

Check if we successfully install `nginx`

- `ps aux | grep nginx`

### Building Nginx from source & adding modules

- Prepare

  - [nginx.org](nginx.org)
  - [nginx.com](nginx.com)

- Download

  - `wget https://nginx.org/download/nginx-1.23.2.tar.gz`
  - `tar -zxvf nginx-1.23.2.tar.gz`

- Install some dependencies needs to compile `nginx`:

  - Run `./configure` inside folder `nginx-1.23.2.tar.gz`
  - `apt-get install build-essential`
  - Run `./configure`, check missing dependencies and install them
  - `apt-get install libpcre3 libpcre3-dev zlib1g zlib1g-dev libssl-dev`

- Add some `flags`

  - `./configure --help`
  - [Learn more](http://nginx.org/en/docs/configure.html)
  - `./configure --sbin-path=/usr/bin/nginx --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --with-pcre --pid-path=/var/run/nginx.pid --with-http_ssl_module`

- Add some `modules` to extend the standard ngnix functionality

  - pagespeed
  - SSL

  - 2 forms:
    - `bundle modules`
    - `3rd-party modules`

- Compile source: `make`
- Install the compiled source: `make install`
- Check configuration files: `ls -l /etc/nginx`
- Test `nginx` executable command: `nginx -V`
- Start `nginx`: `nginx`
- Check running processes: `ps aux | grep nginx`
- Navigate to `http://127.0.0.1:8000`

- See all bundle modules at: [http://nginx.org/en/docs/](http://nginx.org/en/docs/)@module references

### Adding an Nginx service

- Using `systemd` service

  - [Learn more](https://www.freedesktop.org/wiki/Software/systemd/)
  - `ubuntu 15.04`
  - `centos 7.0`
  - start, stop & restart
  - reload (configuration)
  - start on boot

- Using standard `nginx commandline tool`

  - Check processes: `ps aux | grep nginx`
  - Send stop signal: `nginx -s stop`

- Enable `systemd` service

  - [Script](https://www.nginx.com/resources/wiki/start/topics/examples/systemd/)
  - `touch /lib/systemd/system/nginx.service`
  - Paste the script content

  - ```service
    [Unit]
    Description=The NGINX HTTP and reverse proxy server
    After=syslog.target network-online.target remote-fs.target nss-lookup.target
    Wants=network-online.target

    [Service]
    Type=forking
    PIDFile=/var/run/nginx.pid
    ExecStartPre=/usr/bin/nginx -t
    ExecStart=/usr/bin/nginx
    ExecReload=/usr/bin/nginx -s reload
    ExecStop=/bin/kill -s QUIT $MAINPID
    PrivateTmp=true

    [Install]
    WantedBy=multi-user.target
    ```

- Start: `systemctl start nginx`
- Start `nginx` on `boot`: `systemctl enable nginx`

## Configuration

## Performance

## Security

## Reverse Proxy & Load Balancing
