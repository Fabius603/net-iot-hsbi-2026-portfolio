# Modul 1 – IoT-Grundlagen und Node-RED

**06.11.2026 · Notizen ergänzt am 14.11.2026**

Dieses Modul habe ich zusammen mit
[Jonas Prijic](https://github.com/OffBeatAtze/net-iot-hsbi-2026-portfolio)
bearbeitet. Unser gemeinsames Study log halte ich hier fest. Wir haben mit
Node-RED gearbeitet, die LED und das Display des M5Stick angesteuert und
die Werte eines selbst angeschlossenen Temperatursensors angezeigt.
Das Firmware-Update über WLAN hat unser Professor durchgeführt.

Die offenen praktischen Teile haben im Unterricht nicht mehr geklappt.
Danach haben wir dazu nur noch recherchiert. Diese Abschnitte sind als
Recherche gekennzeichnet und beschreiben, wie der Aufbau funktionieren sollte.

## Einstieg aus der Pre-Study

Beim [ersten MQTT-Test](../PreStudy/mqtt.md) habe ich meine eigene Nachricht
über einen öffentlichen Broker zurückbekommen. Sender und Empfänger konnten
also derselbe Client sein. In Modul 1 kam mit dem M5Stick ein eigenes Gerät
hinzu: Messwerte und Schaltbefehle liefen zwischen Stick und Node-RED.

## Study log

### Aufgabe 1 – Grundlagen im Pod

#### Warm-up

Laptop, Smartphone und WLAN-Sensor sind Beispiele für vernetzte Geräte.
Für die Kommunikation über IP brauchen sie einen Netzzugang, eine passende
IP-Konfiguration und gemeinsame Protokolle.

Wenn Sensor und Anzeige im selben Gebäude sind, muss die Nachricht nicht
unbedingt über das Internet laufen. Ein lokaler Broker und eine lokale
Anzeige können den Weg im eigenen Netz halten. Ein Cloud-Broker würde
auch für diesen lokalen Anwendungsfall eine Internetverbindung voraussetzen.

Eine Schicht beschreibt eine Aufgabe im Netzwerkmodell. Ein Protokoll legt
fest, wie die Kommunikation tatsächlich abläuft. TCP ist zum Beispiel ein
Protokoll für einen zuverlässigen, geordneten Bytestrom.

#### Publish/Subscribe und Topics

Der Broker nimmt veröffentlichte Nachrichten entgegen und verteilt sie an
die passenden Abonnenten. Der Sensor muss dadurch nicht wissen, welche
Anzeigen seine Werte verwenden. Weitere Anzeigen können dieselben Topics
abonnieren, ohne das Programm auf dem Stick zu ändern.

Topics sind hierarchisch aufgebaut. Im Aufgabenblatt stehen zum Beispiel
`<id>/temp` für Temperaturwerte und `<id>/led/set` für LED-Befehle.
`<id>` ist dabei ein Platzhalter für die Kennung des jeweiligen Sticks.
Der Filter `+/temp` erfasst genau eine Ebene vor `temp`; `01/#` erfasst
`01` und alle darunterliegenden Ebenen. Die Wildcards stehen im
Abonnementfilter, nicht im Topic einer veröffentlichten Nachricht.

Topic-Aufbau und Wildcards sind in der
[MQTT-Spezifikation, Abschnitt 4.7](https://docs.oasis-open.org/mqtt/mqtt/v3.1.1/os/mqtt-v3.1.1-os.html#_Toc398718106)
beschrieben.

#### OSI, TCP/IP und Header-Overhead

OSI hilft beim Einordnen der Aufgaben, während TCP/IP die praktische
Protokollfamilie beschreibt. Für den Aufwand auf der Leitung zählt aber
nicht die Zahl der Modellschichten, sondern welche Protokolle und Header
tatsächlich übertragen werden. Gerade bei kleinen Sensornachrichten kann
der zusätzliche Aufwand im Verhältnis zum Messwert groß sein.

Die Frage im Study log war: **Warum sind weniger Modellschichten nicht
automatisch effizienter?** Ein Modell überträgt selbst keine Bytes.
Zusätzliche Kapselungen brauchen Platz, auch wenn sie zur selben
Modellschicht gehören. Im Kursblatt und in RFC 3439, Abschnitt 3, geht
es um diesen zusätzlichen Aufwand. Einen eigenen Mitschnitt haben wir
dazu nicht ausgewertet.

#### Zwei Diagramme aus der Recherche

- **IP als gemeinsamer Unterbau:** Die
  [Protokollübersicht in RFC 791, Abschnitt 2.1](https://www.rfc-editor.org/rfc/rfc791.html#section-2.1)
  von Jon Postel zeigt verschiedene Protokolle über IP und darunter den
  Netzzugang. TCP ist damit nicht der einzige mögliche Transport über IP.
- **MQTT-PUBLISH-Aufbau:** Figure 3.11 der oben verlinkten MQTT-Spezifikation
  von OASIS zeigt den variablen Header mit Topic und Paketkennung.
  Die Paketkennung gibt es bei QoS 1 und 2. Topic und Header werden
  zusätzlich zum eigentlichen Messwert übertragen.

### Aufgabe 2 – Austausch mit einem anderen Pod

Mit einem anderen Pod haben wir die Temperaturanzeige und das OTA-Update
besprochen. Beim Vergleich der Temperaturwerte ging es darum, wie schnell
die Sensoren reagieren, wenn man sie zwischen die Finger nimmt.
Konkrete Reaktionszeiten haben wir dabei nicht festgehalten.

Zum OTA-Update haben wir darüber gesprochen, ob der andere Pod die
Aktualisierung über WLAN mitbekommen hatte. Dabei ging es auch darum,
woran man nach dem Update erkennt, ob der Sensor tatsächlich Werte liefert.

## Aufgabe 3 – Nachrichten in Node-RED

Node-RED verbindet einzelne Verarbeitungsschritte zu einem Flow. Im
Debug-Bereich konnten wir die Temperaturwerte direkt ablesen, die vom
Stick ankamen.

Der einfache Einstiegsflow aus dem Aufgabenblatt besteht aus Inject und
Debug: Inject erzeugt eine Nachricht und Debug zeigt deren Inhalt an.
Dieser direkte Flow braucht noch keinen MQTT-Broker. Erst MQTT-In- und
MQTT-Out-Nodes verbinden einen Flow mit dem Broker.

Beim Einstiegsflow aus dem Aufgabenblatt sollte nach Deploy und einem
Klick auf Inject die Nachricht im Debug-Bereich erscheinen, zum Beispiel
`Hello IoT!` als `msg.payload`. Wenn nichts ankommt, würde ich zuerst die
Verbindung zwischen den Nodes und die Debug-Ausgabe prüfen. Die
[Node-RED-Dokumentation zu Nachrichten](https://nodered.org/docs/user-guide/messages)
beschreibt, wie sich Payload und Datentyp dort kontrollieren lassen.

## Aufgabe 4 – Taster, LED und Display

Jonas und ich haben mit dem Taster am M5Stick sowohl die LED geschaltet
als auch Text auf dem Display ausgegeben.

Bei diesem Aufbau verbindet Node-RED die Tastermeldung mit dem passenden
Ausgabebefehl. Die MQTT-Nachrichten laufen über den Broker zurück zum
Stick. Taster und Ausgabe sitzen zwar auf demselben Gerät, die Verknüpfung
erfolgt aber über das Netzwerk und den Flow.

### Stolperstelle: LED-Zustand selbst umschalten

Für die LED haben wir zuerst versucht, die Umschaltung selbst zu schreiben.
Wir wollten den aktuellen Zustand abfragen und abhängig davon die LED ein-
oder ausschalten. Diesen Ansatz haben wir nicht zum Laufen bekommen.
Am Ende haben wir den bereits vorhandenen Baustein verwendet, statt die
Umschaltung selbst umzusetzen.

## Aufgabe 5 – Verbindung zu einem anderen Pod vorbereiten

Den Flow hatten wir vorbereitet. Zum Schalten der LED eines anderen Pods
sind wir im Unterricht aber nicht mehr gekommen.

### Recherche: erwarteter Ablauf

Der Flow müsste den Schaltbefehl auf das LED-Topic des anderen Sticks
senden. Der Broker bleibt derselbe, nur das Ziel-Topic ändert sich.
Der Nachrichtenweg wäre:

```text
Taster eigener Stick → MQTT-Broker → eigener Node-RED-Flow
                    → MQTT-Broker → LED des anderen Sticks
```

Beim Drücken sollte dann die LED des anderen Sticks reagieren.
Dafür müssen Topic, Payload, Abonnement und Verbindung passen.
Die Geräte müssen dafür nicht direkt miteinander verbunden sein. Für
eine Kette müsste jeder beteiligte Flow das passende Folgeereignis
weitergeben. Ein empfangener LED-Befehl löst nicht automatisch einen
Tastendruck oder eine weitere Nachricht aus.

Fällt der zentrale Broker aus, bricht die Vermittlung für alle beteiligten
Sticks weg. Ein gestoppter Node-RED-Flow würde dagegen mindestens die
Weitergabe an der jeweiligen Stelle unterbrechen.

## Aufgabe 6 – Temperatursensor und OTA-Update

Den Temperatursensor haben Jonas und ich selbst an den M5Stick angeschlossen.
Die Firmware für den Sensor hat unser Professor über WLAN aktualisiert.
Dieses OTA-Update musste also nicht per USB aufgespielt werden.

Danach haben wir die Temperaturwerte in Node-RED gesehen. In IoTempower
wird der Sensor mit seinem Anschluss in `setup.cpp` eingetragen.

## Aufgabe 7 – Temperaturanzeige im Dashboard

Wir konnten die Temperaturwerte im Debug-Bereich und im Dashboard sehen.

### Recherche: Anzeigen vergleichen

Für die gemeinsame Anzeige aus dem Aufgabenblatt kann man mit `+/temp`
die Temperatur-Topics mehrerer Sticks abonnieren. Die genauen Topics
unseres Aufbaus habe ich nicht notiert.

Eine Anzeige sollte mit jeder neuen Nachricht den aktuellen Wert zeigen.
Ein Diagramm sammelt die Werte als Verlauf. Beim Anfassen eines Sensors,
der kühler als die Finger ist, wäre ein allmählicher Temperaturanstieg
zu erwarten. Wie schnell man das sieht, hängt vom Sensor, dem
Messintervall und der Übertragung ab.

Bei `+/temp` müssten die Werte außerdem nach ihrem Herkunfts-Topic getrennt
zugeordnet werden, damit mehrere Sticks als verschiedene Reihen erscheinen.
Eine abweichende Topic-Struktur könnte dazu führen, dass eine Messstelle
gar nicht vom Filter erfasst wird. Ohne neue Messwerte kann eine Anzeige
den letzten Wert stehen lassen; ein unveränderter Wert beweist deshalb
noch keine funktionierende Verbindung.

### Stolperstelle: falsche Adresse für das Dashboard

Beim Öffnen des Dashboards haben wir zunächst die falsche IP-Adresse
verwendet. Soweit ich mich erinnere, war das die Adresse des MQTT-Brokers.
Der Broker verteilt die MQTT-Nachrichten; die Dashboard-Seite wird dagegen
von Node-RED bereitgestellt. Dafür braucht der Browser die passende
Webadresse einschließlich des Ports und gegebenenfalls des Dashboard-Pfads.

Die richtige Adresse haben wir nach Absprache mit unserem Professor
verwendet. Danach konnten wir die Temperaturwerte im Dashboard sehen.

## Aufgabe 8 – Header und Nutzdaten

Die Paketauswertung haben wir im Unterricht nicht mehr geschafft.
Wir haben danach recherchiert, wie sich Header und Nutzdaten aufteilen.
Die folgende Rechnung ist ein Beispiel mit angenommenen Werten.

Grundsätzlich gehören neben den Sensordaten auch Verwaltungsdaten zur
Übertragung: etwa IP- und TCP-Header sowie MQTT-Header und Topic.
Für die Auswertung einer MQTT-PUBLISH-Nachricht müsste man Framegröße
und Payload getrennt bestimmen:

- Overhead im Verhältnis zur Payload:
  `(Framebytes − Payloadbytes) / Payloadbytes`
- Anteil der Payload am Frame: `Payloadbytes / Framebytes`

### Rechenbeispiel aus der Recherche

Als vereinfachtes Beispiel nehme ich eine MQTT-PUBLISH-Nachricht mit dem
Topic `01/temp` und der Payload `23.5`. Ich rechne mit QoS 0, IPv4 und TCP
ohne Optionen. TLS ist nicht dabei, und im TCP-Segment steckt nur diese
eine MQTT-Nachricht.

| Bestandteil | Größe im Beispiel |
| --- | ---: |
| Ethernet-Header ohne VLAN | 14 Byte |
| IPv4-Header ohne Optionen | 20 Byte |
| TCP-Header ohne Optionen | 20 Byte |
| MQTT-Fixed-Header einschließlich Remaining Length | 2 Byte |
| Längenfeld des Topic-Namens | 2 Byte |
| Topic `01/temp` in UTF-8 | 7 Byte |
| Payload `23.5` in UTF-8 | 4 Byte |
| Summe | 69 Byte |

Die MQTT-Nachricht hat damit 15 Byte. Bei QoS 0 ist keine MQTT-Paketkennung
enthalten. Der Overhead des betrachteten Ethernet-Frames beträgt
`69 − 4 = 65 Byte`, im Verhältnis zur Payload also `65 / 4 = 16,25`.
Die eigentliche Payload macht `4 / 69 ≈ 5,8 %` aus.

In dieser Summe fehlen unter anderem Ethernet-FCS, Präambel, zusätzliche
TCP-Bestätigungen, Verbindungsaufbau und mögliche Wiederholungen. Bei einem
WLAN-Mitschnitt kämen andere Header dazu. Die 69 Byte gelten also nur
für dieses Beispiel.

Für eine Abschätzung der Batterielaufzeit wäre zuerst der mittlere Strom
zu bestimmen, zum Beispiel aus Sendezeit und Sendestrom sowie Ruhezeit und
Ruhestrom pro Messintervall. Aus `Kapazität in mAh / mittlerer Strom in mA`
ergäbe sich eine ideale Laufzeit in Stunden. Für unseren Aufbau fehlen
mir dafür die Stromwerte und die genauen Zeiten.

## Aufgabe 9 – Optionale Vertiefung

Für einen weiteren Versuch wäre interessant, die Verbindung zu unterbrechen
und zu beobachten, ob das Dashboard einen alten Temperaturwert weiterhin
anzeigt. Ein Zeitstempel oder eine Kennzeichnung veralteter Werte könnte
verhindern, dass die letzte Anzeige für eine aktuelle Messung gehalten wird.
Diesen Ausfalltest haben wir nur als mögliche Erweiterung betrachtet.

## Aufgabe 10 – Systemaufbau

Einen abschließenden Pitch haben wir nicht gehalten. Die folgende eigene
Skizze fasst den grundsätzlichen Weg der Temperaturwerte zur Anzeige zusammen:

```mermaid
flowchart LR
    S[Temperatursensor] --> M[M5Stick]
    M -->|MQTT über WLAN| B[MQTT-Broker]
    B -->|MQTT| N[Node-RED]
    N --> D[Dashboard im Browser]
```

Der Broker verteilt die Messwerte; Node-RED verarbeitet sie und stellt sie
für das Dashboard bereit. Ein lokaler Aufbau braucht dafür keine Verbindung
ins Internet. WLAN, Broker und Node-RED müssen aber weiterhin funktionieren.
Fällt der Broker aus, kommen keine neuen Nachrichten an. Eine Anzeige mit
einem alten Wert wäre dann ohne zusätzlichen Hinweis leicht missverständlich.

### Entwurf einer Systemgeschichte

Als Anwendung passt die Raumklima-Idee aus der Pre-Study: In einem Lernraum
sollen Studierende die Temperaturen an mehreren Stellen vergleichen können.
Die Sensorknoten senden ihre Werte an einen lokalen Broker; Node-RED macht
sie in einer gemeinsamen Anzeige sichtbar. Ein Internetausfall müsste die
lokale Anzeige nicht unterbrechen, solange die lokalen Komponenten laufen.
Die Schwachstelle wäre der Broker: Wenn er ausfällt, kommen keine neuen
Werte mehr an.

## Bezug zu den Lernzielen

| Lernziel | Bezug in meinen Notizen |
| --- | --- |
| LG1 – Kurs und Pod | Zusammenarbeit mit Jonas und Einstieg aus der Pre-Study |
| LG2 – Publish/Subscribe | Study log zu Broker, Topics und Wildcards; Taster- und LED-Versuch |
| LG3 – Node-RED | Taster, LED, Display und Temperaturanzeige; Pod-Verbindung vorbereitet, erwartetes Verhalten recherchiert |
| LG4 – Sensor und OTA | Sensor selbst angeschlossen, Firmware-Update durch den Professor |
| LG5 – Schichten und Overhead | Recherche mit ausdrücklich theoretischem Rechenbeispiel; keine Paketauswertung |
| LG6 – Systemgeschichte | Systemskizze und schriftlicher Entwurf; kein Pitch gehalten |

## Verfügbare Unterlagen

Die Hardware wurde am Ende eingesammelt. Deshalb konnten wir die offenen
Versuche nicht zu Hause nachholen. Die Beschreibung beruht auf meinen
Erinnerungen an die Arbeit mit Jonas. Screenshots und Flow-Dateien habe ich
aktuell nicht vorliegen. Falls ich noch Aufnahmen finde, ergänze ich sie.

## Reflexion

Die Erfahrungen mit der LED-Umschaltung, der Dashboard-Adresse und der
Zusammenarbeit stehen in [Reflexion 1](../Reflections/ref01.md).
