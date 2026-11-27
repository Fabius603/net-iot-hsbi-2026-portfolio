# Reflexion 1 – MQTT, Node-RED und Temperaturanzeige

## Take-away

**What do you want to remember? What would be your advice to your past self?**

Ich würde beim nächsten Flow zuerst prüfen, ob Topic und Nachrichteninhalt im
Debug-Fenster ankommen. Danach kann ich gezielt nachsehen, ob das Dashboard den
Wert übernimmt.

## What was good?

**Which measurement, circuit, or explanation worked well — and why?**

Jonas und ich haben den Temperatursensor angeschlossen. Nach dem Firmware-Update
haben wir die Werte im Debug-Fenster und im Dashboard gesehen. Auch die LED und
die Textausgabe auf dem Display ließen sich ansteuern.

## What was difficult?

**Where did you struggle? What was the root cause?**

Wir wollten den LED-Zustand zuerst selbst abfragen und umschalten. Das hat nicht
geklappt. Mit dem fertigen Baustein konnten wir weitermachen. Für das Dashboard
hatten wir zuerst die falsche Adresse genommen, vermutlich die des Brokers.

## Exploration and show stoppers

**Was there any good or fun “struggling” or exploration? Anything where you should reach out to peers or instructors?**

Wir hatten einen Flow für die LED eines anderen Pods vorbereitet. Nach dem
Einsammeln der Hardware konnten wir ihn nicht mehr ausprobieren. Den
Header-Overhead habe ich an einem theoretischen Beispiel berechnet. Die
Systemgeschichte blieb ein schriftlicher Entwurf.

## Interaction with peers and instructors

**Who did you work with, what did you learn from them, and what did you give back?**

Ich habe mit Jonas gearbeitet. Mit einem anderen Pod haben wir verglichen, wie die
Temperatursensoren auf Anfassen reagieren, und über das OTA-Update gesprochen.

## How did you verify AI suggestions?

**What did you ask an AI, and how did you check the answer against a datasheet, a measurement, a packet capture, or a live test? Where was it wrong?**

KI half beim Formulieren. Für Aussagen zum Temperatur-Flow hatte ich die
[Debug-Ausgabe](../Module01/pictures/02-debug-temperatur-2026-11-06.png) und das
[Dashboard](../Module01/pictures/01-dashboard-temperatur-2026-11-06.png) als
Belege. Für den vorbereiteten Flow zwischen den Pods und die Overhead-Rechnung gab
es keinen praktischen Gegencheck.

## Help and extra work

**Who helped you, and was it valuable? Who did you help? Did you present, implement, make a PR, or fix something crucial for the class?**

Der Professor hat die Firmware über WLAN aktualisiert und uns die richtige
Dashboard-Adresse gegeben. Danach konnten wir den Temperaturverlauf sehen.
