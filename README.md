https://gitlab.com/nanuchi/techworld-js-docker-demo-app

<img width="1121" height="606" alt="Image" src="https://github.com/user-attachments/assets/2462fd18-7fe5-4a42-aa04-22bf1407274c" />


## demo app - developing with Docker

This demo app shows a simple user profile app set up using 
- index.html with pure js and css styles
- nodejs backend with express module
- mongodb for data storage

All components are docker-based

Basic commands


Docker basic commands keep them handy

    Docker --version

    docker login

    docker search <imagename>
    
    docker images
    
    docker images ls

    docker create imagename

    docker ps

    docker ps -a --> shows images in all the states    

    docker start contnr_ID

    docker stop contnr_ID

    docker create --name image_name --> costum name

    docker stop image_name

    docker run --name image_name image --> container is created, downloaded, started at the same time.

    docker run -d --name image_name image --> "-d" is detached mode

    docker kill contnr_name --> shutsdown the container forcefully






### With Docker

#### To start the application

Step 1: Create docker network

    docker network create mongo-network 

Step 2: start mongodb 

    docker run -d -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password --name mongodb --net mongo-network mongo    

Step 3: start mongo-express
    
    docker run -d -p 8081:8081 -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=password --net mongo-network --name mongo-express -e ME_CONFIG_MONGODB_SERVER=mongodb mongo-express   

_NOTE: creating docker-network in optional. You can start both containers in a default network. In this case, just emit `--net` flag in `docker run` command_

Step 4: open mongo-express from browser

    http://localhost:8081

Step 5: create `user-account` _db_ and `users` _collection_ in mongo-express

Step 6: Start your nodejs application locally - go to `app` directory of project 

    npm install 
    node server.js
    
Step 7: Access you nodejs application UI from browser

    http://localhost:3000

### With Docker Compose

#### To start the application

Step 1: start mongodb and mongo-express

    docker-compose -f docker-compose.yaml up
    
_You can access the mongo-express under localhost:8080 from your browser_
    
Step 2: in mongo-express UI - create a new database "my-db"

Step 3: in mongo-express UI - create a new collection "users" in the database "my-db"       
    
Step 4: start node server 

    npm install
    node server.js
    
Step 5: access the nodejs application from browser 

    http://localhost:3000

#### To build a docker image from the application

    docker build -t my-app:1.0 .       
    
The dot "." at the end of the command denotes location of the Dockerfile.


1. Named Volumes (Best for Production/Databases)Docker completely manages the storage location on your hard drive.CLI Example (MongoDB)PowerShell# Create a volume
docker volume create mongo-data

# Run container attached to the volume
docker run -d --name db -v mongo-data:/data/db mongo
Docker Compose ExampleYAMLservices:
  mongo:
    image: mongo
    volumes:
      - mongo-data:/data/db

volumes:
  mongo-data:
2. Bind Mounts (Best for Live Local Development)Maps a specific, exact folder on your computer directly into the container.CLI Example (Node.js)PowerShelldocker run -d -p 3000:3000 -v "C:\my-app:/usr/src/app" node:24-slim
Docker Compose ExampleYAMLservices:
  node-app:
    image: node:24-slim
    volumes:
      - "./app:/usr/src/app" # 🔄 Live syncs your local folder
3. Essential Volume CommandsActionCommandList all volumesdocker volume lsInspect a volume (Find path)docker volume inspect <volume_name>Delete a specific volumedocker volume rm <volume_name>Wipe all unused volumesdocker volume prune

Using Floci 

aws s3 mb s3://my-data-bucket -- run this to check if teh connection is success 

aws ecr create-repository --repository-name my-app                                          

docker tag my-app:1.0 localhost:4566/my-app:1.0

docker push localhost:4566/my-app:1.0


 