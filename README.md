# datarhei_Restreamer
Installation des Datarhei Restreamer auf einem vServer bei Hetzner
## Hetzner vServer
- CX11 (x86 1 vCPUs, 2 GB RAM, 20 GB Disk Lokal) mit Ubuntu 22.04 LTS reicht aus :-)
- Firewall erstellen und anwenden (UDP: Port 6000 für SRT, TCP 1935-1936 für RTMP, Port 22 für SSH, TCP 8080 und 8181)
```
apt update
apt install docker.io
```
### datarhei Restreamer
>Selbstbeschreibung des Anbieters:
>datarhei Restreamer ist eine moderne Open Source Videostreaming-Plattform, die sich jeder kostenlos installieren kann. Ohne einen Videohoster können Videos von Webcams bis hin zu Fernsehprogrammen zuverlässig live übertragen werden. Wir wollen privaten und professionellen Videocreators dabei helfen, effizienter zu streamen, indem wir einen einfachen, aber dennoch leistungsstarken Service für zum Livestreaming von Videos zur Verfügung stellen. Unsere Mission ist es, ein benutzerfreundliches und leistungsfähiges Produkt für jedermann zum Streamen von Videos zu schaffen.
```
# Installation siehe: https://docs.datarhei.com/restreamer/installing/linux
docker run --detach --name core --privileged --volume /opt/core/config:/core/config --volume /opt/core/data:/core/data --publish 8080:8080 --publish 8181:8181 --publish 1935:1935 --publish 1936:1936 --publish 6000:6000/udp datarhei/restreamer:latest
```
### Installation per Cloud-init Konfiguration
```
#cloud-config
package_update: true
package_upgrade: false
packages:
  - docker.io

runcmd:
  - systemctl enable --now docker
  - docker run -d --restart=always --name restreamer -p 8080:8080 -p 8181:8181 -p 1935:1935 --privileged datarhei/restreamer:latest

```
Nach wenigen Augenblicken kann bereits die Adminseite des Restreamer über `http://<meineIP>:8080/ui` aufgerufen werden.

### Owncast bei Hetzner
>lt. Hetzner:
>Mit dieser App wird Ihr Server zu einem einsatzbereiten Live-Streaming- und Chat-Server. Mit Owncast können Sie von OBS-Studio oder jeder anderen rtmp-Medienquelle streamen.

datarhei Restreamer bietet eine automatische Weiterleitung zu Owncast an, deshalb im Folgenden Infos zur Installation:
#### Installation
Hetzner bietet eine fertige Konfiguration an. Einfach neuen Server erstellen, OS (Ubuntu 22.04) und App (Owncast) auswählen und Server erstellen anklicken.
