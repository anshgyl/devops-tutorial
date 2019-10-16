#DevOps Toolchain Setup Tutotial
____

##Java (OpenJDK8)
Java is a general-purpose programming language that is class-based, object-oriented and designed to have as few implementation dependencies as possible.

###Installation
```
apt update
apt install openjdk-8-jdk
```

###Configure $JAVA_HOME
openjdk8 is located at /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java

Open /etc/environment
```
nano /etc/environment
```

Add the following line at the end of the /etc/environment
```
JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64/bin/"
```

Now reload this file to apply the changes to your current session:
```
source /etc/environment
```

Verify that the environment variable is set:
```
echo $JAVA_HOME
```

####Ref: [Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-on-ubuntu-18-04)

____

##Git
Git is a distributed version control system, it is a tool to manage your project source code history. Git is one of the most widely-used popular version control system in use today. For example, teams at Amazon and Microsoft have adopted git as their version control system for many of their projects. Git is Open Source and you can create a public or private file system which can be accessible by your project team.

###Installation
```
apt update
apt install git
```
You can confirm that you have installed Git correctly by running the following command:
```
git --version
```

```
Output
git version X.X.X
```

###Configuration
```
git config --global user.name "Your Name"
git config --global user.email "youremail@domain.com"
```

If you want to edit your name or email you can edit the following file
```
nano ~/.gitconfig
```

####Ref: [Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-install-git-on-ubuntu-18-04-quickstart)

____

##Maven
Maven is a build automation tool primarily used for java projects.Build process for a simple peice of code like a "hello world!" would not be complicated but in the case of huge projects there might be several dependencies and plugins required to make an executable.In such cases , a set of instructions to add the plugins and dependencies required must be given during build automation. Maven takes this information from pom.xml file. Therefore to use maven for build automation pom.xml file is required.

###Installation
```
apt install maven
```
You can confirm that you have installed maven correctly by running the following command:
```
mvn -version
```
```
Output
Apache Maven X.X.X
Maven home: /usr/share/maven
Java version: X.X.X_X, vendor: Private Build, runtime: /usr/lib/jvm/java-X-openjdk-amd64/jre
Default locale: en_US, platform encoding: ANSI_X3.4-1968
OS name: "linux", version: "4.9.184-linuxkit", arch: "amd64", family: "unix"
```
####Ref: [Mkyong](https://www.mkyong.com/maven/how-to-install-maven-in-ubuntu/)

____

##Jenkins
Suppose, you have a team of 10 people. Everybody is writing code and wants to test their changes. Someone needs to collect all the new packages and then compile it. But if an error arises its very difficult to figure out which piece of code caused that error.

Jenkins is a continuous integration tool. Jenkins maintains a pipeline that will compile, build and test the code maintaining a context to the user. If found any failure, Jenkins can alert the corresponding contributor.

###Installation

**Add the key**
```
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | apt-key add -
```
When the key is added the system will return a response OK.

**Add the Repository**
```
sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
apt update
apt install jenkins
```

###Starting Jenkins

```
service jenkins start
```

**Check if it is active**
```
service jenkins status
```

