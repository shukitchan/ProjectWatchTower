# Project WatchTower

Self Hosting useful applications for your home
Standard Notes: Note Taking application
Bitwarden: Password Management application

## Pre-req

a working Mac machine, Tailscale, Docker Desktop, Amphetamine App, ATS, standardnotes, bitwarden

## Tailscale
* Generate SSL cert for the domain name. Domain name is the full domain that can be retrieved from tailscale web UI
```
/Applications/Tailscale.app/Contents/MacOS/Tailscale cert <domain name>
```

## Amphetamine App
* Create Indefinitely New Session
* Yes - "Allow display sleep"
* No - "Allow system sleep when display is closed"
 
## ATS
* Use ats-alpine container
* expose container port 8443 to port 443
* setup /work to ~/Library/Containers/io.tailsacle.ipn/macos/Data
* records.config - export port 8443 as SSL server port
* allow DELETE in ip_allow.yaml
* remap.config
```
map https://<domain name>/headers http://httpbin.org/headers
map https://<domain name>/sn/ http://host.docker.internal:3000/
map https://<domain name>/bw/ http://host.docker.internal:30080/
map wss://<domain name>/bw/ ws://host.docker.internal:30080/
```
* ssl_multicert.config
```
dest_ip=* ssl_cert_name=/work/<domain name>.crt ssl_key_name=/work/<domain name>.key
```

## standardnotes
* https://docs.standardnotes.com/self-hosting/docker
* Self host repo - https://github.com/shukitchan/standalone
* Decrypt repo for data restore - https://github.com/shukitchan/decrypt

## bitwarden
* https://bitwarden.com/help/install-on-premise-linux
  * provide a domain name
  * not using let's encrypt, not having a SSL cert to use, do not generate self-signed SSL cert
  * need installation key and id
  * Change config.yml in bwdata to use 30080/30443 for ports
* Self host repo - https://github.com/shukitchan/self-host
* Can consider using backup for data restore - https://bitwarden.com/help/backup-on-premise

