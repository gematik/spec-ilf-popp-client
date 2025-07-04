<img align="right" width="250" height="47" src="../images/Gematik_Logo_Flag_With_Background.png"/><br/>

**Inhaltsverzeichnis**

<!-- TOC -->
* [Versorgungsszenarien](#versorgungsszenarien)
  * [PoPP-Token bei physischer Anwesenheit in der LEI – eGK (UC_PoPP_1c)](#popp-token-bei-physischer-anwesenheit-in-der-lei--egk-uc_popp_1c)
  * [PoPP-Token bei physischer Anwesenheit außerhalb der LEI – eGK (UC_PoPP_2c)](#popp-token-bei-physischer-anwesenheit-außerhalb-der-lei--egk-uc_popp_2c)
  * [Sektor-spezifische Ausprägungen](#sektor-spezifische-ausprägungen)
    * [Patientenbasierte Systeme](#patientenbasierte-systeme)
    * [Fallbasierte Systeme](#fallbasierte-systeme)
    * [Warenwirtschaftssysteme](#warenwirtschaftssysteme)
<!-- TOC -->

# Versorgungsszenarien

In der ersten Einführungsstufe des PoPP-Services sind Sie als Hersteller von
Primärsystemen lediglich verpflichtet, zwei von insgesamt zehn in der
PoPP-Service-Spezifikation beschriebenen Versorgungsszenarien zu unterstützen.

Konkret betrifft dies die Szenarien, in denen der Nachweis des
Versorgungskontexts über die elektronische Gesundheitskarte (eGK) erfolgt –
sowohl bei physischer Anwesenheit des Versicherten in Ihrer Einrichtung
(UC_PoPP_1c) als auch bei mobiler Versorgung außerhalb der LEI (UC_PoPP_2c).

Die übrigen Versorgungsszenarien, etwa solche mit Nutzung der GesundheitsID oder
alternativen Check-in-Verfahren, müssen in dieser ersten Stufe noch nicht von
Ihrem System abgedeckt werden. Dies ermöglicht es Ihnen, sich zunächst auf die
technisch etablierten und organisatorisch am häufigsten nachgefragten Prozesse
zu konzentrieren und die PoPP-Integration schrittweise vorzunehmen.

Über die verpflichtende Umsetzung weiterer Versorgungsszenarien werden Sie von
der gematik zu einem späteren Zeitpunkt informiert.

## PoPP-Token bei physischer Anwesenheit in der LEI – eGK (UC_PoPP_1c)

In diesem Versorgungsszenario erscheint der Versicherte persönlich in der
Leistungserbringerinstitution (LEI), beispielsweise in einer Arztpraxis, einer
Apotheke oder einem Krankenhaus. Der Nachweis des Versorgungskontexts erfolgt
durch die Authentifizierung der elektronischen Gesundheitskarte (eGK) des
Versicherten – es
wird also die Karte, nicht der Versicherte selbst, geprüft.
Das Primärsystem übernimmt dabei folgende Aufgaben:

1. Es initiiert den Check-in-Prozess, indem es die eGK des Versicherten über ein
   geeignetes Kartenlesegerät einliest.
2. Über den integrierten PoPP-Client wird eine Anfrage an den PoPP-Service
   gestellt, um ein PoPP-Token für den aktuellen Versorgungskontext zu erhalten.
3. Im Rahmen dieses Prozesses wird die Echtheit und Gültigkeit der eGK vom
   PoPP-Service geprüft, wobei das PS die Kartennachrichten von und zur eGK weiterreichen muss.
4. Nach erfolgreicher Authentifizierung und Prüfung stellt der PoPP-Service das
   PoPP-Token aus und übermittelt es an den PoPP-Client zur weiteren Verwendung
   innerhalb des Primärsystems.
5. Das PoPP-Token kann anschließend vom Primärsystem als Autorisierungsnachweis bei
   Zugriffen auf die TI-Fachdienste (z. B. ePA für Alle, E-Rezept) genutzt werden.

## PoPP-Token bei physischer Anwesenheit außerhalb der LEI – eGK (UC_PoPP_2c)

In diesem Szenario findet die Versorgung des Versicherten außerhalb der
Räumlichkeiten der LEI statt, etwa bei einem Hausbesuch oder einer mobilen
Versorgung durch das Personal der LEI. Auch hier erfolgt der Nachweis des
Versorgungskontexts durch die Authentifizierung der eGK des Versicherten – es
wird also die Karte, nicht der Versicherte selbst, geprüft.
Das mobile Primärsystem übernimmt folgende
Schritte:

1. Das mobile Primärsystem nutzt ein geeignetes mobiles Kartenlesegerät, über
   das die eGK des Versicherten eingelesen wird.
2. Der PoPP-Client im Primärsystem stellt eine Anfrage an den PoPP-Service, um
   ein PoPP-Token für den Versorgungskontext zu erzeugen.
3. Im Rahmen dieses Prozesses wird die Echtheit und Gültigkeit der eGK vom
   PoPP-Service geprüft, wobei das PS die Kartennachrichten von und zur eGK weiterreichen muss.
4. Nach erfolgreicher Authentifizierung und Prüfung stellt der PoPP-Service das
   PoPP-Token aus und übermittelt es an den PoPP-Client zur weiteren Verwendung
   innerhalb des Primärsystems.
5. Das PoPP-Token kann anschließend vom Primärsystem als Autorisierungsnachweis bei
   Zugriffen auf die TI-Fachdienste (z. B. ePA für Alle, E-Rezept) genutzt werden.

## Sektor-spezifische Ausprägungen

In den verschiedenen Sektoren wird der PoPP-Token unterschiedlich verwendet.
Der PoPP-Token wird immer vom PoPP-Client entgegengenommen und dann innerhalb
des Primärsystems an die Stelle weitergegeben, die für die Speicherung oder
weitere Verwendung zuständig ist.

### Patientenbasierte Systeme

Bei patientenbasierten Systemen wie Praxisverwaltungssystemen (PVS) wird der
PoPP-Token dem Patientenstammblatt oder dem Patientenfach zugeordnet und dort
abgelegt.

### Fallbasierte Systeme

Bei fallbasierten Systemen wie Krankenhausinformationssystemen KIS wird der
PoPP-Token dem entsprechenden Fall (z. B. einer Fallakte oder Behandlungsakte)
zugeordnet und dort abgelegt.

### Warenwirtschaftssysteme

Bei Warenwirtschaftssystemen wie den Apothekenverwaltungssystemen AVS wird der
PoPP-Token dem jeweiligen Kassenvorgang zugeordnet und gespeichert.
