# arvan-container-reverse-proxy

To use the Arvan cloud container, we need to generate a dedicated nginx image.

## Docker

`Dockerfile`
```
# Base image
FROM nginx:latest

# Copy NGINX configuration file
COPY nginx.conf /etc/nginx/nginx.conf

# Expose ports
EXPOSE 80

# Start NGINX
CMD ["nginx", "-g", "daemon off;"]

```
The "events" block specifies the maximum number of connections that can be accepted by the server at the same time. In this case, it is set to 1024.

The "stream" block is used for configuring TCP/UDP stream proxying. It allows NGINX to act as a reverse proxy for TCP/UDP traffic. 

Within the "stream" block, there is a "server" block that listens on all IP addresses (0.0.0.0) and port 80. It is configured to proxy pass incoming traffic to the IP address 192.11.217.217 on port 8080. This means that any requests received on port 80 will be forwarded to the specified IP address and port.

`nginx.conf`

```
events {
    worker_connections 1024;
}

stream {
    server {
        listen 0.0.0.0:80;
        proxy_pass 192.11.217.217:8080;
    }
}
```
Now we will build the Docker image with the following commands and transfer it to Docker Hub.

```
docker build -t mydockerhub/mynginx:v1 .
```
```
docker push mydockerhub/mynginx:v1
```
Now we go to Arvan cloud container and complete the relevant section according to the image.

![Alt text](image.png)

Then continue all the steps with the default settings and there is no need to make any changes.

The point in this method is that there is no need to have a dedicated domain or IP, and we must use the free domain that is provided to us.
## X-UI

Now all the work on Arvan's side is done, we are going to make the connection in the x-ui panel

