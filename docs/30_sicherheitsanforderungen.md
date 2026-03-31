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
  
<br><br>
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
aufbauen.  
**<=**

_Hinweis zu A_27205 bzgl. OCSP: Eine OCSP-Response wird im Handshake vom
PoPP-Service mitgeliefert (Unterstützung von OCSP-Stapling [RFC 6066][])._
  
<br><br>
**A_28878 - PoPP-Client - Kein TLS Certificate Pinning**  
Das PS DARF NICHT TLS-Serverzertifikate des PoPP-Service pinnen, sondern prüft
das Zertifikat entsprechend A_27205*.  
**<=**

_Hinweis: Die Schlüssel und Zertifikate des PoPP-Service für die Rolle
TLS-Server werden regelmäßig gewechselt._
  
<br><br>
**A_27207 - PoPP-Client - Sichere Speicherung privater Schlüssel und Token**  
Das PS MUSS private Schlüssel (DPoP) sowie Token (Access Token, PoPP-Token) so
speichern, dass

- nur die Nutzer des PS diese verwenden können, die dafür im Rahmen des
  jeweiligen Anwendungsfalls berechtigt sind und
- nur die jeweilige Anwendung auf die Schlüssel zur Nutzung zugreifen kann,
  diese jedoch nicht für Nutzer des Systems frei auslesbar (bspw. im Dateisystem
  abgelegt) sind.

Dabei sollen vom Betriebssystem bereitgestellte Mechanismen, idealerweise unter
Verwendung von Hardware-Schlüsselspeichern (bspw. TPM) verwendet werden.  
**<=**

_Hinweis zu A_27207: Es wird davon ausgegangen, dass ein PS eine
Benutzerverwaltung umfasst und nur die Nutzer auf schützenswerte Artefakte
zugreifen können, die dazu berechtigt sind. Teile der in der Anforderung
genannten Artefakte werden rein formal vom ZETA Client verarbeitet statt vom
PoPP-Client._
  
<br><br>
**A_27209 - PoPP-Client - Umsetzung des ZETA Client**  
Das PS MUSS den ZETA Client umsetzen.  
**<=**

_Hinweis zu A_27209: Die gematik stellt eine Referenz-Implementierung für den
ZETA Client bereit für Android, iOS, Windows und MacOS._
  
<br><br>
**A_27212 - PoPP-Client - Nutzerinformation zu Kartenlesern**  
Das PS MUSS den Nutzer nach der Installation des PoPP-Client - bzw. nach dem
Update auf die PS-Version, die den PoPP-Client enthält - bei der ersten
Verwendung informieren, dass etwaige für die PoPP-Funktionalität genutzte
Standardkartenleser den gleichen Sicherheitsanforderungen unterliegen wie andere
IT-Geräte der LEI und solche Geräte aus vertrauenswürdigen Quellen bezogen und
sicher betrieben werden müssen.
Dabei muss zusätzlich auf die Hinweise der gematik,
die sich zum einen an Leistungserbringer und zum anderen an DVOs richten, verwiesen
werden, wobei die konkreten Seiten mit den folgenden URLs verlinkt werden müssen:

- [Informationen für Leistungserbringer][]
- [Informationen für IT-Dienstleister / DVOs][]

**<=**
  
<br><br>
**A_28873 - PoPP-Client - eGK-Lese-App**  
Das PS bzw. der PS-Hersteller MUSS, wenn für PoPP die eGK-Anbindung über ein
Smartphone unterstützt wird, dem Nutzer eine konkrete App vorgeben, die er dafür
auf dem Smartphone nutzen muss und dabei durchsetzen, dass diese App

- im Standard-App-Store des Betriebssystems verfügbar ist,
- von einem vertrauenswürdigen Hersteller bereitgestellt wird
  (bspw. dem PS-Hersteller selbst),
- hinsichtlich eGK-Anbindung genau nur die APDU-Pakete zwischen PS und eGK
  vermittelt und eine
- hinsichtlich Vertraulichkeit und Integrität geschützte beidseitig
  authentisierte Verbindung zum PS umsetzt.

**<=**
  
