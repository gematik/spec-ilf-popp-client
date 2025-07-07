<img align="right" width="250" src="../../images/Gematik_Logo_Flag_With_Background.png"/><br/>

**Inhaltsverzeichnis**

<!-- TOC -->
* [eGK stecken](#egk-stecken)
* [Vorbedingungen](#vorbedingungen)
* [Trigger "PoPP-Token abrufen"](#trigger-popp-token-abrufen)
* [Informationen für die StartMessage](#informationen-für-die-startmessage)
* [Kontaktloser Standard-Kartenleser](#kontaktloser-standard-kartenleser)
* [Kontaktloses eHealth-Kartenterminal](#kontaktloses-ehealth-kartenterminal)
* [Nachbedingung](#nachbedingung)
<!-- TOC -->

# eGK stecken

Dieses Dokument befasst sich mit

1. [Vorbedingungen](#vorbedingungen),
   die erfüllt sein müssen, damit ein PoPP-Client  eine eGK nutzen kann,
2. welche [Optionen](#trigger-popp-token-abrufen)
   es gibt den Vorgang "PoPP-Token abrufen" anzustoßen und
3. den [Informationen](#informationen-für-die-startmessage),
   welche der PoPP-Client sammelt, bevor er den PoPP-Service kontaktiert.

# Vorbedingungen

Damit ein PoPP-Client eine eGK nutzen kann, muss die
Leistungserbringerinstitution (LEI) über Kartenlesegeräte verfügen.
[gemSpec_PoPP_Service][] betrachtet dazu die folgenden Möglichkeiten:

1. die eGK wird in einen Standard-Kartenleser gesteckt, der "direkt" vom
   PoPP-Client genutzt wird.
2. die eGK wird in ein eHealth-Kartenterminal (eH-KT) gesteckt, welches über
   einen Konnektor nutzbar ist, auf den der PoPP-Client Zugriff hat.

Aus Sicht der TI 1.0 verfügen LEI über einen Konnektor und eH-KT, aber nicht
in allen Fällen über einen Standard-Kartenleser.
Perspektivisch ist mit Einführung der TI 2.0 damit zu rechnen, dass sich
eine signifikante Anzahl von LEI dafür entscheidet (von möglichen Ausnahmen
abgesehen) eGKs nur noch in Standard-Kartenlesern einzusetzen.
Falls eine LEI schon vor der Einführung von PoPP über Standard-Kartenleser
verfügt, dann werden diese jetzt für andere Zwecke genutzt und 
möglicherweise bleibt das auch nach der Einführung von PoPP so.

Aus diesen Überlegungen heraus wird ein möglichst universell einsetzbarer
PoPP-Client die in einer LEI verfügbare Kartenlesegeräte wie folgt behandeln:

1. An das [Primärsystem][] sind kein, ein oder mehrere Standard-Kartenleser
   angeschlossen.
2. Vielleicht alle, vielleicht nur einige oder keiner der verfügbaren
   Standard-Kartenleser werden für PoPP genutzt.
3. Das [Primärsystem][] hat über den Konnektor Zugriff auf kein, ein oder
   mehrere eH-KT.
4. Vielleicht alle, vielleicht nur einige oder keines der verfügbaren eH-KT
   wird für PoPP genutzt.
5. Jedes derzeit zugelassen eH-KT verfügt über zwei Kartenslots, die
   potenziell für PoPP nutzbar sind.
   Typischerweise wird nur einer dieser Kartenslots zum Stecken einer eGK
   genutzt.

Zusammenfassend lässt sich festhalten:

1. Potenziell hat der PoPP-Client Zugriff auf eine Reihe von
   Kartenlesegeräten, entweder "direkten" Zugriff auf
   Standard-Kartenleser oder über den Konnektor Zugriff auf eH-KT
   (wobei jedes eH-KT über mehrere Kartenslots verfügt).
2. Die Anzahl verfügbarer Kartenlesegeräte wird sich durch Zukauf oder
   Deinstallationen im Laufe der Zeit ändern.
3. Von den verfügbaren Kartenlesegeräten wird möglicherweise nur eine
   Untermenge für PoPP genutzt.
4. Die Untermenge der für PoPP genutzten Kartenlesegeräte wird sich
   möglicherweise durch Umwidmung ändern. Beispielsweise werde ein
   1. nicht für PoPP verwendetes Kartenlesegerät durch Umwidmung ab dem 
      Zeitpunkt X für PoPP verwendet oder
   2. ein Kartenlesegerät wird durch Umwidmung zum Zeitpunkt Y nicht mehr 
      für PoPP verwendet.

**Fazit Vorbedingung:**
Das [Primärsystem][] ist so konfiguriert, dass dessen PoPP-Client wenigstens
ein Kartenlesegerät (Standard-Kartenleser oder eHealth-Kartenterminal)
für PoPP nutzen kann.

# Trigger "PoPP-Token abrufen"

Dieser Abschnitt setzt voraus, dass der PoPP-Client (wie im Abschnitt
[Vorbedingungen](#vorbedingungen) beschrieben) mindestens ein,
in großen LEI mitunter auch sehr viele Kartenlesegeräte für PoPP nutzen kann.

Alle zugelassenen eH-KT und typische Standard-Kartenleser sind in der
Lage zu signalisieren, dass eine Karte gesteckt wurde.
Damit eröffnen sich für einen PoPP-Client die folgenden Möglichkeiten:
Der Vorgang "PoPP-Token abrufen" wird

1. automatisch gestartet, sobald eine Karte in ein Kartenlesegerät gesteckt
   wird, oder
2. manuell am [Primärsystem][] für ein bestimmtes Kartenlesegerät gestartet.
   Dabei ist es möglich, dass sich zum Startzeitpunkt im ausgewählten
   Kartenlesegerät bereits eine Karte befindet oder noch gesteckt werden muss.

In der Realität wird es Arbeitsplätze mit Kartenlesegeräten geben,

1. die ausschließlich für PoPP eingesetzt werden.
   Dort bietet es sich an den Vorgang "PoPP-Token abrufen" automatisch mit dem
   Stecken einer Karte zu starten.
2. die für andere Zwecke vorgesehen sind und nur selten für PoPP verwendet
   werden.
   Dort bietet es sich an den Vorgang "PoPP-Token abrufen" manuell zu starten.
3. die im Laufe der Zeit umgewidmet werden.
   Deshalb bietet es sich an im PoPP-Client eine Konfigurationsmöglichkeit
   vorzusehen, ob der Vorgang "PoPP-Token abrufen" beim Stecken einer Karte
   automatisch ausgelöst wird, oder manuell angestoßen wird.

**Fazit Trigger "PoPP-Token abrufen":**
Der PoPP-Client oder das Primärsystem bietet eine Konfigurationsmöglichkeit, über welche 
festgelegt wird, ob beim Stecken einer Karte in ein für PoPP vorgesehenes
Kartenlesegerät der Vorgang "PoPP-Token abrufen" automatisch startet oder
manuell am PoPP-Client angestoßen werden muss.

# Informationen für die StartMessage

Dieser Abschnitt setzt voraus, dass der Vorgang "PoPP-Token abrufen" 
gestartet wurde und deshalb die folgenden Informationen bekannt sind:  

1. Kartenlesegerät, welches für die Kommunikation mit der eGK verwendet wird.
   Das beinhaltet die Information, ob es sich um einen Standard-Kartenleser
   handelt oder um ein eH-KT.
2. Falls es sich um ein
   1. Standard-Kartenleser handelt, muss der PoPP-Client wissen, ob die eGK
      kontaktbehaftet (T=1 gemäß ISO/IEC 7816-3) oder kontaktlos (T=CL gemäß
      ISO/IEC 14443) angesprochen wird.
      Möglicherweise erhält der PoPP-Client aus einer zum PoPP-Client 
      gehörenden Konfigurationsdatei diese Information (T=1 oder T=CL).
      Automatisch erhält der PoPP-Client diese Information etwa dadurch, dass 
      er versucht den Inhalt der Datei [EF.GDO][] zu lesen.
      Gelingt dies unmittelbar nach Aktivierung der Karte nicht (Antwort-APDU 
      zum [READ BINARY][] Kommando enthält als Statuswort '6982' =
      SecurityStatusNotSatisfied), dann wird die eGK kontaktlos angesprochen,
      sonst kontaktbehaftet.
   2. Falls es sich um ein eH-KT handelt, dann wird der Konnektor durch 
      Aufruf der Methode [StartCardSession][] darauf 
      vorbereitet den Kartenslot des eH-KT mit der eGK für PoPP zu nutzen.
      Dabei übermittelt der Konnektor eine "clientSessionId".
      Aus den Informationen zum Kartenslot geht hervor, ob die eGK 
      kontaktbehaftet (T=1) oder kontaktlos (T=CL) angesprochen wird.

Nachdem der PoPP-Client wie in [gemSpec_PoPP_Service][] beschrieben eine 
WebSocket Verbindung zum PoPP-Service aufgebaut hat, wird als erste 
Nachricht über diese WebSocket Verbindung eine _StartMessage_ gemäß 
[I_PoPP_Token_Generation.yaml][] geschickt, welche unter anderem folgende 
Informationen enthält (**ACHTUNG:** Die folgende Abbildung ist gegenüber 
[I_PoPP_Token_Generation.yaml][] auf den hier wesentlichen Inhalt gekürzt):

```yaml
StartMessage:
  type: object
  title: "Start proof process using electronic health card (eHC)"
  properties:
    cardConnectionType:
      type: string
      enum:
        - "contact-standard"
        - "contact-connector"
        - "contactless-standard"
        - "contactless-connector"
    clientSessionId:
      type: string
```

Den Wert des Parameters "cardConnectionType" wählt der PoPP-Client entsprechend
zum für PoPP ausgewählten Kartenlesegerät.

Den Wert des Parameters "clientSessionId" hat der PoPP-Client vom Konnektor 
erhalten, falls ein eH-KT als Kartenlesegerät verwendet wird, oder der Wert 
dieses Parameters wird vom PoPP-Client aus dem Wertebereich erzeugt, der durch
[I_PoPP_Token_Generation.yaml][] dafür vorgegeben ist.

# Kontaktloser Standard-Kartenleser

Falls die eGK in einem Standard-Kartenleser steckt und dort kontaktlos
angebunden ist, dann muss der PoPP-Client einen PACE-Kanal zur eGK aufbauen.
Dazu benötigt der PoPP-Client die auf der eGK aufgedruckte sechsstellige CAN.
Der Aufbau eines PACE-Kanals wird im Kapitel
[Sessionkeys mittels symmetrischer Kartenverbindungsobjekte][PACE]
beschrieben.

# Kontaktloses eHealth-Kartenterminal

Falls die eGK in einem eH-KT steckt und dort kontaktlos angebunden ist, dann 
kümmert sich das eH-KT um den Aufbau des PACE-Kanals. 

# Nachbedingung

Zum Schluss der hier behandelten Aktivitäten

1. ist eine Karte in ein für PoPP vorgesehenes Kartenlesegerät gesteckt und
2. aktiviert und betriebsbereit und
3. der PoPP-Client hat eine _StartMessage_ gemäß
   [I_PoPP_Token_Generation.yaml][] erzeugt (der Versand der
   _StartMessage_ wird [hier](20_WebSocket.md) beschrieben).

[EF.GDO]:https://gemspec.gematik.de/docs/gemSpec/gemSpec_eGK_ObjSys_G2_1/latest/#5.3.6

[I_PoPP_Token_Generation.yaml]:https://github.com/gematik/api-popp/blob/main/src/openapi/I_PoPP_Token_Generation.yaml

[PACE]:https://gemspec.gematik.de/docs/gemSpec/gemSpec_COS/latest/#15.4.2

[Primärsystem]:https://fachportal.gematik.de/hersteller-anbieter/primaersysteme

[READ BINARY]:https://gemspec.gematik.de/docs/gemSpec/gemSpec_COS/latest/#14.3.2.2

[StartCardSession]:https://gemspec.gematik.de/docs/gemSpec/gemSpec_Kon/latest/#4.1.5.5.7

[gemSpec_PoPP_Service]:https://gemspec.gematik.de/prereleases/Draft_PoPP_25_1/gemSpec_PoPP_Service_V1.0.0_CC2/
