# Artifactory Registry

![artifactory](https://tutos-android-france.com/wp-content/uploads/2017/05/Artifactory_HEX1.png)

## prerequisite

* Docker: [https://get.docker.com](https://get.docker.com/)
* Trial licence of artifactory pro: [https://jfrog.com/artifactory/free-trial](https://jfrog.com/artifactory/free-trial/)

### Installation of artifactory

To install artifactory, two methods are available to you:

1. Stack AWS
   if you are using AWS, you just need to:
* Clone the repository: [https://github.com/franck-art/artifactory](https://github.com/franck-art/artifactory)

* load the artifactory-stack.yml file into AWS through the cloud formation service
2. With docker Container

just follow the following procedure or follow the file artifactory-pro in my github repositories [https://github.com/franck-art/artifactory/blob/master/artifactory-pro](https://github.com/franck-art/artifactory/blob/master/artifactory-pro)

  mkdir -p $JFROG_HOME/artifactory/var/etc/

  cd $JFROG_HOME/artifactory/var/etc/

  touch ./system.yaml

  chown -R 1030:1030 $JFROG_HOME/artifactory/var

  docker run --name artifactory -v $JFROG_HOME/artifactory/var/:/var/opt/jfrog/artifactory -d -p 8081:8081 -p 8082:8082 docker.bintray.io/jfrog/artifactory-pro:latest

#### home page Artifactory

To access the artifactory home page, type the url in your browser:
[http://@ip:8082](http://@ip:8082)

* NB: check that port 8082 is open

##### Usage with HTTP Request

Two methods are used to communicate with the registry: HTTP and HTTPS
In our case, we used the HTTP method because we did not configure a certificate.

On the client machine, the one that will login,push and pull:

* Edit/create the daemon.json file locate in /etc/docker/daemon.json and add these lines
  
  {
    "insecure-registries" : ["@ip-public_registry:8082"]
  }

* restart the docker service: **sudo service docker restart**

###### Docker tag, login, push, pull

after the *insecure-registries* mode has been activated, we can push our docker images

* docker login -u admin -p password @ip_registry:8082 

* docker tag IMAGE_TAG_ID  @ip_registry:8082/Repository_key/Images:version

* docker push/pull @ip_registry:8082/Repository_key/Images:Version

NB: The **Repository_key** is configured in the home page of artifactory at: configuration -> repositories

