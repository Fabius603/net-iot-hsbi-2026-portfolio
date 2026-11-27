# Reflexion 0 – Pre-Study und erste Netzwerktests

## Take-away

**What do you want to remember? What would be your advice to your past self?**

Ein Switch leitet im lokalen Netz anhand von MAC-Adressen weiter. Ein Router
verbindet IP-Netze. An meinem eigenen Laptop habe ich gesehen, warum ein Ziel
außerhalb meines Subnetzes wie `1.1.1.1` über das Standardgateway läuft.

## What was good?

**Which measurement, circuit, or explanation worked well — and why?**

Beim MQTT-Test kam meine eigene Nachricht im abonnierten Topic wieder an. Ein
Client reichte aus, weil er auf dasselbe Topic veröffentlichen und es abonnieren
konnte.

## What was difficult?

**Where did you struggle? What was the root cause?**

Bei der Traceroute standen viele Zwischenstationen als `* * *` in der Ausgabe. Das
Ziel antwortete bei Hop 13. Wo die Verbindung von privaten zu öffentlichen
Adressen wechselte, konnte ich mit dieser Ausgabe nicht feststellen.

## Exploration and show stoppers

**Was there any good or fun “struggling” or exploration? Anything where you should reach out to peers or instructors?**

Im IoTempower-Video fand ich die Idee interessant, Messwerte mehrerer Sensoren an
einem Ort zu sammeln. Daraus entstand meine Idee für eine Raumklima-Anzeige.

## Interaction with peers and instructors

**Who did you work with, what did you learn from them, and what did you give back?**

Die Pre-Study habe ich allein bearbeitet.

## How did you verify AI suggestions?

**What did you ask an AI, and how did you check the answer against a datasheet, a measurement, a packet capture, or a live test? Where was it wrong?**

KI hat beim Formulieren geholfen. Die Aussagen zu IP-Adresse, Subnetz und
Standardroute habe ich mit dem
[Netzwerkauszug](../PreStudy/evidence/network-excerpt-2026-11-05.txt) abgeglichen.
Auf dem [MQTT-Screenshot](../PreStudy/evidence/mqtt-received.jpg) konnte ich die
empfangene Nachricht und die QoS-Einstellungen nachsehen.

## Help and extra work

**Who helped you, and was it valuable? Who did you help? Did you present, implement, make a PR, or fix something crucial for the class?**

Für den Einstieg habe ich die Videos, die Netzwerkausgaben und den MQTT-Test
genutzt. Die Ergebnisse habe ich im [Pre-Study-Bericht](../PreStudy/README.md)
festgehalten.
