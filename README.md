# docker-ELK
ongoing

## 1. Install Docker and Docker-compose

Copy paste into Pi4 `curl -sSL https://get.docker.com | sh`

Check docker install successfully using command `docker`

Add permission to Pi User to run Docker Commands `sudo usermod -aG docker pi`

Checking Docker completed installation `docker run hello-world`

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

Installing dependencies 
```
sudo apt-get install libffi-dev libssl-dev
sudo apt install python3-dev
sudo apt-get install -y python3 python3-pip
```

Install Docker Compose `sudo pip3 install docker-compose`


## 2. Copy elk-docker-compose

Clone into directory `https://github.com/stefanwalther/rpi-docker-elk.git`

Then go into the folder `cd/rpi-docker-elk.git`

Install the container `docker-compose up`


## 3. Install beats

