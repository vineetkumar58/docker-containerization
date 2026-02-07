# Experiment: NGINX Deployment Using Different Base Images

## Objective
Deploy NGINX using different Docker base images and compare image size, layers, and usage.

## Part 1: Official NGINX Image
docker pull nginx:latest
docker run -d --name nginx-official -p 8080:80 nginx
curl http://localhost:8080 ```bash




![ ](Screenshots/Exp4/lab1.png)
