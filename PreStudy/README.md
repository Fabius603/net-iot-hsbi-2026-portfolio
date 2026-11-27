# Pre-Study

**Versuche vom 05.11.2026 · Notizen ergänzt am 14.11.2026 · Einzelarbeit**

Für die Pre-Study habe ich mir die Einführungsvideos angesehen, mein
Laptop-Netzwerk untersucht und eine Nachricht über MQTT gesendet.
Die praktischen Versuche stehen auf zwei eigenen Seiten:

- [Netzwerk am Laptop untersuchen](network-discovery.md)
- [MQTT-Nachricht senden und empfangen](mqtt.md)

## Lernziele

Aus den [Leitfragen und Übungen der Pre-Study](../materialien/hsbi-networking-iot-10d/pre-study.md#2-required-preparation-watch--explore) abgeleitet.

<a id="lg1"></a>
### LG1 – Anwendung

Eine IoT-Anwendung mit Nutzenden und beteiligten Geräten beschreiben.

**Nachweise:** [Raumklima-Anzeige](#task-1).

Beim Entwurf der Raumklima-Anzeige habe ich festgelegt, wer die Werte nutzt und
welchen Weg sie vom Sensor über den Broker bis zum Display nehmen.

<a id="lg2"></a>
### LG2 – Adressierung

IP-Adresse und Subnetzmaske erklären sowie IP-Adresse, Subnetz und Standardgateway
am Laptop ermitteln.

**Nachweise:** [IP-Adresse und Subnetzmaske](#task-2),
[Netzwerkversuch am Laptop](network-discovery.md).

An der IP-Adresse meines Laptops und der `/28`-Maske habe ich nachvollzogen, wie
sich Netz, nutzbare Adressen und Standardgateway einordnen lassen.

<a id="lg3"></a>
### LG3 – Schichten

OSI und TCP/IP vergleichen und den Paketweg mit einer Traceroute untersuchen.

**Nachweise:** [OSI und TCP/IP](#task-3),
[Traceroute im Netzwerkversuch](network-discovery.md).

Im Beispiel mit 8 Byte Messdaten habe ich nachgerechnet, dass allein IPv4 und TCP
40 Byte Header hinzufügen. Der Aufwand entsteht durch die Protokolle und ihre
Header.

<a id="lg4"></a>
### LG4 – MQTT

Den Einsatz eines MQTT-Brokers begründen und eine Nachricht veröffentlichen und
empfangen.

**Nachweise:** [Zehn Sensoren und drei Displays](#task-4),
[MQTT-Nachricht senden und empfangen](mqtt.md).

Im Test kam meine eigene Nachricht über den Broker zurück. Am Display-Beispiel
habe ich daraus abgeleitet, warum ein neuer Empfänger dieselben Topics abonnieren
kann, ohne dass ich die Sensorprogramme ändern muss.

<a id="lg5"></a>
### LG5 – Netzgrenzen

Erklären, was den Zugriff auf Geräte in einem anderen Heimnetz begrenzt und was
ein Overlay daran ändert.

**Nachweise:** [Zugriff auf ein fremdes Heimnetz](#task-5).

Am Beispiel zweier Heimnetze habe ich erklärt, warum eine private IP-Adresse von
außen meist unerreichbar ist und wie Overlay-Adressen den Zugriff ermöglichen.

## Notizen zu den Videos

Ich habe „Teaching the IoTempower Way“ und die Lektionen 1a, 1b und 2a
von Ed Harmoush angesehen:

- [Teaching the IoTempower Way](https://www.youtube.com/watch?v=wqCoQ8RdTjY)
- [1a – Hosts, IP-Adressen und Netze](https://www.youtube.com/watch?v=bj-Yfakjllc)
- [1b – Netzwerkgeräte](https://www.youtube.com/watch?v=H7-NR3Q3BeI)
- [2a – OSI-Modell, Schichten 1–3](https://www.youtube.com/watch?v=LkolbURrtTs)

An IoTempower fand ich besonders interessant, die Messwerte mehrerer Sensoren
an einem Ort zusammenzuführen. Eine gemeinsame Anzeige könnte dann die
Werte verschiedener Messstellen zeigen.

Bei den Netzwerkvideos ist mir der Unterschied zwischen Switch und Router
klarer geworden. Ein Switch leitet Frames anhand von MAC-Adressen im lokalen
Netz weiter. Ein Router verbindet verschiedene IP-Netze und entscheidet anhand
der Ziel-IP-Adresse und seiner Routingtabelle, wohin ein Paket als Nächstes geht.

Ich verstehe jetzt auch besser, wohin mein Laptop ein Paket schickt.
Mit IP-Adresse und Subnetzmaske erkennt er, ob das Ziel im eigenen Netz
liegt. Für entfernte Ziele nutzte er bei meinem Versuch das Standardgateway.
In meinem Versuch war das `172.20.10.1`; darüber lief auch die Verbindung
zu `1.1.1.1`.

<a id="task-1"></a>
## 1. Idee für ein IoT-Projekt

**Lernziele:** [LG1](#lg1)

Eine mögliche Idee ist eine Raumklima-Anzeige für einen Lernraum.
Studierende könnten damit Messwerte an verschiedenen Stellen im Raum vergleichen.
So ließe sich die Idee aus dem Video mit mehreren Messstellen ausprobieren.

Dafür würde ich zwei M5Stack-Knoten mit passenden Sensoren einsetzen.
Sie würden ihre Werte über WLAN an einen MQTT-Broker auf dem OpenWrt-Router
senden. Ein Laptop könnte die Werte abonnieren und im Dashboard anzeigen.
Eine LED könnte zusätzlich auf einen überschrittenen Grenzwert hinweisen.

Jeder Knoten bekäme eigene Topics für Messwerte und Status. Die Anzeige
sollte auch ohne Internet funktionieren. Bisher ist das nur eine Projektidee.

<a id="task-2"></a>
## 2. IP-Adresse und Subnetzmaske

**Lernziele:** [LG2](#lg2)

Eine IP-Adresse gehört zu einer Netzwerkschnittstelle. Die Subnetzmaske legt
fest, welcher Teil der Adresse das Netz bezeichnet. Damit kann der Rechner
bestimmen, ob er ein Ziel direkt im eigenen Netz erreichen kann oder einen
Router braucht.

Mein Laptop hatte beim Versuch die Adresse `172.20.10.3` und die Maske
`255.255.255.240`, also `/28`. Das Netz ist damit `172.20.10.0/28`.
Die nutzbaren Hostadressen reichen von `172.20.10.1` bis `172.20.10.14`;
`172.20.10.15` ist die Broadcast-Adresse. Das Gateway war `172.20.10.1`.

Zwei Geräte im selben lokalen Subnetz dürfen nicht dieselbe IP-Adresse
verwenden. Sonst kann es passieren, dass über ARP unterschiedliche
MAC-Adressen zur gleichen IP bekannt werden und Pakete beim falschen Gerät
ankommen. In getrennten privaten Netzen kann dieselbe IP mehrfach vorkommen.

<a id="task-3"></a>
## 3. OSI und TCP/IP

**Lernziele:** [LG3](#lg3)

OSI teilt die Kommunikation in sieben Schichten auf. Bei TCP/IP sind es vier:
Netzzugang, Internet, Transport und Anwendung. OSI hilft mir, die Aufgaben der
Protokolle einzuordnen. TCP/IP passt direkter zu den Protokollen, die im
Netz verwendet werden.

Für kleine Sensornachrichten ist der zusätzliche Aufwand durch Header relativ
groß. Bei 8 Byte Messdaten kommen zum Beispiel bei IPv4 und TCP ohne Optionen
bereits 40 Byte Header dazu. MQTT und die Übertragung im WLAN sind dabei noch
nicht mitgerechnet. Weniger Schichten im Modell sparen für sich genommen
keine Bytes; dafür kommt es auf die verwendeten Protokolle an.

Bei einem Sensor hängen Funkübertragung, Wiederholungen und Energieverbrauch
zusammen. Eine Änderung an einer Stelle kann sich auf die anderen auswirken.
Ein Overlay verpackt Pakete außerdem noch einmal für den Weg durch ein
anderes Netz. Die Schichten lassen sich deshalb in der Praxis nicht immer
so sauber trennen wie im Modell.

<a id="task-4"></a>
## 4. Zehn Sensoren und drei Displays

**Lernziele:** [LG4](#lg4)

Wenn jeder der zehn Sensoren direkt an jedes der drei Displays sendet,
entstehen bis zu 30 einzelne Beziehungen. Ein Sensor muss dann wissen,
welche Displays es gibt und wie er sie erreicht. Ein neues Display oder eine
geänderte Adresse führt zu Änderungen an mehreren Stellen.

Mit MQTT verbinden sich alle Geräte mit dem Broker. Die Sensoren
veröffentlichen auf Topics, und die Displays abonnieren die benötigten Werte.
Ein neues Display kann dieselben Topics abonnieren, ohne dass ich die
Sensorprogramme anpassen muss.

Dafür hängt das System vom Broker ab. Fällt er aus, verteilt er auch keine
Nachrichten mehr. Bei der Raumklima-Idee würde ich ihn deshalb lokal betreiben,
damit die Anzeige zumindest unabhängig von der Internetverbindung bleibt.

<a id="task-5"></a>
## 5. Ein Gerät im Heimnetz eines Kommilitonen anpingen

**Lernziele:** [LG5](#lg5)

Mit seiner privaten IPv4-Adresse kann ich das normalerweise nicht direkt.
Eine Adresse wie `192.168.1.20` kann in vielen Heimnetzen vorkommen und wird
im öffentlichen Internet nicht geroutet. NAT und Firewall verhindern außerdem
meist, dass eine Verbindung von außen einfach bei einem Gerät im Heimnetz
ankommt. Bei gleichen Subnetzen würde mein Laptop die Adresse sogar im
eigenen Netz suchen.

Mit einem Overlay wie Nebula oder Yggdrasil könnten die Geräte über ihre
Overlay-Adressen miteinander kommunizieren. Dazu müssten beide Geräte
entsprechend eingerichtet sein und die Firewalls den Verkehr erlauben.
Andere Geräte im Heimnetz wären dadurch noch nicht automatisch erreichbar;
dafür bräuchte es zusätzliche Routen. Bei öffentlich erreichbarem IPv6 kann
eine direkte Verbindung auch ohne Overlay möglich sein, wenn die Firewalls
sie zulassen.

## Was ich aus den Versuchen mitnehme

Beim MQTT-Test habe ich meine eigene Nachricht über den Broker zurückbekommen.
Dafür reichte ein Client, der auf dasselbe Topic sendet und es abonniert.

Bei der Traceroute waren viele Zwischenstationen nur als `* * *` zu sehen.
Das Ziel antwortete trotzdem bei Hop 13. Wo genau der Übergang von privaten
zu öffentlichen IP-Adressen lag, konnte ich dadurch nicht erkennen.
Meine Erfahrungen stehen in der [Reflexion zur Pre-Study](../Reflections/ref00.md).

## Hilfsmittel und KI-Nutzung

Beim Formulieren der Texte habe ich KI-Unterstützung genutzt. Grundlage waren
meine Videonotizen, Erinnerungen und die Ausgaben aus dem Netzwerk- und MQTT-Test.
Die Angaben habe ich mit den gespeicherten Ausgaben und Screenshots abgeglichen.