Jenkins runs on port 8080 by default, therefore we can open [localhost:8080/](http://localhost:8080/) to use jenkins

###Enable Firewall
If unable to run jenkins then use the following commands
```
sudo ufw allow OpenSSH
sudo ufw enable
```

###Setting up Jenkins

Use the password provided by followind command to unlock jenkins
```
cat /var/lib/jenkins/secrets/initialAdminPassword
```
Choose install suggested plugins, configure user and it's done! 

####Ref: [Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-install-jenkins-on-ubuntu-18-04)

____

##Docker
Docker is a continuous delivery tool. 
In a business enterprise, requirements keep changing and new updates keep waiting for deployment.

Process of deployment has various pre-requisites steps that need to be followed. Code that is contributed by the developer goes to the testing environment and then to the staging environment before reaching to final production.

There are many issues can be faced throughout the stages.

* Testing environment should have the same configuration as a developer machine.
* There should not be any kernel dependency.
* Synchronizing environment across is a redundant task. Applications development, testing, staging and production environment should be same.

**Advantages of Docker**

* Docker abstarcts the application from any kernel dependency using its docker layer.
* Applications environment can be created and maintanined as a script in Dockerfile which can be updated, reviewed and reused.

###Installation

Installing a few prerequisite packages
```
apt install apt-transport-https ca-certificates curl software-properties-common
```
Add the Docker repository to your system
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
apt update
```
Install Docker
```
apt install docker-ce
```
	
Check if docker is successfully installed by seeing if status is active 
service docker status

###Executing Docker commands without sudo
```
usermod -aG docker ${USER}
su - ${USER}
```
Confirm that your user is now added to the docker group by typing:
```
id -nG
```

####Ref: [Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04)

____

##Docker Compose
In a large business enterprise, there are various microservices handling different functionalities of the system. Each microservice should be dockerised and must be able to communicate with each other. Docker-compose is a tool that helps in defining and running multi-container docker applications.</br>

**Advantages over docker**

* Compose lets you define services in a loosely coupled way. 
* As each component of a product is deployed as a separate container, it is straightforward to scale 
 application.

###Installation
```
curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```
You can confirm that you have installed docker-compose correctly by running the following command:
```
docker-compose --version
```
```
Output
docker-compose version 1.21.2, build a133471
```

____

##Rundeck
Rundeck is an open source automation service with a web console, command line tools and a WebAPI. It lets you easily run automation tasks across a set of nodes.

###Installation
```
echo "deb https://rundeckpro.bintray.com/deb stable main" | tee /etc/apt/sources.list.d/rundeck.list
apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 379CE192D401AB61
apt update
apt install rundeckpro-cluster
```

###Starting Rundeck
```
service rundeckd start
```
To verify that the service started correctly, tail the logs
```
tail -f /var/log/rundeck/service.log
```

The service is ready once you see something similar to:
```
Grails application running at http://localhost:4440 in environment: production
```

Logging in for the first time
Navigate to [http://localhost:4440/](http://localhost:4440/) in a browser.
Log in with the ***username: admin*** and ***password: admin***

####Ref: [Rundeck Docs](https://docs.rundeck.com/docs/administration/install/ubuntudebian.html)

____

##Elastic Search
Elasticsearch is a distributed open-source search engine released under Apache license. It is a REST API layer over Apache’s Lucene. It provides horizontal scalability, reliability and capability of a real-time search through the documents. Elasticsearch is able to search faster because it uses indexing to search over documents.

###Installation
**Download**
```
curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.4.0-linux-x86_64.tar.gz
```
**Extract**
```
tar -xvf elasticsearch-7.4.0-linux-x86_64.tar.gz
```
**Run**
```
cd elasticsearch-7.4.0/bin
./elasticsearch
```

____

##Elastic Search
Elasticsearch is a distributed open-source search engine released under Apache license. It is a REST API layer over Apache’s Lucene. It provides horizontal scalability, reliability and capability of a real-time search through the documents. Elasticsearch is able to search faster because it uses indexing to search over documents.

###Installation
**Download**
```
curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.4.0-linux-x86_64.tar.gz
```
**Extract**
```
tar -xvf elasticsearch-7.4.0-linux-x86_64.tar.gz
```
**Run**
```
cd elasticsearch-7.4.0/bin
./elasticsearch
```
**Verify**
[http://localhost:9200](http://localhost:9200)

____

##Logstash
Logstash is an open-source, a server-side tool that is used to collect, parse and then process data centrally into a structured form. It provides various filters that enhance the insights and human readability.

###Installation
**Download**
```
curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-7.4.0.tar.gz
```
**Extract**
```
tar -xvf logstash-7.4.0.tar.gz 
```
**Run**
```
cd  logstash-7.4.0/bin
./logstash -f "<file_path_to_apache.conf>"
```
**Verify**
[http://localhost:9600](http://localhost:9600)

____

##Kibana
Kibana is a tool to visualise indexed data in real-time. It provides a variety of visualisations to get the best meaning out of your data. Dashboards according to use case can be created and get updated with new data automatically.

###Installation
**Download**
```
curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-7.4.0-linux-x86_64.tar.gz
```
**Extract**
```
tar -xvf kibana-7.4.0-linux-x86_64.tar.gz
```
**Run**
```
cd kibana-7.4.0-linux-x86_64/bin
./kibana
```
**Verify**
[http://localhost:5601](http://localhost:5601)