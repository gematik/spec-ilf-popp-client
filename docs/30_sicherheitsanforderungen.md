<img align="right" width="250" src="../images/Gematik_Logo_Flag_With_Background.png"/><br/>

**Inhaltsverzeichnis**

<!-- TOC -->
* [Sicherheitsvorgaben PoPP-Client](#sicherheitsvorgaben-popp-client)
<!-- TOC -->

# Sicherheitsvorgaben PoPP-Client

Dieser Abschnitt beschreibt die zentralen Sicherheitsanforderungen an
Primärsysteme im Zusammenhang mit dem PoPP-Client.
Dazu gehören Vorgaben für die sichere Kommunikation (TLS), die geschützte
Speicherung sensibler Daten, die Integration des ZETA Clients, die
Information der Nutzer über Kartenleser sowie Schutzmaßnahmen gegen Angriffe
durch manipulierte Smartcards.

**A_27205 - PoPP-Client - TLS-Verbindungsaufbau**  
Das PS MUSS beim TLS-Verbindungsaufbau das TLS-Server-Zertifikat des
PoPP-Service wie folgt prüfen:

- Prüfung, dass das Zertifikat von einem Herausgeber, der Mitglied im [CAB Forum][]
  ist, ausgestellt wurde,
- Validierung, dass der aufgerufene hostname im Zertifikat aufgeführt ist
  (Hostnamevalidierung),
- OCSP Status "good",
- Signatur OCSP-Response auf CA aus [CAB Forum][] rückführbar

und nur im Falle einer erfolgreichen, positiven Prüfung die TLS-Verbindung
aufbauen. **<=**

_Hinweis zu A_27205 bzgl. OCSP: Eine OCSP-Response wird im Handshake vom
PoPP-Service mitgeliefert (Unterstützung von OCSP-Stapling [RFC 6066][])._

**A_27207 - PoPP-Client - Sichere Speicherung privater Schlüssel und Token**  
Das PS MUSS private Schlüssel (DPoP) sowie Token (Access Token, PoPP-Token) so
speichern, dass

- nur die Nutzer des PS diese verwenden können, die dafür im Rahmen des
  jeweiligen Anwendungsfalls berechtigt sind und
- nur die jeweilige Anwendung auf die Schlüssel zur Nutzung zugreifen kann,
  diese jedoch nicht für Nutzer des Systems frei auslesbar (bspw. im Dateisystem
  abgelegt) sind.

Dabei sollen vom Betriebssystem bereitgestellte Mechanismen, idealerweise unter
Verwendung von Hardware-Schlüsselspeichern (bspw. TPM) verwendet werden. **<=**

_Hinweis zu A_27207: Es wird davon ausgegangen, dass ein PS eine
Benutzerverwaltung umfasst und nur die Nutzer auf schützenswerte Artefakte
zugreifen können, die dazu berechtigt sind. Teile der in der Anforderung
genannten Artefakte werden rein formal vom ZETA Client verarbeitet statt vom
PoPP-Client._

**A_27209 - PoPP-Client - Umsetzung des ZETA Client**  
Das PS MUSS den ZETA Client umsetzen. **<=**

_Hinweis zu A_27209: Die gematik stellt eine Referenz-Implementierung für den
ZETA Client bereit für Android, iOS, Windows und MacOS._

**A_27212 - PoPP-Client - Nutzerinformation zu Kartenlesern**  
Das PS MUSS den Nutzer nach der Installation des PoPP-Client - bzw. nach dem
Update auf die PS-Version, die den PoPP-Client enthält - bei der ersten
Verwendung informieren, dass etwaige für die PoPP-Funktionalität genutzte
Standardkartenleser den gleichen Sicherheitsanforderungen unterliegen wie andere
IT-Geräte der LEI und solche Geräte aus vertrauenswürdigen Quellen bezogen
werden müssen. **<=**

<!-- afi: Ich habe die folgende AFO gestrichen. Der PoPP-Service führt alle
erforderlichen Prüfungen durch. Ich wüsste auch nicht, wie der PoPP-Client
hier sinnvoll prüfen könnte. Mir fällt dazu nur Folgendes ein:
1. alle Antwort APDU verwerfen, die mehr als 65538 Byte groß sind. -->

**A_27594 - PoPP-Client - Robustheit gegen Angriffe über manipulierte Smartcard
bei Standard-Kartenleser**  
Das PS MUSS sicherstellen, dass es robust ist gegenüber etwaigen Angriffen über
manipulierte Smartcards, die per Standard-Kartenleser angebunden sind. **<=**

_Hinweis zu A_27594*: Grundsätzlich kann von Personen versucht werden,
manipulierte Smartcards einzusetzen.
Daher muss mit der Übergabe unerwarteter Daten an der Schnittstelle gerechnet
werden.
Eine fachliche Verarbeitung der von der Karte übergebenen Daten ist lokal
im PoPP-Client gar nicht notwendig und sollte daher unterbleiben._

[CAB Forum]:https://cabforum.org/
[RFC 6066]:https://datatracker.ietf.org/doc/html/rfc6066
