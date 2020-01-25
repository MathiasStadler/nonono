# nonono

## source

```txt
# rest angular 8
https://www.djamware.com/post/5d8d7fc10daa6c77eed3b2f2/angular-8-tutorial-rest-api-and-httpclient-examples


# kafka + angular
https://dev.to/victorgil/kafka-websockets-angular-event-driven-microservices-all-the-way-to-the-frontend-12aa



```

## install vbguest on buster

```bash
# on buster box
sudo apt-get update # This will update the repositories list
sudo apt-get upgrade # This will update all the necessary packages on your system
sudo apt-get dist-upgrade # This will add/remove any needed packages
reboot # You may need this since sometimes after a upgrade/dist-upgrade, there are some left over entries that get fixed after a reboot
sudo apt-get install linux-headers-$(uname -r) # This should work now
exit
```

```bash
#on host
vagrant reload
```

```bash
# on buster box
sudo apt-get install -y linux-headers-$(uname -r) &&
sudo apt-get install -y dkms

```

- install VBoxGuestExtension

```bash
cd 
rm VBoxGuestAdditions_5.2.34.iso
wget http://download.virtualbox.org/virtualbox/5.2.34/VBoxGuestAdditions_5.2.34.iso
mkdir -p /media/VBoxGuestAdditions
mount -o loop,ro VBoxGuestAdditions_4.2.34.iso /media/VBoxGuestAdditions
sh /media/VBoxGuestAdditions/VBoxLinuxAdditions.run
# rm VBoxGuestAdditions_5.2.34.iso
umount /media/VBoxGuestAdditions
rmdir /media/VBoxGuestAdditions
exit
exit # exit out of ssh
```

```bash
# on host
vagrant reload
vagrant ssh
# to check modules are loaded
lsmod |grep vbox
```

## install docker

- follow this instruction

```txt
https://github.com/MathiasStadler/debian-stretch-install-docker
```

## clone frontend

```bash
mkdir ~/playground && cd $_
git clone https://github.com/VictorGil/transfers_frontend.git
```

## git clone node

```bash
git clone https://github.com/nodejs/docker-node.git
```

## build node container

```bash
sudo apt-get install -y curl
cd docker-node/10/buster
sudo docker build -t node .
```

## run npm out of container

