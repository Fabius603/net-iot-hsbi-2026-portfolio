# Reflexion 1 – Netzwerkgrundlagen, MQTT und Node-RED

**14.11.2026 · Erfahrungen aus Pre-Study und Modul 1**

## Was ich mitnehme

Mir ist der Unterschied zwischen Switch und Router klarer geworden.
Ein Switch arbeitet im lokalen Netz mit MAC-Adressen. Ein Router verbindet
IP-Netze und entscheidet, wohin ein Paket als Nächstes geht.

Ich verstehe auch besser, warum mein Laptop Pakete an das Standardgateway
schickt. `1.1.1.1` lag außerhalb meines Subnetzes, also ging die Verbindung
über die Standardroute.

## Was gut funktioniert hat

Beim MQTT-Test kam meine eigene Nachricht im abonnierten Topic wieder an.
Ich brauchte dafür kein zweites Gerät. Im IoTempower-Video fand ich die
Idee interessant, mehrere Sensorwerte an einem Ort zu sammeln. Dazu passt
die Idee einer Raumklima-Anzeige aus der Pre-Study.

In Modul 1 haben Jonas und ich den Temperatursensor selbst angeschlossen.
Die Firmware hat unser Professor über WLAN aktualisiert. Danach haben wir
die Werte im Debug-Bereich und im Dashboard gesehen. Auch die LED und die
Textausgabe auf dem Display haben wir angesteuert.

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

## Abgleich mit den Versuchen

In den gespeicherten Netzwerkauszügen stehen IP-Adresse, Subnetz und
Standardroute. Die Traceroute zeigt die Antwort des Ziels bei Hop 13.
Auf dem MQTT-Screenshot stehen die empfangene Nachricht und die
QoS-Einstellungen. Damit lassen sich diese Erklärungen überprüfen.

Die übrigen Versuche haben im Unterricht nicht mehr geklappt. Danach
wurde die Hardware eingesammelt, und wir haben zu den offenen Teilen nur
noch recherchiert. Den Flow für die LED eines anderen Pods hatten wir
vorbereitet, aber nicht ausprobiert. Beim Header-Overhead haben wir mit
einem Beispiel gerechnet und keinen Mitschnitt ausgewertet. Die
Systemgeschichte steht als schriftlicher Entwurf im Bericht. Einen Pitch
haben wir nicht gehalten.

## Zusammenarbeit

Die Pre-Study habe ich allein bearbeitet. Ab Modul 1 arbeite ich mit Jonas
zusammen. Mit einem anderen Pod haben wir verglichen, wie die Temperatursensoren
auf das Anfassen reagieren, und über das OTA-Update gesprochen. Der Professor
hat uns bei der Dashboard-Adresse weitergeholfen.

Die Erfahrungen aus den Modulen 2–3 ergänze ich hier später.
