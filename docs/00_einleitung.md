<img align="right" width="250" src="../images/Gematik_Logo_Flag_With_Background.png"/><br/>

**Inhaltsverzeichnis**

<!-- TOC -->
* [Struktur und Inhalt des PoPP-Client Implementierungsleitfadens](#struktur-und-inhalt-des-popp-client-implementierungsleitfadens)
* [Einleitung](#einleitung)
* [Hintergrund und Einordnung](#hintergrund-und-einordnung)
* [Infopunkte](#infopunkte)
  * [Was ist der PoPP-Service?](#was-ist-der-popp-service)
  * [Was ist ein PoPP-Token?](#was-ist-ein-popp-token)
  * [Was ist der PoPP-Client?](#was-ist-der-popp-client)
  * [Zero Trust in der TI 2.0](#zero-trust-in-der-ti-20)
  * [Versorgungskontext und Behandlungskontext](#versorgungskontext-und-behandlungskontext)
* [PoPP-Service: Bereitstellung in Stufen](#popp-service-bereitstellung-in-stufen)
  * [Stufe 1:](#stufe-1)
  * [Stufe 2:](#stufe-2)
<!-- TOC -->

# Struktur und Inhalt des PoPP-Client Implementierungsleitfadens

Diese [Einleitung](00_einleitung.md) liefert eine Einordnung von PoPP und gibt Orientierung über das Gesamt-Projekt PoPP in zwei Stufen.

Der [Systemüberblick](10_systemueberblick.md) beschreibt die Rolle und den Aufbau von Primärsystemen im Kontext des PoPP-Services, die Integration neuer Komponenten und Schnittstellen, die Vorteile der Lösung sowie die Abläufe und Interaktionen im Versorgungskontext.

Der Abschnitt [Versorgungsszenarien](20_versorgungsszenarien.md) beschreibt die aktuell verpflichtenden Anwendungsfälle für den PoPP-Service, erläutert deren technische Abläufe sowie die systemtypischen Unterschiede bei der Verwendung und Speicherung des PoPP-Tokens.

Der Abschnitt [Sicherheitsvorgaben PoPP-Client](30_sicherheitsanforderungen.md) beschreibt die zentralen Sicherheitsanforderungen an Primärsysteme im Zusammenhang mit dem PoPP-Client.  

Der Abschnitt [Funktionsmerkmale](40_funktionsmerkmale.md) beschreibt die zentralen Funktionen des PoPP-Clients, insbesondere die Abläufe zur PoPP-Token-Generierung mit verschiedenen Kartenlesern.
Detaillierte Implementierungsdetails dazu finden sich im Abschnitt zur [eGK](../docs/eGK/00_uebersicht.md). 
Es wird auf die Verwendung von ZETA Clients hingewiesen und zur PoPP-Beispielimplementierung informiert.

# Einleitung

Mit der Einführung des Proof of Patient Presence (PoPP)-Service in der
Telematikinfrastruktur 2.0 kommen auf Primärsysteme (PS) in
Leistungserbringerinstitutionen neue Aufgaben zu. 

Dieser
Implementierungsleitfaden möchte Sie dabei
unterstützen, die PoPP-Anforderungen praxisnah und effizient umzusetzen. Im
Mittelpunkt stehen dabei die Implementierung des PoPP-Clients sowie die
reibungslose Kommunikation mit dem PoPP-Service entsprechend den Vorgaben der
gematik.

Dieses Dokument richtet sich insbesondere an technische Produktmanager:innen,
Architekt:innen und Entwickler:innen von Primärsystemen, die ihre Systeme an den PoPP-Service der TI 2.0 anbinden. 
Es bietet
einen verständlichen Überblick über die wichtigsten fachlichen und technischen
Anforderungen, stellt Versorgungsszenarien vor und gibt hilfreiche Tipps für
eine erfolgreiche Implementierung und Integration.

Die Inhalte orientieren sich an der aktuellen PoPP-Service-Spezifikation der
gematik und konzentrieren sich bewusst auf die für Sie
relevanten Themen. Andere Bereiche wie Anforderungen an PoPP-Module
für Apps und Browser-Anwenungen oder der Betrieb des zentralen PoPP-Service sind klar
abgegrenzt. 

Ziel ist es, alle wichtigen Aspekte kompakt, verständlich und mit Blick auf
die praktische Umsetzung aufzubereiten – damit der Einstieg und die Integration
möglichst leichtfallen.

# Hintergrund und Einordnung

Der Proof of Patient Presence (PoPP)-Service spielt eine zentrale Rolle in der
Telematikinfrastruktur 2.0. Er ermöglicht es, zuverlässig und sicher
nachzuweisen, dass ein Versicherter zu einem bestimmten Zeitpunkt in einem
definierten Versorgungskontext mit einer Leistungserbringerinstitution (LEI)
zusammentrifft. Dieser Nachweis wird mithilfe des sogenannten PoPP-Tokens
erbracht, das kryptografisch gesichert vom PoPP-Service ausgestellt wird. Das
PoPP-Token ist zukünftig die Grundlage für den Zugriff auf verschiedene
Fachanwendungen der TI – etwa die elektronische Patientenakte (ePA für Alle),
das E-Rezept oder das Versichertenstammdatenmanagement (VSDM 2.0).

Für Sie ergibt sich daraus die Aufgabe, ihre Systeme so
weiterzuentwickeln, dass Sie die neuen PoPP-Prozesse optimal unterstützen und
die erforderlichen Schnittstellen zum PoPP-Service bereitstellen. Das
Primärsystem wird um einen PoPP-Client erweitert,
welcher die Kommunikation mit dem PoPP-Service übernimmt.

Zusätzlich sind weitere Bausteine wie der ZETA Client zu integrieren, um eine
sichere Authentifizierung und Kommunikation nach modernen Zero-Trust-Prinzipien
zu gewährleisten.

Die Erweiterung des Primärsystems um einen PoPP-Client ist ein wichtiger Schritt,
um Interoperabilität und Sicherheit innerhalb der TI 2.0 weiter zu stärken.
Die korrekte 
Umsetzung der PoPP-Prozesse trägt zur Sicherheit beim Zugriff von Leistungserbringern 
auf die Fachanwendungen der TI bei.
In den folgenden Abschnitten dieses Dokumentes finden Sie die wichtigsten
Versorgungsszenarien, Anforderungen und empfohlene Umsetzungsschritte aus Ihrer
Sicht – mit dem Ziel, Sie bestmöglich bei der erfolgreichen Integration zu
unterstützen.

# Infopunkte

## Was ist der PoPP-Service?

Der PoPP-Service (Proof of Patient Presence) ist ein zentraler Plattformdienst
der Telematikinfrastruktur, der dabei unterstützt, den Versorgungskontext
zwischen einem Versicherten und einer Leistungserbringerinstitution (LEI) sicher
und nachvollziehbar nachzuweisen. Er stellt ein kryptografisch gesichertes
PoPP-Token aus, das bestätigt, dass ein Versicherter zu einem bestimmten
Zeitpunkt tatsächlich im Versorgungskontext mit einer LEI steht. Dieses
PoPP-Token wird künftig die Voraussetzung für den Zugriff auf zahlreiche
TI-Fachdienste wie die elektronische Patientenakte (ePA für Alle), das E-Rezept
oder das Versichertenstammdatenmanagement (VSDM 2.0) sein.
Die Kommunikation mit dem PoPP-Service erfolgt über speziell integrierte
Komponenten im Primärsystem, insbesondere den PoPP-Client und den ZETA Client.
Diese sorgen dafür, dass alle Prozesse sicher und reibungslos ablaufen.

## Was ist ein PoPP-Token?

Das PoPP-Token ist ein digitaler Nachweis, der vom PoPP-Service ausgestellt und
kryptografisch gesichert wird. Es enthält alle relevanten Informationen darüber,
dass ein bestimmter Versicherter zu einem bestimmten Zeitpunkt mit einer
bestimmten LEI in einem Versorgungskontext steht. Das PoPP-Token ist damit der
Schlüssel für den Zugang zu verschiedenen TI-Fachdiensten und ermöglicht eine
sichere, transparente und nachvollziehbare Autorisierung.

## Was ist der PoPP-Client?

Der PoPP-Client ist Teil des Primärsystems und übernimmt die Kommunikation mit
dem PoPP-Service. Er ist verantwortlich für das Anfordern und Empfangen von
PoPP-Token sowie für die Umsetzung der fachlichen Abläufe beim Check-in von
Versicherten mit eGK oder GesundheitsID.

## Zero Trust in der TI 2.0

Zero Trust ist ein modernes Sicherheitskonzept, das davon ausgeht, dass
grundsätzlich kein System oder Nutzer – egal ob innerhalb oder außerhalb des
eigenen Netzwerks – automatisch vertrauenswürdig ist. In der TI 2.0 wird dieses
Prinzip durch den Einsatz des ZETA Guard (am Dienst) und des ZETA Clients (in
der Client-Komponente) sowie weiterer ZETA Dienste konsequent
umgesetzt. Jede Kommunikation und jeder Zugriff auf den PoPP-Service wird
individuell geprüft und autorisiert, unabhängig davon, aus welchem Netzwerk die
Anfrage stammt. So wird die Sicherheit und Integrität der TI nachhaltig
gestärkt.

## Versorgungskontext und Behandlungskontext

Der Versorgungskontext entsteht, sobald ein Versicherter mit einer
Leistungserbringerinstitution (LEI) in Kontakt tritt – zum Beispiel beim
Betreten einer Praxis oder Apotheke.
Er stellt das Potenzial dar, dass im weiteren Verlauf eine konkrete Behandlung
oder Leistung erfolgen kann.
Erst wenn tatsächlich eine medizinische Maßnahme, Beratung oder Abgabe
stattfindet, spricht man vom Behandlungskontext.
Der Behandlungskontext ist somit eine spezifische Ausprägung des allgemeinen
Versorgungskontexts.
Eine weitere Ausprägung wäre ein Pflegekontext in häuslicher oder stationärer
Pflege.

# PoPP-Service: Bereitstellung in Stufen

Die Bereitstellung des PoPP-Service erfolgt in zwei Stufen, die im Abstand von
etwa einem halben Jahr vorgesehen sind.

## Stufe 1:

Der **PoPP-Service Stufe 1** beinhaltet die Erstellung von PoPP-Token mittels
einer eGK in Versorgungsszenarien bei dem Leistungserbringer und Versicherte
an einem Ort physisch zusammentreffen.
Dies entspricht den Use Cases UC_PoPP_1c und UC_PoPP_2c aus
[gemSpec_PoPP_Service][].
Die Erläuterung der Versorgungsszenarien für die Primärsystem-Sicht erfolgt
im Kapitel [Versorgungsszenarien](20_versorgungsszenarien.md).

Plandatum für die Verfügbarkeit im Feld von Stufe 1: **30.06.2026**

## Stufe 2:

Der **PoPP-Service Stufe 2** beinhaltet zusätzlich die Erstellung von PoPP-Token
in Versorgungsszenarien, bei denen von Versichertenseite die GesundheitsID
verwendet wird sowie Versorgungsszenarien mit der eGK in der Fernversorgung.
Diese Versorgungsszenarien sind noch nicht Teil dieses Leitfadens; sie werden
später ergänzt.

Plandatum für die Verfügbarkeit im Feld von Stufe 2: **Q4 2026** 

[gemSpec_PoPP_Service]:https://gemspec.gematik.de/prereleases/Draft_PoPP_25_1/gemSpec_PoPP_Service_V1.0.0_CC2/
