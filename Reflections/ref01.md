# Reflexion 1 – Netzwerkgrundlagen, MQTT und OpenWrt

**14.11.2026 · Erfahrungen aus Pre-Study und den Modulen 1–2**

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

## Zusammenarbeit

Die Pre-Study habe ich allein bearbeitet. Ab Modul 1 arbeite ich mit Jonas
zusammen. Mit einem anderen Pod haben wir verglichen, wie die Temperatursensoren
auf das Anfassen reagieren, und über das OTA-Update gesprochen. Der Professor
hat uns bei der Dashboard-Adresse weitergeholfen.

Die ersten vier Aufgaben aus Modul 2 haben wir in der Vorlesung gemacht,
die Aufgaben 5–9 zu Hause. Die Erfahrungen aus Modul 3 ergänze ich hier später.
