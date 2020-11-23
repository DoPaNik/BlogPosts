#  Infrastructure as Code (IaC) in 5 Minuten verstehen

In diesem Artikel wirst Du innerhalb von 5 Minuten erfahren, was "Infrastructure as Code (IaC)" ist und was sich wirklich alles hinter dem Begriff versteckt. Denn IaC ist mehr als nur Terraform! Durch den gewonnenen Überblick wirst Du problemlos das richtige Tooling für Dein Projekt zusammenstellen können, versprochen!

##  Was ist Infrastructure as Code?

Kurz gesagt versteht man unter IaC den Prozess der Automatisierung von Infrastruktur unter Zuhilfenahme von Prinzipien und Praktiken aus der Softwareentwicklung.

Doch um wirklich verstehen zu können, was damit gemeint ist, sollten wir uns die Entwicklungsgeschichte von IaC etwas genauer ansehen:

> "Nur wer die Vergangenheit kennt, kann die Gegenwart verstehen und die Zukunft gestalten" – August Bebel

Lass uns 15-20 Jahre in der Zeit zurückreisen und einen Blick auf Softwarefirmen wie Adobe, Atlassian oder Facebook werfen. Aus der heutigen Sicht sehr erfolgreiche Unternehmen. Doch auch diese haben einmal klein angefangen. Um ihre digitalen Produkte verkaufen zu können, betrieben sie zuerst einzelne Server. Nach und nach wurden aus dem einzelnen Server kleinere Rechenzentren. Diese wurden erst selbst betrieben, später kaufte man sich die Verwaltung und die Hardware von anderen Dienstleistern ein.
Bei dem Einen handelte es sich vermehrt um „Web-“ bei dem Anderen um „Lizenzserver“.
Mit den Jahren wuchs neben den Nutzerzahlen auch die Anzahl der Server in den Rechenzentren. Nur so konnte ein stabiler Betrieb sichergestellt werden – man unterschätz vollkommen wieviele Server zum Einsatz kommen. So verkündete Facebook 2009, gerade 5 Jahre nach Start ihres sozialen Netzwerkes, dass sie mehr als 30 000 Server betreiben! 

Aber selbst wenn eine Firma lediglich eine zweistellige Anzahl an Servern bereitstellen muss, kommt es schnell zu organisatorischen Herausforderungen. Dies fing damit an, dass bei einem Upgrade neue Server-Hardware bestellt und in den Serverräumen verbaut werden mussten. Dank Cloud-Computing ist dies heute mit einem geringeren Aufwand verbunden. Doch auch nachdem man die Maschinen in Betrieb genommen hat, gibt es noch allerhand zu tun. Lass uns doch einen Blick auf die notwenigen Schritte für ein Upgrade im Rechenzentrum anschauen. Man kann dieses Unterfangen in drei Teilschritte zerlegen:
 
### Server Provisionierung
Unter der "Provisionierung" versteht man die Beschaffung und Bereitstellen eines Servers. Darunter fällt das Installieren eines Serverracks im eigenen Rechenzentrum, wie auch das Hochfahren einer Serverinstanz beim Cloudprovider der Wahl (AWS, Azure, GC). Netzwerkkonfigurationen und das Erstellen eines Loadbalancer wird ebenso in diesem Teilschritt durchgeführt – weil erst danach kann der Server für die spezifische Konfiguration verwendet werden.
 
### Server Konfiguration
Die Server "Konfiguration" beinhaltet neben Usermanagement auch die Installation von Abhängigkeiten, wie Laufzeitumgebungen, aber auch Datenbanken und sonstige Arbeiten, die für den Betrieb des später zu deployenden Softwareprodukts notwendig sind.

### Server Deployment
Als letzter Schritt steht noch das "Deployment" an. Irgendwie muss das Softwareartefakt ja auf den Server geschoben werden, was heute im besten Fall durch eine Continous Deployment Pipeline durchgeführt wird, wurde früher oftmals manuell durchgeführt.

