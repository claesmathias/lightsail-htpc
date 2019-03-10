# lightsail-htpc
Home theater PC on Lightsail with Plex, Sonarr, Radarr, Ombi, NZBGet, NZBHydra, Letsecnrypt, DuckDNS

## Edit config
Edit the following lines in `user-data.txt` 
```
EMAIL=example@mail.com
URL=example.duckns.org
TOKEN=duck-dns-token
TZ=Europe/Brussels

htpasswd -b -c /home/centos/letsencrypt/nginx/.htpasswd admin password
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

## Application Setup
The applications are available at the following urls with the user/password configured (default: admin/password)
- Plex: `https://yoursubdomain.duckdns.org/` (to be configured first, see below)
- Sonarr: `https://yoursubdomain.duckdns.org/sonarr`
- Radarr: `https://yoursubdomain.duckdns.org/radarr`
- NZBGet: `https://yoursubdomain.duckdns.org/nzbget`
- NZBHydra: `https://yoursubdomain.duckdns.org/nzbhydra`
- Ombi: `https://yoursubdomain.duckdns.org/ombi` (to be configured first, see below)

Download the key file [here](https://lightsail.aws.amazon.com/ls/webapp/account/keys "AWS Lightsail keys")
```
chmod 600 LightsailDefaultKey-eu-central-1.pem 
```
Obtain the public IP
```
aws lightsail get-instance --instance-name lightsail-eu-central-vm | grep publicIpAddress
```
Forward Ombi port (3579) and Plex port (32400) and start configuring the applications via http://localhost:3579 http://localhost:32400/web
```
ssh -i LightsailDefaultKey-eu-central-1.pem  centos@PUBLIC_IP -L 3579:localhost:3579 -L 32400:localhost:32400
```
