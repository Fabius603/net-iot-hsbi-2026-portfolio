# Reflexion 1 – Netzwerkgrundlagen, MQTT, OpenWrt und Overlay

**14.11.2026, ergänzt am 20.11.2026 · Erfahrungen aus Pre-Study und den Modulen 1–3**

## Was ich mitnehme

Mir ist der Unterschied zwischen Switch und Router klarer geworden.
Ein Switch arbeitet im lokalen Netz mit MAC-Adressen. Ein Router verbindet
IP-Netze und entscheidet, wohin ein Paket als Nächstes geht.

Ich verstehe auch besser, warum mein Laptop Pakete an das Standardgateway
schickt. `1.1.1.1` lag außerhalb meines Subnetzes, also ging die Verbindung
über die Standardroute.

In Modul 2 konnten wir die Vergabe einer Adresse direkt im Mitschnitt
verfolgen: Discover, Offer, Request und ACK gehörten zur selben
DHCP-Anfrage. Bei der Reservierung blieb der Laptop ein DHCP-Client,
bekam aber nach der Umstellung immer die vorgesehene Adresse `.50`.

In Modul 3 habe ich verstanden, warum ein Overlay ohne Portweiterleitung
auskommt: Jeder Knoten baut die Verbindung von innen nach außen auf, und
das Lighthouse vermittelt nur. Im Router-Log konnte ich sehen, wie der
Tunnel zuerst über das Relay lief und dann direkt zum Cluster wechselte.

## Was gut funktioniert hat

Beim MQTT-Test kam meine eigene Nachricht im abonnierten Topic wieder an.
Ich brauchte dafür kein zweites Gerät. Im IoTempower-Video fand ich die
Idee interessant, mehrere Sensorwerte an einem Ort zu sammeln. Dazu passt
die Idee einer Raumklima-Anzeige aus der Pre-Study.

In Modul 1 haben Jonas und ich den Temperatursensor selbst angeschlossen.
Die Firmware hat unser Professor über WLAN aktualisiert. Danach haben wir
die Werte im Debug-Bereich und im Dashboard gesehen. Auch die LED und die
Textausgabe auf dem Display haben wir angesteuert.

Der Router funktionierte zu Hause mit einem anderen Uplink weiter.
Nach der Verkleinerung auf `/26` waren Broker und Router noch unter
derselben Adresse erreichbar. Beim Portweiterleitungstest kam die
Testnachricht über die WAN-Seite an; nach dem Entfernen der Regel war
eine neue Verbindung wieder blockiert.

In Modul 3 kam unsere MQTT-Nachricht aus Node-RED über das Overlay zum
Broker auf unserem Router und wieder zurück in die Debug-Ausgabe.

## Was schwierig war

Bei der Traceroute stand an vielen Stellen nur `* * *`. Ich konnte deshalb
nicht erkennen, wo der Übergang von privaten zu öffentlichen Adressen lag.
Das Ziel war trotzdem erreichbar und antwortete bei Hop 13.

Bei der LED wollten wir die Umschaltung erst selbst schreiben. Wir haben
versucht, den Zustand abzufragen und davon abhängig neu zu schalten.
Das hat nicht geklappt. Am Ende haben wir den fertigen Baustein verwendet.

Für das Dashboard hatten wir zuerst die falsche IP-Adresse genommen.
Ich glaube, es war die des Brokers. Nach Absprache mit dem Professor
hatten wir die richtige Adresse und konnten die Temperaturwerte sehen.

In Modul 2 haben wir nach dem Flashen zunächst die Versionsnummer nicht
gefunden. Nach Absprache mit dem Professor konnten wir weitermachen.
Die Anzeige unter „Status → Overview“ zeigt den Firmwarestand direkt an.

In Modul 3 hat `apk update` erst nicht funktioniert, weil ich nur
`pk update` kopiert hatte. Das Nebula-Interface hatten wir zuerst
`nebula12` genannt und dann vorsichtshalber in `nebula1` umbenannt. Erst
später habe ich gesehen, dass der Name zu `tun.dev` im Bundle passen muss.
Die Node-RED-Adresse haben wir wegen Zahlendrehern erst nach mehreren
Versuchen erreicht. Eine LED konnten wir nicht schalten, weil der Router
keine steuerbare LED hat. Wir haben deshalb nur die Debug-Ausgabe genutzt.

## Abgleich mit den Versuchen

In den gespeicherten Netzwerkauszügen stehen IP-Adresse, Subnetz und
Standardroute. Die Traceroute zeigt die Antwort des Ziels bei Hop 13.
Auf dem MQTT-Screenshot stehen die empfangene Nachricht und die
QoS-Einstellungen. Damit lassen sich diese Erklärungen überprüfen.

Die übrigen Versuche aus Modul 1 haben im Unterricht nicht mehr geklappt. Danach
wurde die Hardware eingesammelt, und wir haben zu den offenen Teilen nur
noch recherchiert. Den Flow für die LED eines anderen Pods hatten wir
vorbereitet, aber nicht ausprobiert. Beim Header-Overhead haben wir mit
einem Beispiel gerechnet und keinen Mitschnitt ausgewertet. Die
Systemgeschichte steht als schriftlicher Entwurf im Bericht. Einen Pitch
haben wir nicht gehalten.

Für Modul 2 liegen jetzt eigene Paketmitschnitte und gespeicherte
Testergebnisse vor. DHCP, ARP, DNS und MQTT lassen sich damit an den
übertragenen Paketen prüfen. Beim NAT-Test zeigt die Verbindungstabelle
die ursprüngliche WAN-Adresse mit Port `11883` und die Antwort des Brokers
von Port `1883`. Die reine Anzeige einer Firewallregel hätte dafür nicht
gereicht. Der Vorher-/Nachher-Test zeigt außerdem, dass nur die zeitweise
Freigabe den Zugriff ermöglicht hat.

In Modul 3 haben Claude-Erklärungen und eigene Vermutungen nicht gereicht.
Ob der Tunnel direkt oder über das Relay läuft, zeigt erst das Router-Log
mit „Host roamed to new udp ip/port“. Dass der Durchsatz über das Overlay
so niedrig ist, haben wir mit iperf3 gemessen: rund 60 Mbit/s im LAN und
2,5 Mbit/s über das Overlay. In einem Kontrolllauf lag Nebula bei 57 % CPU
auf dem Router. Das stützt die Erklärung, dass der kleine Router-Prozessor
mitbremst, beweist sie aber nicht allein, weil auch die WLAN-Strecken
stark schwankten.

## Zusammenarbeit

Die Pre-Study habe ich allein bearbeitet. Ab Modul 1 arbeite ich mit Jonas
zusammen. Mit einem anderen Pod haben wir verglichen, wie die Temperatursensoren
auf das Anfassen reagieren, und über das OTA-Update gesprochen. Der Professor
hat uns bei der Dashboard-Adresse weitergeholfen.

Die ersten vier Aufgaben aus Modul 2 haben wir in der Vorlesung gemacht,
die Aufgaben 5–9 zu Hause. Modul 3 haben wir am 20.11.2026 gemeinsam
bearbeitet.
