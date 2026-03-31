<img align="right" width="250" src="../images/Gematik_Logo_Flag_With_Background.png"/><br/>

**Inhaltsverzeichnis**

<!-- TOC -->

- [PoPP-Service Lokalisierung in den verschiedenen Umgebungen](#popp-service-lokalisierung-in-den-verschiedenen-umgebungen)
  - [Vorgabe für Primärsysteme im Feld](#vorgabe-für-primärsysteme-im-feld)
  - [Feste FQDN je Umgebung](#feste-fqdn-je-umgebung)
  - [Wichtige Hinweise](#wichtige-hinweise)
  - [Bezug zur Spezifikation](#bezug-zur-spezifikation)
  - [Zusammenfassung](#zusammenfassung)

<!-- /TOC -->

# PoPP-Service Lokalisierung in den verschiedenen Umgebungen

Dieses Kapitel beschreibt die DNS-basierte Lokalisierung des PoPP-Service durch Primärsysteme. Der
PoPP-Service ist gemäß folgendem FQDN-Schema zu adressieren:

`popp.<umgebungskürzel>.poppservice.de`

Als <umgebungskürzel> sind die Werte prod, test, ref oder dev vorgesehen.

## Vorgabe für Primärsysteme im Feld

Für den Einsatz eines Primärsystems in der echten Betriebsumgebung bei Leistungserbringern (LEI) ist
ausschließlich der produktive Endpunkt zu verwenden:

`popp.prod.poppservice.de`

Die Endpunkte dev, ref und test sind ausschließlich für Entwicklungs-, Integrations- und Testzwecke
vorgesehen und dürfen im Produktivbetrieb nicht verwendet werden.

## Feste FQDN je Umgebung

Damit sind folgende feste FQDNs je Umgebung vorgegeben:

| Umgebung             |             FQDN             | DNS-Records | Verwendungszweck              |
| :------------------- | :--------------------------: | :---------: | :---------------------------- |
| Produktiv (PU)       | [popp.prod.poppservice.de][] |   A, AAAA   | **Echtbetrieb bei LEI**       |
| Referenz (RU)        | [popp.ref.poppservice.de][]  |   A, AAAA   | Referenzumgebung              |
| Test (TU)            | [popp.test.poppservice.de][] |   A, AAAA   | Integrationstest / Abnahme    |
| Entwicklung (RU-DEV) | [popp.dev.poppservice.de][]  |   A, AAAA   | Entwicklung /Frühtestumgebung |

> **⚠️ Temporärer Hinweis zur Verwendung der RU-DEV**
>
> Der PoPP-Service wird voraussichtlich ab Mai 2026 in RU und RU-DEV unter den benannten FQDNs
> verfügbar sein. Schon jetzt ist es möglich, in der RU-DEV unter einer Interims-Adresse gegen den
> PoPP-Service zu testen. Diese Adresse ist per Allow-Listing erreichbar. Bei Interesse in die
> Allow-List aufgenommen zu werden, erstellen Sie bitte ein ANFTI2-Ticket im
> [gematik_Service_Portal].

## Wichtige Hinweise

- Konfigurierbare Umgebung
  - Hinterlegen Sie die Umgebung (ref/dev/test/prod) als Konfiguration. Der daraus resultierende
    FQDN ist `popp.<umgebungskürzel>.poppservice.de`.
  - Keine hartcodierten IP-Adressen verwenden; immer den FQDN auflösen.
- DNS-Auflösung
  - Unterstützen Sie A- (IPv4) und AAAA-Records (IPv6)
  - Verifizieren Sie, dass der FQDN in der gewünschten Umgebung auflösbar ist, bevor die Verbindung
    aufgebaut wird.
- Protokoll- und Sicherheitsaspekte
  - Halten Sie sich an die im Abschnitt
    [Sicherheitsvorgaben PoPP-Client](30_sicherheitsanforderungen.md) festgelegten Vorgaben. Dieser
    Abschnitt definiert ausschließlich die Host-Lokalisierung

## Bezug zur Spezifikation

Folgende Anforderungen in [gemSpec_PoPP_Service][] sind Pflichten des Anbieters PoPP-Service mit
Wirkung auf die PoPP-Clients:

- A_28810 – PoPP-Service – DNS Registrierung
  - Anbieterpflicht: Registrierung der Hosts und IP-Adressen gemäß
    popp.<umgebungskürzel>.poppservice.de für alle Außenschnittstellen (siehe Kap. 3.1).
  - Implikation für Primärsysteme: Verlassen Sie sich auf die FQDNs und implementieren Sie die
    Auflösung über DNS; keine Annahmen über feste IPs treffen.
- A_28811 – PoPP-Service – Auflösung der FQDNs
  - Anbieterpflicht: Sicherstellen, dass die FQDNs im Internet korrekt auflösbar sind.
  - Implikation für Primärsysteme: Prüfen Sie im Rahmen von Installation, Test und Betrieb die
    erfolgreiche DNS-Auflösung der oben genannten FQDNs (A und AAAA).

## Zusammenfassung

- FQDN-Bildung: `popp.<umgebungskürzel>.poppservice.de`.
- DNS-Auflösung funktioniert für A und AAAA.
- Keine IP-Hartcodierung; FQDN-gestützte Verbindungen.
- Endpunkte werden auf Basis des FQDNs adressiert.

[gematik_Service_Portal]: https://service.gematik.de/servicedesk/customer/portal/37
[gemSpec_PoPP_Service]: https://gemspec.gematik.de/docs/gemSpec/gemSpec_PoPP_Service/latest/
[popp.dev.poppservice.de]: https://popp.dev.poppservice.de
[popp.prod.poppservice.de]: https://popp.prod.poppservice.de
[popp.ref.poppservice.de]: https://popp.ref.poppservice.de
[popp.test.poppservice.de]: https://popp.test.poppservice.de
