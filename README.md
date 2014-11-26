# Docker image for liferay 6.2
This is a fork from snasello/docker-liferay-6.2, created and maintained by snasello. This version is narrowed in functionality in order to illustrate a very simple 2-tier dockerized application.

The image is build in docker registry : https://registry.hub.docker.com/u/m451/docker-liferay-6.2
you can pull it :
```
docker pull m451/docker-liferay-6.2
```

## Start image
you can start it directly, it will use the hsqldb (not for production!)
```
docker run --rm -t -i -p 8080:8080 m451/docker-liferay-6.2 
```
When you have the message "INFO: Server startup in xxx ms" you can open a browser and go to http://localhost:8080

### Linking with Mysql
First start the mysql image
```
docker run --name lep-mysql -e MYSQL_ROOT_PASSWORD=r00tpassw0rd -e MYSQL_USER=lportal -e MYSQL_PASSWORD=p455w0rd -e MYSQL_DATABASE=liferaydb -d mysql:5.6
```
This will create a new docker container called **lep-mysql** based on mysql 5.6, create a database called **liferaydb** as well as a user called **lportal** with the password **p455w0rd** and the root database password called **r00tpassw0rd**.
See https://registry.hub.docker.com/_/mysql/ for details on how to use the mysql dockerfile.

Then start the liferay image with a link to the database
```
docker run --rm -it --name lep-portal -p 8080:8080 --link lep-mysql:db_lr m451/docker-liferay-6.2
```

This will make the container **lep-mysql** accessable from the container **lep-portal** via the environment variable **db_lr**. The dockerfile of the image m451/liferay-6.2:6.2-ce-ga2 is configured to use the properties within the **/config** folder of this repository. This will configure Liferay to use **jdbc:mysql://db_lr/liferaydb** as database with the username and password we just used when we created the MySql container.
