## Pengertian :
- registry :
- images : 
- container :

## Docker Command List :
### 1) Images :
1. `docker images`
    - see all images available
2. `docker [image_name]`
    - download image with tag latest from registry
3. `docker [image_name]:specified_version_tag`
    - download image with specific version from registry
4. `docker image rm [image_id] or [image_name]`    
### 2) Container
1. `docker container ls`
    - see all running container
2. `docker container ls -all`
    - see all container
3. `docker container create [image_name] or [image_name]:specified_version_tag`
    - create container with random name
4. `docker container create --name [container_name] [image_name]`
    - create container with specific name
5. `docker container start [container_name]`
    - start container
6. `docker container stop [container_name]`
    - stop container
7. `docker container rm [container_name]`
    - delete container        
8. `docker container create --name [container_name] -p 8080:20717 [image_name]`
    - export port
    - 8080 -> port for outside container to get 20717
    - 20717 -> port inside container
### 3) Create Own Image Docker
- use Dockerfile
- exp: go application named main.go
    1. make Dockerfile without extension:
        ``` dockerfile
        FROM golang:1.11.4  
        #download golang from registry      
        
        COPY main.go /app/main.go 
        #copy main.go(our apss) into /app/main.go located in image

        CMD ["go", "run", "/app/main.go"]
        #tell the image how to run main.go
        ```
    2. Build golang image from Dockerfile: `docker build --tag app-golang:1.0 .`
### 4) Upload Image to Docker Hub :
1. Make repository
2. `~$ docker login` 
3. push image:
    ``` dockerfile
    ~$ docker tag local-image:tagname reponame:tagname
    # docker tag app-golang:1.0 hammam/app-golang:1.0
    ~S docker push reponame:tagname
    # docker push hammam/app-golang:1.0
    ```

        

