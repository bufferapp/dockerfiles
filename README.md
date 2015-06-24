# Dockerfiles

A repo of Dockerfiles for images to be hosted on the 
[Docker registry](https://registry.hub.docker.com/repos/bufferapp/).

## Publishing to Dockerhub workflow

1. Create a new folder for your image with a new `Dockerfile` and any other 
required files
2. Build your image, creating a unique tag w/ the bufferapp namespace. This 
is telling docker to build the Dockerfile found in your current directory (`.`).

  ```bash
  $ docker build -t bufferapp/apache-php .
  # ...docker creates your image...
  $ docker images
  REPOSITORY              TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
  bufferapp/apache-php    latest              94edd5d07de6        30 seconds ago      508.4 MB
  ```

3. After testing it out publish it to Docker Hub:

  ```
  $ docker push bufferapp/apache-php
  ```
  
4. Add an optional description for the image on Docker Hub
