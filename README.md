# Dockerfiles

A repo of Dockerfiles for images to be hosted on the 
[Docker registry](https://registry.hub.docker.com/repos/bufferapp/).

## Publishing to Dockerhub workflow

1. Create a new folder for your image with a new `Dockerfile` and any other 
required files
2. Build your image, creating a unique tag w/ the bufferapp namespace:

  `docker build -t bufferapp/apache-php .`

3. After testing it out publish it to Docker Hub:

  `docker push bufferapp/apache-php`
  
4. Add an optional description for the image on Docker Hub