<br><br>
**A_28874 - PoPP-Client - Geschützte Verbindung zur eGK-Lese-App**  
Das PS MUSS, wenn es für PoPP die eGK-Anbindung über ein Smartphone unterstützt,
eine hinsichtlich Vertraulichkeit und Integrität geschützte beidseitig
authentisierte Verbindung zur auf dem Smartphone für die eGK-Anbindung
verwendeten App umsetzen.  
**<=**

_Hinweis: Die in A_28873* und A_28874* genannte beidseitige Authentisierung muss
nicht zwingend über vorab provisionierte Identitäten/Schlüssel/Geheimnisse
umgesetzt werden.
Eine Kopplung bei Inbetriebnahme (bspw. über ein im PS angezeigten QR-Code der
Kopplungsinformationen wie Password-authenticated key exchange (PAKE) oder
Schlüssel enthält und direkt von der eGK-Lese-App gescannt wird) ist ausreichend._
  
<br><br>
**A_28875 - PoPP-Client - eGK-Lese-App - Geräteprüfung durch App für eGK-Anbindung**  
Das PS MUSS, wenn es für PoPP die eGK-Anbindung über ein Smartphone unterstützt,
durchsetzen, dass diese App mittels Betriebssystem-Funktion abfragt, ob
- sich das Betriebssystem in einem Betriebszustand befindet, der den
  Anforderungen des Betriebssystemherstellers entspricht, also kein
  Rooting/Jailbreaking durchgeführt wurde,
- sie nicht in einer Entwicklungs-/Debug-Umgebung ausgeführt wird,
- sie nicht unter ungewöhnlichen Benutzerrechten gestartet ist,
- sie nicht auf einer veralteten Betriebssystemversion läuft

und im Negativfall dem Nutzer darstellen, welche Risiken für die Anbindung der
eGK von Versicherten bei einer Fortsetzung der Anwendung bestehen
(z. B. dass Daten von Versicherten offengelegt werden oder unberechtigte
Fernzugriffe auf die Versicherten-eGK stattfinden können) oder die Fortsetzung
unterbindet.  
**<=**
  
<br><br>
**A_27594 - PoPP-Client - Robustheit gegen Angriffe über manipulierte Smartcard
bei Standard-Kartenleser**  
Das PS MUSS sicherstellen, dass es robust ist gegenüber etwaigen Angriffen über
manipulierte Smartcards, die per Standard-Kartenleser angebunden sind, was auch
für die App zur Anbindung der eGK gilt, wenn das PS für PoPP die eGK-Anbindung
über ein Smartphone unterstützt.  
**<=**

_Hinweis zu A_27594*: Grundsätzlich kann von Personen versucht werden,
manipulierte Smartcards einzusetzen.
Daher muss mit der Übergabe unerwarteter (bspw. zu großer) Daten an der
Schnittstelle gerechnet werden.
Eine fachliche Verarbeitung der von der Karte übergebenen Daten ist lokal
im PoPP-Client gar nicht notwendig und sollte daher unterbleiben._
  
<br><br>
**A_28876 - PoPP-Client - Keine entfernte Anbindung der eGK über Versichertengeräte**  
Das PS DARF die eGK NICHT über Geräte von Versicherten über den PoPP-Client an
den PoPP-Service anbinden.  
**<=**

_Hinweis: Die Anbindung einer eGK über den PoPP-Client ist immer eine Anbindung
durch die LEI über Geräte der LEI.
Eine Anbindung der eGK an den PoPP-Service durch Versicherte über deren
Versichertengeräte für Remote-Szenarien ist möglich und erfolgt über ein
PoPP-Modul entsprechend gemSpec_PoPP_Modul, welches in der vom Versicherten
genutzten App integriert ist._

[CAB Forum]:https://cabforum.org/

[Informationen für IT-Dienstleister / DVOs]:https://fachportal.gematik.de/telematikinfrastruktur/komponenten-dienste/popp#c13109

[Informationen für Leistungserbringer]:https://www.gematik.de/telematikinfrastruktur/ti-2-0#c13097

[RFC 6066]:https://datatracker.ietf.org/doc/html/rfc6066
