# arvan-container-reverse-proxy

To use the Arvan cloud container, we need to generate a dedicated nginx image.

Dockerfile
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

nginx.conf

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
