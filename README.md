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

Now the ELK Stack docker is up and running.

Check the ELK by using curl command `curl localhost:9200`

Or try login the Kibana website from another cdevice by connecting to the same network and open the browser `htto://192.168.0.157:5601`

###### Notice that the IP address might not be the same 

###### When logged into Kibana website, there are no logs to be found because there are none data injected.

From this folder, change Logstash input to beats

`nano "/root/docker/rpi-docker-elk/logstash/config/logstash.conf"`

```
input {
	beats {
		port => 5000
	}
}
```

Save and exit the file. Restart docker-compose by `docker-compose restart`

## 3. Install beats

Clone my Github into your respective folder `git clone https://github.com/maismuka/docker-ELK.git`

Unzip filebeat file `tar -xvzf filebeat-6.5.4-linux-arm.tar.gz`

Go into the directory `cd /filebeat-6.5.4-linux-x86_64/`

Notice that there are `filebeat.yml`. Here you might want to change logs directory to ship and logstash ports

`nano filebeat.yml`

Change the `enabled: false` to `true`
Change paths you want to ship to logstash

```
#=========================== Filebeat inputs =============================

filebeat.inputs:

# Each - is an input. Most options can be set at the input level, so
# you can use different inputs for various configurations.
# Below are the input specific configurations.

- type: log

  # Change to true to enable this input configuration.
  enabled: true

  # Paths that should be crawled and fetched. Glob based paths.
  paths:
    - /var/log/suricata/fast.log
    #- c:\programdata\elasticsearch\logs\*
```

Change the port, take note the changed port to be used on the logstash after this

```
#----------------------------- Logstash output --------------------------------
output.logstash:
  # The Logstash hosts
  hosts: ["localhost:5000"]
```

Save and exit.

Start the Filebeat using command `./filebeat -c filebeat.yml -e`

###### This Filebeat only ship the data, but from the logs folder, there are none. For this testing injecting data purpose, I intend to use Suricata IDS for detecting malicious data on the same network

## 4. Install Suricata IDS




