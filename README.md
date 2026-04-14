# Self-Hosted Livestreaming on a vServer

Dieses Repository ist keine klassische Softwareentwicklung, sondern eine Anregung für Livestreamer, die mit wenig Aufwand einen eigenen vServer aufsetzen und moderne Open-Source-Tools ausprobieren möchten.

Mich fasziniert daran vor allem eines: Dinge, die ich mir um 2018 in Teilen noch selbst bauen oder mühsam zusammensetzen musste, sind heute mit wenigen Befehlen und guten Weboberflächen verfügbar. Ein kleiner vServer reicht oft schon aus, um spannende Streaming-Setups selbst zu betreiben.

Zwei Werkzeuge verdienen dabei aus meiner Sicht deutlich mehr Aufmerksamkeit: **datarhei Restreamer** und **Owncast**.

## Warum diese Tools spannend sind

### datarhei Restreamer
Restreamer ist ein sehr zugängliches Tool, um Streams entgegenzunehmen, weiterzuleiten und in verschiedene Richtungen zu verteilen. Gerade für kleinere Produktionen, Vereine, Einzelkämpfer oder Testumgebungen ist es beeindruckend, wie schnell man damit zu einem funktionierenden Setup kommt. Statt vieles selbst zu skripten oder zu verkabeln, bekommt man eine sofort nutzbare Weboberfläche und einen klaren Einstieg in Self-Hosted-Streaming.

### Owncast
Owncast ist auf eine andere Weise spannend: Es macht unabhängiges Livestreaming mit eigener Plattform möglich. Wer nicht nur auf große Plattformen angewiesen sein möchte, sondern einen eigenen Stream mit eigener Zuschaueroberfläche und Chat betreiben will, bekommt hier ein sehr starkes Open-Source-Werkzeug.

## Warum beides zusammen interessant ist

Restreamer und Owncast stehen für zwei wichtige Seiten moderner Streaming-Infrastruktur:  
Restreamer hilft beim technischen Umgang mit Signalen und Weiterverteilung, Owncast beim eigenständigen Publizieren für Zuschauer.

Zusammen zeigen diese Tools, wie weit man heute mit einem einfachen vServer und Open Source schon kommt.

## Für wen dieses Repo gedacht ist

Dieses Repository richtet sich an:

- Livestreamer mit technischem Interesse
- Vereine und kleine Veranstalter
- Leute, die Self-Hosted-Streaming ausprobieren möchten
- alle, die einen vServer nicht nur für Webseiten, sondern auch für Medien-Workflows nutzen wollen

## Ziel dieses Repos

Dieses Repo soll nicht das vollständige Thema erklären, sondern Lust machen, die beiden Tools kennenzulernen, schnell einen Server aufzusetzen und eigene erste Erfahrungen zu sammeln.

## Beispiel: kleiner vServer bei Hetzner

Ein einfacher Linux-vServer reicht oft bereits aus, um erste Tests mit Restreamer oder Owncast zu machen.

### Grundidee
- Ubuntu oder Debian installieren
- Docker / Docker Compose einrichten
- Tool starten
- Weboberfläche aufrufen
- ersten Stream testen

## Persönliche Einordnung

Für mich ist dieses Repo auch eine Erinnerung daran, wie sehr sich die Streaming-Welt verändert hat.  
Was früher Eigenbau, viele Einzelschritte oder Spezialwissen erforderte, ist heute teilweise mit erstaunlich guten Open-Source-Tools zugänglich geworden.

Gerade deshalb lasse ich dieses Repo online: Viele Livestreamer kennen solche Werkzeuge noch nicht, obwohl sie für kleine und mittlere Setups enorm hilfreich sein können.

---

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