Bei jedem neuen Server müssen diese Schritte abgehandelt werden. Doch der Betrieb der "alten" Server sowie die Weiterentwicklung der Produkte soll natürlich weitergehen. So kam es dazu, dass während neue Server bereitgestellt, gleichzeitig andere Server modifiziert wurden. Oder zweckentfremdet. Da kann es kurzum passieren, dass aus einer "Testmaschine" eine neue "Produktiv-Instanz" gemacht wird. Manchmal muss auch ganz dringend eine andere Laufzeitumgebungen getestet oder generell anderes Tooling, nach Vorlieben des Entwicklers, installiert werden.

> "The result is a unique snowflake - good for a ski resort, bad for a data center." - Martin Fowler [https://martinfowler.com/bliki/SnowflakeServer.html][1]

Solche nicht dokumentiertem Adhoc-Änderungen führen längerfristig immer zu Problemen. Man redet hierbei von einem "Configuration Drift", der Zeitpunkt ab dem man von der Standardkonfiguration des Servers abweicht.
Durch die lange Laufzeit der Server sammeln sich nach und nach immer mehr Altlasten an. Schritt für Schritt mutieren die Server zu sogenannte Snowflake-Server – jeder ein Unikat und damit fast unmöglich zu warten.

> "A server should be like a phoenix, regularly rising from the ashes" - Martin Fowler [https://martinfowler.com/bliki/PhoenixServer.html][2]

Aufgrund dieser Tatsache etablierte sich das PhoenixServer Pattern, welches diesem Umstand ein Ende setzten wollte. "Wie der Phoenix aus der Asche", soll ein Server virtuell abgebrannt und in regelmäßigen Abständen neu aufgesetzt werden.

Doch ohne Automatisierung der Infrastruktur hätte man dies nicht bewältigen können. Anfangs Versuchte man die Automatisierung mit Shell-Skripten abzubilden. Schnell wurde klar, dass diese Scripte durch ihre oftmals hohe Komplexität und die dadurch schlechte Portierfähigkeit auf die verschiedensten Serversysteme, nicht das Mittel der Wahl sein konnte. 1993 wurde mit dem Projekt „CFEngine“ das erste IaC Tool entwickelt. CFEngine bietet ein Betriebssystem unabhängige Schnittstelle und abstrahiert damit die Unterschiede der verschiednen Linux Distributionen. Mit seiner deklarativen und domainspezifischen Beschreibungssprache vereinfachte das Tool die Konfiguration von Servern ungemein. Heute existieren viele ähnliche Lösungen. 
![IaC Tools Überblick mit Zeit]()


## Die verschiedenen Phasen
Wenn man IaC anwendet, unterscheidet man zwischen zwei Phasen. Jede Phase deckt bestimmte Arbeiten ab:
1. Initiale Setup Phase
	-  Provisionierung der Infrastruktur 
	- Konfiguration der Infrastruktur
	- Initiales Installieren von Software
	- Initiale Konfiguration von Software
2. Maintaining Phase 
	- Anpassen der Infrastruktur
	- Entfernen und Hinzufügen von Komponenten
	- Software Updates
	- Rekonfiguration von Software

Um es einwenig zu abstrahieren, werde ich nachfolgend von dem Initialen Infrastruktur Setup und derem Management sowie vom initialen Applikations Setup und dessen Management reden. Dies deckt dann die beiden Phasen komplett ab.
![]()

## Die verschiedenen Arten von IaC
### Scripte
Der einfachste Weg, etwas zu automatisieren, ist das Schreiben eines Scripts. Dabei werden die Teilschritte, der sonst manuell durchgeführten Aufgabe, in der bevorzugten Skriptsprache abgebildet und danach in der Zielumgebung ausgeführt. Das nachfolgende Bash-Script installiert einen Webserver und startet diesen.

```bash
#!/bin/bash

# Update Package Manager
sudo apt-get update

# Install Apache
sudo apt-get install -y apache2

# Start Apache
sudo service apache2 start
```

Beliebte Skriptsprachen:
- [Bash][3]
- [Python][4]
- [Ruby][5]
- [Powershell][6]


### Konfigurations-Management Tools
Konfigurations-Management Tools, sind dafür konzipiert, Software auf vorhandenen Servern zu installieren und zu verwalten. Hier ist zum Beispiel eine Ansible-Rolle, die denselben Apache-Webserver, wie das obige Bash-Skript, konfiguriert:

```yaml
- hosts: apache
  sudo: yes
  tasks:
    - name: install apache2
      apt: name=apache2 update_cache=yes state=latest
```

Beliebte Konfigurations-Management Tools:
- [CFEngine][7]
- [Ansible][8]
- [Chef][9]
- [Puppet][10]
- [Saltstack][11]

### Server-Templating Tools
Eine Alternative zum Konfigurations-Management Tools sind Server-Templating Tools wie [Docker][12], [Packer][13] und [Vagrant][14]. Anstatt eine Reihe von Servern zu starten und sie durch Ausführen desselben Codes auf jedem einzelnen Server zu konfigurieren, besteht die Idee hinter Server-Templating-Tools darin, ein Abbild eines Servers zu erstellen. Diese "Momentaufnahme" des Betriebssystems, der Software und jeglicher Dateien kann so als eigenständiges Artefakt in Form eines Images ausgeliefert werden. Nachfolgend das Apache Beispiel in einem Dockerfile als Template für ein Ubuntu basierendes Image mit einem Webserver:

```Dockerfile
FROM ubuntu:latest

RUN apt-get -y update && \
    apt-get install -y apache2 

ENTRYPOINT ["/usr/sbin/apache2"]
CMD ["-D", "FOREGROUND"]

```

### Orchestrierungs-Tools
Server-Templating Tools eignen sich hervorragend für die Erstellung von VMs und Containern, aber wie kann man diese effizient verwalten?  Und an dieser Stelle kommen Tools wie [Kubernetes][15], [Amazon ECS][16], [Docker Swarm][17] oder [Nomad][18] ins Spiel. Da diese Tools sehr komplex sind und eine rießige Bandbreite an Funktionalität mitbringen, werde ich an dieser Stelle nicht tiefer auf die Funktionsweise eingehen, sondern lediglich ein kurzes Beispiel nennen. So kann beispielsweise mit Kubernetes festgelegt werden, wie deine Docker-Container verwaltet, wieviele Instanzen vorgehalten und wie bei einem Rollout vorgegangen werden sollen.

### Provisioning Tools
Während Konfigurations-Management , Server-Templating und Orchestrierungs-Tools den Code definieren, der auf jedem Server läuft, sind Provisioning-Tools wie [Terraform][19], [AWS CloudFormation][20] und [Pulumi][21] für die Erstellung der Server selbst verantwortlich. 
Tatsächlich können mit ihnen nicht nur Server, sondern auch Caches, Load Balancer, Firewall-Einstellungen, Routing-Regeln und so ziemlich jeder andere Aspekt einer IT-Infrastruktur erstellen werden.

## Fazit

[1]:	https://martinfowler.com/bliki/SnowflakeServer.html
[2]:	https://martinfowler.com/bliki/PhoenixServer.html
[3]:	https://www.gnu.org/software/bash/
[4]:	https://realpython.com/tutorials/devops/
[5]:	https://www.ruby-lang.org/de/
[6]:	https://docs.microsoft.com/en-us/powershell/
[7]:	https://cfengine.com/
[8]:	https://www.ansible.com/
[9]:	https://www.chef.io/
[10]:	https://puppet.com/
[11]:	https://www.saltstack.com/
[12]:	https://www.docker.com/
[13]:	https://www.packer.io/
[14]:	https://www.vagrantup.com/
[15]:	https://kubernetes.io/de/
[16]:	https://aws.amazon.com/de/ecs/
[17]:	https://docs.docker.com/engine/reference/commandline/swarm/
[18]:	https://www.nomadproject.io/
[19]:	https://www.terraform.io/
[20]:	https://aws.amazon.com/de/cloudformation/
[21]:	https://www.pulumi.com/