- [from here](https://gist.github.com/ArtemGordinsky/b79ea473e8bc6f67943b)

```bash
docker run -v "$PWD":/usr/src/app -w /usr/src/app node sh -c 'npm install'
```

## run npm install of transfer-frontend

```bash
cd
cd playground/transfer_frontend.git
docker run -v "$PWD":/usr/src/app -w /usr/src/app node sh -c 'npm install'
```

## run npm audit fix

```bash
docker run -v "$PWD":/usr/src/app -w /usr/src/app node sh -c 'npm audit fix '
```

## npm audit fix transfer-frontend

```bash
docker run -v "$PWD":/usr/src/app -w /usr/src/app node sh -c 'npm audit build '
```

## npm start frontend

```bash
sudo docker run -p 0.0.0.0:4200:4200 -v "$PWD":/usr/src/app -w /usr/src/app node sh -c 'npm start '
```

@TODO
- hint: Add --host 0.0.0.0 to ng run  inside the package.json to reach network wide


## prepare Reusing the Maven local repository
docker volume create --name maven-repo

## git clone web service

```bash
cd
cd playground
git clone https://github.com/VictorGil/transfers_websockets_service.git
```

## set maven docker container

```bash
DOCKER_MAVEN_PROJECT_CONTAINER="maven:3.6.3-jdk-13"
echo "DOCKER_MAVEN_PROJECT_CONTAINER ${DOCKER_MAVEN_PROJECT_CONTAINER}"
```

## git clone transfer-api

```bash
cd
cd playground
sudo rm -rf transfers_api
git clone https://github.com/VictorGil/transfers_api.git
cd transfers_api
# down grad java reason missing container
#sed -i 's@<java.version>12</java.version>@<java.version>11</java.version>@g' pom.xml
docker run -it --rm -v maven-repo:/root/.m2  --name my-maven-project -v "$(pwd)":/usr/src/mymaven -w /usr/src/mymaven ${DOCKER_MAVEN_PROJECT_CONTAINER} mvn clean install


cd
cd playground
PROJECT="transfers_api"
sudo rm -rf "${PROJECT}"
GIT_URL="https://github.com/VictorGil/transfers_api.git"
JAR_FILE_NAME="transfers-api-1.1.0.jar"
sudo rm -rf "${PROJECT}"
git clone "${GIT_URL}"
cd "${PROJECT}"
#sed -i 's@<java.version>.*</java.version>@<java.version>11</java.version>@g' pom.xml
# build with docker-maven
docker run -it --rm -v maven-repo:/root/.m2  --name my-maven-project -v "$(pwd)":/usr/src/mymaven -w /usr/src/mymaven ${DOCKER_MAVEN_PROJECT_CONTAINER} mvn clean install
# import to maven local repo
docker run -it --rm -v maven-repo:/root/.m2  --name my-maven-project -v "$(pwd)":/usr/src/mymaven -w /usr/src/mymaven ${DOCKER_MAVEN_PROJECT_CONTAINER} mvn org.apache.maven.plugins:maven-install-plugin:2.5.2:install-file -Dfile=target/${JAR_FILE_NAME}


```

## git clone https://github.com/VictorGil/transfers_recording_service.git

```bash
cd
cd playground
PROJECT="transfers_recording_service"
sudo rm -rf "${PROJECT}"
GIT_URL="https://github.com/VictorGil/transfers_recording_service.git"
JAR_FILE_NAME="transfers-recording-service-1.1.0.jar"
sudo rm -rf "${PROJECT}"
git clone "${GIT_URL}"
cd "${PROJECT}"
#sed -i 's@<java.version>.*</java.version>@<java.version>11</java.version>@g' pom.xml
# build with docker-maven
docker run -it --rm -v maven-repo:/root/.m2  --name my-maven-project -v "$(pwd)":/usr/src/mymaven -w /usr/src/mymaven ${DOCKER_MAVEN_PROJECT_CONTAINER} mvn clean install
# import to maven local repo
docker run -it --rm -v maven-repo:/root/.m2  --name my-maven-project -v "$(pwd)":/usr/src/mymaven -w /usr/src/mymaven ${DOCKER_MAVEN_PROJECT_CONTAINER} mvn org.apache.maven.plugins:maven-install-plugin:2.5.2:install-file -Dfile=target/${JAR_FILE_NAME}
```

## https://github.com/VictorGil/kafka_util.git

```bash
cd
cd playground
PROJECT="kafka_util"
GIT_URL="https://github.com/VictorGil/kafka_util.git"
JAR_FILE_NAME="kafka-util-1.0.0-SNAPSHOT.jar"
sudo rm -rf "${PROJECT}"
git clone "${GIT_URL}"
cd "${PROJECT}"
#sed -i 's@<java.version>.*</java.version>@<java.version>11</java.version>@g' pom.xml
# build with docker-maven
docker run -it --rm -v maven-repo:/root/.m2  --name my-maven-project -v "$(pwd)":/usr/src/mymaven -w /usr/src/mymaven ${DOCKER_MAVEN_PROJECT_CONTAINER} mvn clean install
# import to maven local repo
docker run -it --rm -v maven-repo:/root/.m2  --name my-maven-project -v "$(pwd)":/usr/src/mymaven -w /usr/src/mymaven ${DOCKER_MAVEN_PROJECT_CONTAINER} mvn org.apache.maven.plugins:maven-install-plugin:2.5.2:install-file -Dfile=target/${JAR_FILE_NAME}
```

## https://github.com/VictorGil/account_balance_workflow_api.git

```bash
cd
cd playground
PROJECT="account_balance_workflow_api"
GIT_URL="https://github.com/VictorGil/account_balance_workflow_api.git"
JAR_FILE_NAME="account-balance-workflow-api-1.0.0.jar"
sudo rm -rf "${PROJECT}"
git clone "${GIT_URL}"
cd "${PROJECT}"
#sed -i 's@<java.version>.*</java.version>@<java.version>11</java.version>@g' pom.xml
# build with docker-maven
docker run -it --rm -v maven-repo:/root/.m2  --name my-maven-project -v "$(pwd)":/usr/src/mymaven -w /usr/src/mymaven ${DOCKER_MAVEN_PROJECT_CONTAINER} mvn clean install
# import to maven local repo
docker run -it --rm -v maven-repo:/root/.m2  --name my-maven-project -v "$(pwd)":/usr/src/mymaven -w /usr/src/mymaven ${DOCKER_MAVEN_PROJECT_CONTAINER} mvn org.apache.maven.plugins:maven-install-plugin:2.5.2:install-file -Dfile=target/${JAR_FILE_NAME}
```

## https://github.com/VictorGil/cadence_transfers_recording_service.git

```bash
cd
cd playground
PROJECT="cadence_transfers_recording_service"
GIT_URL="https://github.com/VictorGil/cadence_transfers_recording_service.git"
JAR_FILE_NAME="transfers-recording-service-1.0.0-SNAPSHOT.jar"
sudo rm -rf "${PROJECT}"
git clone "${GIT_URL}"
cd "${PROJECT}"
#sed -i 's@<java.version>.*</java.version>@<java.version>11</java.version>@g' pom.xml
# build with docker-maven
docker run -it --rm -v maven-repo:/root/.m2  --name my-maven-project -v "$(pwd)":/usr/src/mymaven -w /usr/src/mymaven ${DOCKER_MAVEN_PROJECT_CONTAINER} mvn clean install
# import to maven local repo
docker run -it --rm -v maven-repo:/root/.m2  --name my-maven-project -v "$(pwd)":/usr/src/mymaven -w /usr/src/mymaven ${DOCKER_MAVEN_PROJECT_CONTAINER} mvn org.apache.maven.plugins:maven-install-plugin:2.5.2:install-file -Dfile=target/${JAR_FILE_NAME}
```

## start with maven

```bash
cd
cd playground
PROJECT="transfers_websockets_service"
sudo rm -rf "${PROJECT}"
GIT_URL="https://github.com/VictorGil/transfers_websockets_service.git"
JAR_FILE_NAME="transfers-websockets-service-1.0.0.jar"
sudo rm -rf "${PROJECT}"
git clone "${GIT_URL}"
cd "${PROJECT}"
#sed -i 's@<java.version>.*</java.version>@<java.version>11</java.version>@g' pom.xml
# build with docker-maven
docker run -it --rm -v maven-repo:/root/.m2  --name my-maven-project -v "$(pwd)":/usr/src/mymaven -w /usr/src/mymaven ${DOCKER_MAVEN_PROJECT_CONTAINER} mvn clean install
# import to maven local repo
docker run -it --rm -v maven-repo:/root/.m2  --name my-maven-project -v "$(pwd)":/usr/src/mymaven -w /usr/src/mymaven ${DOCKER_MAVEN_PROJECT_CONTAINER} mvn org.apache.maven.plugins:maven-install-plugin:2.5.2:install-file -Dfile=target/${JAR_FILE_NAME}
```
