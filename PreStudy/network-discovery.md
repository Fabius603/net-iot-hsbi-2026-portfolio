# Netzwerk am Laptop untersuchen

**05.11.2026 · Windows-Laptop · Einzelarbeit**

Ich habe mir die IP-Konfiguration und Routingtabelle meines Laptops angesehen
und danach den Weg zu `1.1.1.1` mit `tracert` geprüft.

## Verwendete Befehle

```powershell
ipconfig /all
route print
tracert -d -w 300 -h 30 1.1.1.1
```

Mit `-d` werden die IP-Adressen nicht in Namen aufgelöst. `-w 300` begrenzt
die Wartezeit je Antwort auf 300 ms, `-h 30` die Anzahl der Hops auf 30.
Durch den kurzen Timeout können auch verspätete Antworten fehlen.

Die Aufzeichnung begann um 11:46:57 Uhr. Hier liegen die Ausgaben:

- [IP-Konfiguration und Routingtabelle als Auszug](evidence/network-excerpt-2026-11-05.txt)
- [Traceroute-Ausgabe](evidence/tracert-2026-11-05.txt)

Die Textdateien sind gekürzt und einheitlich formatiert. Persönliche Angaben
und die übrigen Netzwerkadapter sind im Auszug weggelassen.

## IP-Adresse und Standardgateway

| Einstellung | Wert |
| --- | --- |
| IPv4-Adresse | `172.20.10.3` |
| Subnetzmaske | `255.255.255.240` (`/28`) |
| Standardgateway | `172.20.10.1` |
| DHCP-Server | `172.20.10.1` |

Bei `/28` bleiben vier Bits für den Hostanteil. Das ergibt `2^4 = 16`
Adressen: die Netzadresse `172.20.10.0`, die Broadcast-Adresse `172.20.10.15`
und 14 nutzbare Adressen von `.1` bis `.14`.

In der Routingtabelle stand dazu diese Standardroute:

```text
Netzwerkziel  Netzwerkmaske  Gateway      Schnittstelle
0.0.0.0      0.0.0.0        172.20.10.1  172.20.10.3
```

Die Verbindung lief über WLAN. Für `1.1.1.1` gab es keine speziellere Route,
also verwendete der Laptop das Gateway `172.20.10.1`. Das eigene Netz
`172.20.10.0/28` liegt im privaten Adressbereich `172.16.0.0/12`.

## Traceroute zu 1.1.1.1

| Hop | Antwort | Laufzeiten (RTT) |
| --- | --- | --- |
| 1 | `172.20.10.1` | 3, 2, 2 ms |
| 2–12 | Keine Antwort innerhalb des Timeouts | jeweils `* * *` |
| 13 | `1.1.1.1` | 22, 21, 21 ms |

Das Ziel antwortete bei Hop 13, also nach zwölf Zwischenstationen laut
Trace. Die genaue Zahl physischer Router lässt sich daraus wegen möglicher
Tunnel nicht sicher ablesen.

## Stolperstelle

Bei vielen Zwischenstationen stand nur `* * *`. Dadurch konnte ich nicht
sehen, wo der Übergang von privaten zu öffentlichen IP-Adressen lag.
Auch den Ort der NAT-Übersetzung kann ich aus dieser Ausgabe nicht ablesen.

Das Ziel war trotzdem erreichbar. Die Sterne zeigen nur, dass in der
Wartezeit keine Antwort kam. Warum die einzelnen Stationen nicht
antworteten, kann ich aus der Ausgabe nicht erkennen.
Die RTT von rund 21–22 ms am Ziel gilt für Hin- und Rückweg zusammen;
über die verfügbare Datenrate sagt sie nichts aus.
