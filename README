# php-instantclient-nginx-alpine
PHP7 Container With PDO-OCI8, Nginx and Oracle Instant client 19 Based On Alpine Linux

PHP7/NGINX container with pdo-oci and oci8 extensions that provided for connecting to the Oracle databases using Oracle Instant Client 19.9 and Alpine Linux Docker Image.

## What You Can Do

#### Before Making Docker Image:
 1. put your source files inside `build/src` directory
 
 2. for renaming project root directory inside container you should change it's path from
  - `build/config/nginx.conf` `L:39`
  - `build/Dockerfile` `L:55` `L:57` `L:62` `L:63`
  
 3. for changing nameservers while making image, you can change it from `build/config/resolv.conf`
 
 4. for changing default timezone of php you can change it from `build/config/php.ini`
 
 5. you can make your image with running below command in the root path of cloned repo:
 ```console
 sudo docker-compose build --no-cahce
 ```
 
#### After Making Docker Image:
 1. check that image exists or not with:
  ```console 
  sudo docker images 
  ```
  
 2. if you have image in above list, your image is ready to run
 
 3. you can run it inside cloned repo's root path with:
 ```console
 sudo docker-compose run -d
 ```
 
 4. find your app execution result in `http://localhost:8081`
