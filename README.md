# Project WatchTower

## Pre-req

Mac, Google Drive, Tailscale, Docker Desktop, Amphetamine App, ATS, Crond, standardnotes, bitwarden

## Google Drive
* setup /sn/ for standardnotes backup files
* setup /bw/ for bitwarden backup files

## Tailscale
* Generate SSL cert for the domain
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
* Using backup for data restore - https://bitwarden.com/help/backup-on-premise

## Crond 
* use crond on alpine
* Setup /work for user directory
* Setup /dest for Google Drive
* Edit /var/spool/cron/crontabs/root
```
0 3 * * * /bin/cp -R -u /work/Standard\ Notes\ Backups/ /dest/My\ Drive/sn/
0 4 * * * /bin/cp -R -u /work/bwdata/ /dest/My\ Drive/bw/
```

