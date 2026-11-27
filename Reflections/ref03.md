# Reflexion 3 – Nebula und MQTT über das Overlay

## Take-away

**What do you want to remember? What would be your advice to your past self?**

Ich habe verstanden, wie zwei Knoten ohne manuelle Portweiterleitung
zueinanderfinden. Das Lighthouse vermittelt die Adressen. Im Router-Log konnte ich
sehen, wie der Tunnel nach dem ersten Kontakt über das Relay direkt zum Cluster
wechselte.

## What was good?

**Which measurement, circuit, or explanation worked well — and why?**

Die MQTT-Nachricht ging aus Node-RED über das Overlay zum Broker auf unserem
Router und zurück in die Debug-Ausgabe. Node-RED war beim
[Test von zu Hause](../Module03/evidence/remote-proof-zuhause-2026-11-21.txt)
auch über das Heim-WLAN erreichbar.

## What was difficult?

**Where did you struggle? What was the root cause?**

Beim Kopieren von `apk update` hatte ich nur `pk update` eingefügt. Das Interface
hatten wir zuerst `nebula12` genannt. In der Bundle-Konfiguration steht `nebula1`.
Der Name muss zu `tun.dev` passen. Zahlendreher in der Node-RED-Adresse haben uns
zusätzlich Zeit gekostet.

## Exploration and show stoppers

**Was there any good or fun “struggling” or exploration? Anything where you should reach out to peers or instructors?**

LuCI zeigte „Carrier: Absent“, obwohl das Interface erreichbar war. `ip -brief
addr show nebula1` zeigte die Overlay-Adresse. Zu Hause ging der Laptop zuerst
über die falsche Standardroute. Mit seiner Adresse aus dem Pod-LAN als Absender
erreichte er Node-RED.

## Interaction with peers and instructors

**Who did you work with, what did you learn from them, and what did you give back?**

Ich habe das Modul mit Jonas bearbeitet. Unsere Ergebnisse haben wir in diesem
Modul keinem anderen Pod gezeigt.

## How did you verify AI suggestions?

**What did you ask an AI, and how did you check the answer against a datasheet, a measurement, a packet capture, or a live test? Where was it wrong?**

Claude half bei Fragen zu NAT, zum Nebula-Bundle und zur MQTT-Fehlersuche. Die
Erklärungen habe ich mit der Router-Konfiguration, dem
[Tunnel-Log](../Module03/evidence/remote-proof-zuhause-2026-11-21.txt) und den
Messungen abgeglichen. `iperf3` zeigte rund 60 Mbit/s im LAN und 2,5 Mbit/s über
das Overlay. Die CPU-Last von Nebula stützt die Vermutung, dass der Router
mitbremst. Wie viel die WLAN-Verbindung zum Unterschied beigetragen hat, blieb
offen.

## Help and extra work

**Who helped you, and was it valuable? Who did you help? Did you present, implement, make a PR, or fix something crucial for the class?**

Der Professor hat uns geholfen, das Bundle per `croc` zu übertragen. Beim
MQTT-Test haben wir die Debug-Ausgabe verwendet, weil wir keine über MQTT
steuerbare LED zur Verfügung hatten.
