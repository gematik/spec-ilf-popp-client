<img align="right" width="250" src="../../images/Gematik_Logo_Flag_With_Background.png"/><br/>

**Inhaltsverzeichnis**

<!-- TOC -->
* [Vorbedingung](#vorbedingung)
* [Verbindung zum PoPP-Service aufbauen](#verbindung-zum-popp-service-aufbauen)
* [Nachbedingung](#nachbedingung)
<!-- TOC -->

# Vorbedingung

Für die beschriebenen Arbeitsschritte ist es erforderlich, dass der PoPP-Client
eine _StartMessage_ gemäß [I_PoPP_Token_Generation.yaml][] erzeugt hat.
Die Beschreibung dazu findet sich
[hier](10_egk-stecken.md#informationen-für-die-startmessage).

# Verbindung zum PoPP-Service aufbauen

Dieses Dokument befasst sich nur rudimentär mit dem Aufbau einer Verbindung 
zum PoPP-Service.
Details finden sich in [gemSpec_PoPP_Service][].
Wegen der vereinfachten Sichtweise und der hier betrachteten Aufgabe "hole 
ein PoPP-Token vom PoPP-Service und verwende dazu eine in der 
Leistungserbringerinstitution gesteckte eGK" lässt sich der Verbindungsaufbau
recht einfach beschreiben:

1. Erstelle eine WebSocket Verbindung zum PoPP-Service.
2. Sende die _StartMessage_ über diese WebSocket Verbindung zum PoPP-Service.

# Nachbedingung

Zum Schluss der hier betrachteten Aktivitäten hat der PoPP-Client eine 
_StartMessage_ gemäß [I_PoPP_Token_Generation.yaml][] an den PoPP-Service 
geschickt.

[I_PoPP_Token_Generation.yaml]:https://github.com/gematik/api-popp/blob/main/src/openapi/I_PoPP_Token_Generation.yaml

[gemSpec_PoPP_Service]:https://gemspec.gematik.de/prereleases/Draft_PoPP_25_1/gemSpec_PoPP_Service_V1.0.0_CC2/
