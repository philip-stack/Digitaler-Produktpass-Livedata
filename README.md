# Digitaler-Produktpass-Livedata
Installation von Docker und Aktivierung 
von WSL 2 
Für den Betrieb der Anwendung in einem Docker-Container wird Docker Desktop benötigt. Unter 
Windows ist dafür zusätzlich das Windows-Subsystem für Linux (WSL 2) erforderlich. 
Um WSL zu aktivieren, gibt es zwei Möglichkeiten. 
Methode 1: Aktivierung per Befehl wsl --install 
Öffnen Sie die Eingabeaufforderung oder PowerShell als Administrator. Das funktioniert, indem Sie mit 
der rechten Maustaste auf das Startmenü-Symbol klicken und „Windows PowerShell (Administrator)“ 
oder „Eingabeaufforderung (Administrator)“ auswählen. 
Führen Sie anschließend den Befehl 
wsl --install 
aus und bestätigen Sie mit Enter. Dieser Befehl aktiviert automatisch alle erforderlichen Windows
Features, lädt den benötigten Linux-Kernel herunter, legt WSL 2 als Standard fest und installiert 
standardmäßig die Ubuntu-Distribution. 
Nach Abschluss der Installation kann ein Neustart erforderlich sein. Beim ersten Start der Linux
Distribution nach dem Neustart werden Sie aufgefordert, einen Benutzernamen und ein Passwort 
festzulegen. 
Methode 2: Manuelle Aktivierung über die Windows-Funktionen 
Alternativ kann WSL auch manuell eingerichtet werden. Öffnen Sie dazu die Systemsteuerung, gehen 
Sie zu „Programme“ und anschließend zu „Windows‑Funktionen aktivieren oder deaktivieren“. 
Aktivieren Sie hier die Optionen „Windows‑Subsystem für Linux“ und „Plattform für virtuelle Computer“ 
(wird für WSL 2 benötigt). Klicken Sie auf „OK“, um die Installation zu starten, und starten Sie den 
Computer neu, wenn Sie dazu aufgefordert werden. 
Nach dem Neustart muss eine Linux-Distribution manuell über den Microsoft Store installiert werden. 
Beispielsweise, indem Sie nach „Ubuntu“ oder allgemein nach „Linux“ suchen. Für diese Anleitung 
wurde die folgende Version verwendet:  
https://apps.microsoft.com/detail/9PDXGNCFSCZV?hl=neutral&gl=AT&ocid=pdpshare 
Nachdem WSL 2 erfolgreich eingerichtet ist, kann Docker Desktop installiert werden. Die 
Installationsdatei steht auf der offiziellen Webseite unter https://www.docker.com/products/docker
desktop/ zur Verfügung. Während des Installationsvorgangs sollte darauf geachtet werden, dass die 
Option „Use WSL 2 instead of Hyper‑V“ aktiviert bleibt. Nach Abschluss wird Docker Desktop gestartet. 
Die erfolgreiche Einrichtung kann anschließend mit dem Befehl „docker –version“ oder durch den Start 
des Testcontainers „docker run hello-world“ überprüft werden. Erscheint die Testausgabe ohne Fehler, 
ist Docker korrekt installiert und die Umgebung für den Containerbetrieb vollständig eingerichtet.  
3 
Installation und Start der Anwendung 
mit Docker Compose 
Nachdem Docker Desktop installiert und WSL 2 aktiviert wurde, kann die Anwendung über Docker 
Compose eingerichtet und gestartet werden. 
Im Projekt befinden sich zwei Hauptordner „dpp_sortingmachine_live“ und „dpp_sortingmachine_sim“, 
die jeweils einen eigenen Container darstellen. 
Zur Installation wird das gewünschte Verzeichnis in der PowerShell oder einem Terminal geöffnet. Mit 
dem Befehl ls kann überprüft werden, ob sich in diesem Verzeichnis die Datei „docker-compose.yml“ 
befindet. Nur wenn diese Datei sichtbar ist, kann der folgende Schritt erfolgreich ausgeführt werden. 
Je nach Anwendungszweck wird entweder das Verzeichnis „dpp_sortingmachine_live“ oder 
„dpp_sortingmachine_sim“ geöffnet: 
• „dpp_sortingmachine_live“ wird verwendet, um den Digitalen Produktpass der Sortiermaschine 
in der digitalen Miniaturfabrik zu betreiben. 
• „dpp_sortingmachine_sim“ dient dem Start der Simulation einer Produktionslinie der 
Sortiermaschine. 
Innerhalb des gewählten Verzeichnisses wird anschließend der folgende Befehl ausgeführt: 
docker-compose up --build 
Damit werden die Container‑Images für die jeweilige Anwendung neu erstellt und gestartet. Docker lädt 
automatisch alle notwendigen Abhängigkeiten und richtet die Umgebung vollständig ein. Der Prozess 
kann je nach Systemleistung einige Minuten dauern. 
Nach erfolgreichem Start erscheinen im Terminal die Log‑Ausgaben des Containers. Die laufenden 
Container können danach bequem über Docker Desktop verwaltet werden. In der Oberfläche werden 
die gestarteten Services angezeigt. Dort können sie gestartet, gestoppt oder neu gestartet werden. Die 
installieren Container können in der folgenden Abbildung 1 gesehen werden. 
Abbildung 1: Installierte Container in Docker Desktop 
4 
Über Docker Desktop lässt sich außerdem prüfen, auf welchem Port die Anwendung bereitgestellt wird. 
Die Applikation kann anschließend im Browser über die angegebene Adresse http://localhost:<Port>, 
geöffnet werden. 
Zum Beenden kann der Container entweder direkt in Docker Desktop gestoppt oder im Terminal mit 
docker-compose down 
heruntergefahren werden. Beim nächsten Start reicht erneut der Befehl „docker-compose up –build“, 
um die Umgebung wieder vollständig bereitzustellen. 
Einrichtung des Containers 
dpp_sortingmachine_live 
Nach dem erfolgreichen Start des Containers mit 
docker-compose up --build 
kann die Benutzeroberfläche der Anwendung im Browser über http://localhost:3000 aufgerufen werden. 
Das angezeigt User-Interface sollte wie in der folgenden Abbildung aussehen. 
Abbildung 2: User-Inerface für „dpp_sortingmachine_live“ 
Zusätzlich 
stellt 
der 
Container 
eine 
Node‑RED‑Umgebung 
bereit, 
über 
http://127.0.0.1:1880/#flow/flowShellyCO2 erreichbar ist. In dieser Umgebung wird die Verbindung zum 
MQTT‑Broker überprüft und bei Bedarf konfiguriert. 
die 
Nach dem Öffnen von Node‑RED sollte kontrolliert werden, ob eine Verbindung zum MQTT‑Broker 
besteht. Dies ist am grünen Statussymbol („Verbunden“) im Knoten Shelly Energy (Wh) zu erkennen. 
5 
Abbildung 3: grünes Statussymbol im Knoten „Shelly Energy (Wh)“ 
Wenn keine Verbindung besteht, wird der Knoten „Shelly Energy (Wh)“ doppelt angeklickt. 
Anschließend öffnet sich das Konfigurationsfenster. Neben dem Feld Server befindet sich ein 
Stiftsymbol, über das die Server‑Einstellungen geöffnet werden können.  
Abbildung 4: Einstellungen des Knoten "Shelly Energy (Wh)" 
Im Reiter Sicherheit müssen die folgenden Zugangsdaten hinterlegt sein: 
• Benutzername: sdibiedp:sdibiedp 
• Passwort: 2MWvO-Q_Y9Herb_mK2C_LE_6dxbvs1-1 
Abbildung 5: MQTT Konfigurationen im Reiter "Sicherheit" 
Sollten diese Felder leer sein, müssen die Daten manuell eingetragen werden. 
Anschließend ist das Topic zu überprüfen, über das die Kommunikation zwischen Node‑RED und der 
Shelly‑App erfolgt. In der Standardkonfiguration wird das Topic folgendermaßen angegeben: 
shellyplugsg3-8cbfeaa0c123/status/switch:0 
6 
Abbildung 6: Eingestelltes Topic des Knoten " Shelly Energy (Wh)" 
Manuelles Verbinden über die Shelly App 
Für die Kommunikation der Sortiermaschine mit dem MQTT‑Broker wird der Shelly Plug verwendet. 
Um diesen einzurichten, muss zunächst die Shelly App auf einem Smartphone oder Tablet installiert 
werden. 
Die Shelly App ist kostenlos im Google Play Store (für Android) sowie im Apple App Store (für iOS) 
verfügbar. 
Nach der Installation wird die App geöffnet und ein Shelly‑Konto angelegt oder falls bereits vorhanden, 
mit bestehenden Zugangsdaten angemeldet. Das Benutzerkonto wird benötigt, um Geräte zu verwalten 
und Cloud‑Funktionen zu aktivieren. 
Anschließend wird der Shelly Plug mit einer Steckdose verbunden. Nach dem Einschalten startet das 
Gerät im Einrichtungsmodus. Auf dem Smartphone wird nun über die WLAN‑Einstellungen das 
temporäre Netzwerk des Geräts gesucht. Dieses trägt üblicherweise den Namen 
shellyplug<Geräte-ID> 
Dieses Netzwerk wird ausgewählt und verbunden. Danach kehrt man in die Shelly App zurück, in der 
automatisch ein Einrichtungsassistent erscheint. 
Im Assistenten wird das WLAN‑Netzwerk ausgewählt, mit dem der Shelly Plug verbunden werden soll. 
Nach der Eingabe des WLAN‑Passworts verbindet sich das Gerät mit dem Netzwerk. Nach kurzer Zeit 
erscheint der Shelly Plug in der Geräteübersicht der App. 
Wird das Gerät nicht automatisch gefunden, kann es über „Gerät hinzufügen“ manuell eingebunden 
werden. Dazu wird erneut das Shelly‑Netzwerk ausgewählt, und die App führt durch den 
Verbindungsprozess. 
Nach erfolgreicher Einrichtung ist der Shelly Plug betriebsbereit und im gleichen Netzwerk erreichbar. 
Nun können in den Geräteeinstellungen die MQTT‑Verbindungseinstellungen vorgenommen werden 
(Hostname, Benutzername, Passwort, Zertifikat und Topic), sodass der Datenaustausch mit Node‑RED 
und dem Container „dpp_sortingmachine_live“ funktioniert. 
7 
Das Topic kann jedoch frei gewählt werden. Wenn nach der Einrichtung keine Nachrichten (Messages) 
im Debug‑Fenster von Node‑RED angezeigt werden, wird empfohlen, ein anderes Topic zu verwenden. 
Beispielweise kann an das bestehende Topic eine zusätzliche Zahl angehängt werden, etwa: 
shellyplugsg3-8cbfeaa0c12312/status/switch:0 
Wichtig ist, dass dieselbe Änderung sowohl in Node‑RED als auch in der Shelly‑App vorgenommen 
wird, damit die Kommunikation konsistent bleibt. Nach der Anpassung sollte außerdem überprüft 
werden, ob der Benutzername und das Passwort im Reiter Sicherheit weiterhin korrekt hinterlegt sind, 
da diese Daten bei Änderungen am Topic gelegentlich neu eingetragen werden müssen. 
In der Shelly‑App müssen generell die folgenden Einstellungen vorhanden sein: 
• Hostname: seal.lmq.cloudamqp.com 
• Benutzername und Passwort: wie oben beschrieben 
• Zertifikat: ca.pem 
• Aktivierte Optionen: 
o RPC status notifications over MQTT 
o Generic status update over MQTT 
o SSL connectivity 
• Topic: identisch mit dem in Node‑RED eingetragenen Wert 
Diese Einstellungen können auch in der folgenden Abbildung gesehen werden. 
Abbildung 7: Konfigurationen in der Shelly App 
Nach dem Speichern dieser Einstellungen sollten im Debug‑Fenster von Node‑RED regelmäßig neue 
Nachrichten erscheinen. Dies zeigt an, dass die Verbindung erfolgreich hergestellt wurde und Daten 
empfangen werden. 
8 
Das Sendeintervall zur Datenbank kann über die Node Begrenzung 1 msg/30 m angepasst werden. 
Standardmäßig ist hier ein Intervall von 30 Minuten eingestellt. Dieser Wert kann je nach benötigter 
Datendichte verändert werden. 
Damit ist der Container „dpp_sortingmachine_live“ vollständig eingerichtet, mit dem MQTT‑Broker 
verbunden und betriebsbereit. 
