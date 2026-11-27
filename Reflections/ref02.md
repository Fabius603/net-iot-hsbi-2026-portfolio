# Reflexion 2 – OpenWrt, DHCP und NAT

## Take-away

**What do you want to remember? What would be your advice to your past self?**

Die DHCP-Vergabe konnte ich zum ersten Mal Schritt für Schritt im Mitschnitt
verfolgen. Bei der Reservierung blieb der Laptop DHCP-Client und bekam nach der
Umstellung die vorgesehene Adresse `.50`.

## What was good?

**Which measurement, circuit, or explanation worked well — and why?**

Nach der Verkleinerung des LANs auf `/26` blieben Router und Broker unter
derselben Adresse erreichbar. Zu Hause funktionierte der Router auch mit einem
anderen Uplink weiter.

## What was difficult?

**Where did you struggle? What was the root cause?**

Nach dem Flashen haben wir die Versionsnummer zunächst nicht gefunden. In LuCI
steht der Firmwarestand direkt unter „Status → Overview“. Dort würde ich beim
nächsten Mal zuerst nachsehen.

## Exploration and show stoppers

**Was there any good or fun “struggling” or exploration? Anything where you should reach out to peers or instructors?**

Beim NAT-Test haben wir die Portweiterleitung kurz aktiviert und danach entfernt.
Über den WAN-Port `11883` kam die MQTT-Testnachricht beim Broker an. Nach dem
Entfernen der Regel war eine neue Verbindung wieder blockiert.

## Interaction with peers and instructors

**Who did you work with, what did you learn from them, and what did you give back?**

Mit Jonas habe ich die ersten vier Aufgaben in der Vorlesung bearbeitet. Die
Aufgaben 5 bis 9 haben wir zu Hause gemacht.

## How did you verify AI suggestions?

**What did you ask an AI, and how did you check the answer against a datasheet, a measurement, a packet capture, or a live test? Where was it wrong?**

KI half beim Formulieren. Die Erklärungen zu DHCP, ARP, DNS und MQTT habe ich mit
dem [LAN-Mitschnitt](../Module02/evidence/lan-protocols-2026-11-14.pcap)
abgeglichen. Beim NAT-Test zeigte die
[Verbindungstabelle](../Module02/evidence/nat-open-test-2026-11-14.json) die
Zuordnung vom WAN-Port `11883` zum Broker-Port `1883`.

## Help and extra work

**Who helped you, and was it valuable? Who did you help? Did you present, implement, make a PR, or fix something crucial for the class?**

Als wir den Firmwarestand nach dem Flashen nicht fanden, haben wir den Professor
gefragt. Danach konnten wir mit der Einrichtung weitermachen.
