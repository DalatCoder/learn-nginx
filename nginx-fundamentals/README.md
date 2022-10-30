# NGINX Fundamentals: High-Performance Servers from Scratch

[Learn more](https://www.udemy.com/course/nginx-fundamentals/)

## Overview

Nginx

- Web server
- Reverse proxy server (core)

Nginx vs Apache

Apache:

- Prefork mode: spawns several processes, each can serve a single request at a time such as PHP request, images, videos,...

Nginx:

- Deals with requests asynchronously: single engine process can serve multiple requests concurrently
- Can't embed programming language into its processes
- Dynamic content must be dealt with by a completely separate process like `PHP FPM` and then reverse proxy
  back to the client via `Nginx`
- `Nginx` can serve static resource without `PHP` ever knowing about it.

Nginx is faster than Apache

- Faster static resources
- Higher concurrency

Configuration

- Nginx: URI location first
- Apache: Filesystem location (`.htaccess` file)

## Installation

## Configuration

## Performance

## Security

## Reverse Proxy & Load Balancing
