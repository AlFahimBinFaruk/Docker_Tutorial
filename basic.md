## How to run app and db on same network.
* Both of DB and APP and have to be in same network in-order for them to connect.  

### Using CLI.
```
docker network create my-mongo-network
```

```
docker run --name my-mongo -p 4000:27017 -e MONGO_INITDB_ROOT_USERNAME=mongo -e MONGO_INITDB_ROOT_PASSWORD=password --network my-mongo-network -d mongo 
```

```
docker run --name my-exp-app --network my-mongo-network -e ME_CONFIG_MONGODB_SERVER=my-mongo -e ME_CONFIG_MONGODB_ADMINUSERNAME=mongo 
-e ME_CONFIG_MONGODB_ADMINPASSWORD=password -p 8081:8081 -d mongo-express
```


### Using Docker compose.
* By default all the services that run in docker compose shares the same network.

```yaml
version: "3"
services:
  mongodb:
    image: mongo
    ports:
      - "27017:27017"
    environment:
      - MONGO_INITDB_ROOT_USERNAME=mongo
      - MONGO_INITDB_ROOT_PASSWORD=password
    volumes:
      - my-mongo-data:/data/db  
  mongo-express:
    image: mongo-express
    restart: always
    ports:
      - "8080:8081"
    environment:
      - ME_CONFIG_MONGODB_SERVER=mongodb
      - ME_CONFIG_MONGODB_ADMINUSERNAME=mongo
      - ME_CONFIG_MONGODB_ADMINPASSWORD=password
volumes:
  my-mongo-data:
    driver: local   
```