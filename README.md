# lightsail-htpc
Home theater PC on Lightsail with Plex, Sonarr, Radarr, Ombi, NZBGet, NZBHydra, Letsecnrypt, DuckDNS

## Edit config
Edit the following lines in `user-data.txt` 
```
EMAIL=example@mail.com
URL=example.duckns.org
TOKEN=duck-dns-token
TZ=Europe/Brussels
```

## Create instance
Create AWS Lightsail instance via [AWS Lightsail CLI](https://docs.aws.amazon.com/cli/latest/reference/lightsail/index.html "AWS Lightsail CLI")
```
aws lightsail create-instances --instance-names "lightsail-htpc-eu-central-vm" --availability-zone eu-central-1a --blueprint-id centos_7_1805_01 --bundle-id medium_2_0 --user-data file://user-data.txt
```
You can find the script in `/var/lib/cloud/instance/user-data.txt` on the instance

## Configure instance - open ports
```
aws lightsail open-instance-public-ports --port-info fromPort=443,toPort=443,protocol=TCP --instance-name lightsail-htpc-eu-central-vm
aws lightsail open-instance-public-ports --port-info fromPort=32400,toPort=32400,protocol=TCP --instance-name lightsail-htpc-eu-central-vm
```
