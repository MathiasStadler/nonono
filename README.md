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

## start with maven
cd transfers_websocket_service
docker run -it --rm -v maven-repo:/root/.m2  --name my-maven-project -v "$(pwd)":/usr/src/mymaven -w /usr/src/mymaven maven:3.5.3-jdk-11-slim mvn clean install

## git clone transfer-api

```bash
cd
cd playground
sudo rm -rf transfers_api
git clone https://github.com/VictorGil/transfers_api.git
cd transfers_api
# down grad java reason missing container
sed -i 's@<java.version>12</java.version>@<java.version>11</java.version>@g' pom.xml
docker run -it --rm -v maven-repo:/root/.m2  --name my-maven-project -v "$(pwd)":/usr/src/mymaven -w /usr/src/mymaven maven:3.5.3-jdk-11-slim mvn clean install

```

## import in local repo maven-repo
docker run -it --rm -v maven-repo:/root/.m2  --name my-maven-project -v "$(pwd)":/usr/src/mymaven -w /usr/src/mymaven maven:3.5.3-jdk-11-slim mvn install:install-file –Dfile=./target/transfers-api-1.1.0.jar -DgroupId=net.devaction.kafka -DartifactId=transfers-api -Dversion=1.1.0


## git clone https://github.com/VictorGil/transfers_recording_service.git

```bash
cd
cd playground
git clone https://github.com/VictorGil/transfers_recording_service.git
cd transfer_recording_services
# build with docker-maven
docker run -it --rm --name my-maven-project -v "$(pwd)":/usr/src/mymaven -w /usr/src/mymaven maven:3.6.3-jdk-11-slim mvn -X clean install

```