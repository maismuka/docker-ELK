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

Since this is ARM machine, no Suricata repository can be add.

Proceed with basic `apt install`

`apt install suricata suricata-update`

Clone this git `git clone https://github.com/401trg/utilities/blob/master/suricata_et_rule_update.py`

And run `python3 suricata_et_rule_update.py`

This will download rules and configuration files from Emerging Threat's open ruleset and move them to /etc/suricata/. It also enabled informational rules in EmergingThreat's included configuration file

Then update the rules, run `suricata-update`

View available sources `suricata-update list-sources`. The results is somewhat as below;

```
15/2/2020 -- 07:15:16 - <Info> -- Using data-directory /var/lib/suricata.
15/2/2020 -- 07:15:16 - <Info> -- Using Suricata configuration /etc/suricata/suricata.yaml
15/2/2020 -- 07:15:16 - <Info> -- Using /etc/suricata/rules for Suricata provided rules.
15/2/2020 -- 07:15:16 - <Info> -- Found Suricata version 4.1.2 at /usr/bin/suricata.
Name: et/open
  Vendor: Proofpoint
  Summary: Emerging Threats Open Ruleset
  License: MIT
Name: et/pro
  Vendor: Proofpoint
  Summary: Emerging Threats Pro Ruleset
  License: Commercial
  Replaces: et/open
  Parameters: secret-code
  Subscription: https://www.proofpoint.com/us/threat-insight/et-pro-ruleset
Name: oisf/trafficid
  Vendor: OISF
  Summary: Suricata Traffic ID ruleset
  License: MIT
```

Proceed to enable anything you want, for example `suricata-update enable-source oisf/trafficid`

Redo untill satisfy.

Lastly, to rerun `suricata-update` to verify changes.

Restart the Suricata.

ITS DONE!

## 5. To create Telegram alert on new log appeared at /var/log/suricata/fast.log






