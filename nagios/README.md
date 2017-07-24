###### Copyright (c) 2017 Milton Vincenttis
This is information about Nagios specifically on RHEL 7.3.x, CentOS 7.3.x.

### Introduction to Nagios

### Installation
The exact places of the Nagios configuration files depends on the Linux distribution. As this is for RHEL/centOS, we know the files will be in:


### Configuring


### Sample of monitoring

### Developing NRPE Plugins

### Developing JNRPE Plugins

#### Using de JNRPE Server
* Getting the JNRPE Server:

  The source for JNRPE server is a java maven project and maintained in github. In order to download the source files, just clone it and run mvn clean install in the parent project.

  ```
  $ git clone https://github.com/ziccardi/jnrpe.git

  $ cd jnrpe
  $ mvn clean install
  ```

* Running the JNRPE server

  After finished the build process, we will have a .jar file, that contains everything it is needed to run it. In whatever place create a folder maybe named jnrpe-dev, copy the file under jnrpe-server/target/jnrpe-server-2.1.0-SNAPSHOT-bin.zip to that folder just created, jnrpe-dev.

  Explode the file and change the folder to it.

  ```
  $ mkdir jnrpe-dev; cd jnrpe-dev
  $ cp ../jnrpe-server/target/jnrpe-server-2.1.0-SNAPSHOT-bin.zip .
  $ tar -xvf jnrpe-server-2.1.0-SNAPSHOT-bin.zip
  $ cd jnrpe-server-2.1.0-SNAPSHOT/bin
  ```

  For JNRPE run it needs a file called jnrpe.ini. This file is in jnrpe/jnrpe-install/src/config. Then we do copy it for the current folder.

  ```
  $ cp jnrpe/jnrpe-install/src/config/jnrpe.ini .  
  ```  

  After that the JNRPE server is nice to run. We will run it in interactive mode. Just issue a following command:

  ```
  $ ./jnrpe  --conf ./jnrpe.ini -i
  ```
  JNRPE Server will open a prompt, for interaction.

  ```
  $ JNRPE>
  ```

### Nagios on Docker
#### Installing and Configuring
Nagios in Docker is quite direct.

In the docker hub there are some pre-made images that can be used as the basis for a personal image, or even one that will be used to develop monitoring tests, plugins, etc.

Here is the image used to learn and test Nagios in Docker: https://hub.docker.com/r/quantumobject/docker-nagios/

#--- Pull the image from docker hub
$ docker pull quantumobject/docker-nagios

#--- Runs the image, expoding the port 80
$ docker run -d -p 80:80 quantumobject/docker-nagios

#--- Open this url on browser for check if Nagios web frontend is reachable
http://localhost/Nagios
usr: nagiosadmim
pwd: admin

It is possible change the password. Go inside container and change it.
$ docker exec -it <containerId> bash
