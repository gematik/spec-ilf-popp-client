<img align="right" width="250" src="../../images/Gematik_Logo_Flag_With_Background.png"/><br/>

**Inhaltsverzeichnis**

<!-- TOC -->
* [Vorbedingung](#vorbedingung)
* [Bearbeitung _Scenario_](#bearbeitung-_scenario_)
  * [eGK in eHealth-Kartenterminal](#egk-in-ehealth-kartenterminal)
  * [eGK in Standard-Kartenleser](#egk-in-standard-kartenleser)
* [Nachbedingung](#nachbedingung)
<!-- TOC -->

# Vorbedingung

Für die beschriebenen Arbeitsschritte ist es erforderlich, dass der PoPP-Client
eine _StartMessage_ gemäß [I_PoPP_Token_Generation.yaml][] an den PoPP-Service
geschickt hat.
Die Beschreibung dazu findet sich [hier](20_WebSocket.md).

# Bearbeitung _Scenario_

Nach dem Empfang der _StartMessage_ wird der PoPP-Service mindestens eine 
Nachricht schicken.
Der PoPP-Client bearbeitet die Nachrichten des PoPP-Service innerhalb einer 
Schleife.
Die Schleife wird verlassen, wenn die empfangene Nachricht nicht vom Typ 
_Scenario_ ist (siehe [Nachbedingung](#nachbedingung)).
Der PoPP-Client bearbeitet eine _Scenario_ Nachricht wie
[hier](#egk-in-ehealth-kartenterminal)
oder
[hier](#egk-in-standard-kartenlesegerät)
beschrieben.

## eGK in eHealth-Kartenterminal

Falls die eGK in einem eHealth-Kartenterminal steckt, dann

1. leitet der PoPP-Client das vom PoPP-Service empfangene _Scenario_ über 
   die Methode [SecureSendAPDU][] an den Konnektor weiter und
2. wartet auf die zugehörige Antwort des Konnektors.
3. Die Antwort des Konnektors leitet der PoPP-Client als 
   _ScenarioResponseMessage_ (beschrieben in [I_PoPP_Token_Generation.yaml][])
   an den PoPP-Service weiter und wartet dann auf weitere Nachrichten vom 
   PoPP-Service.

## eGK in Standard-Kartenleser

Falls die eGK in einem Standard-Kartenleser steckt, dann enthält die vom 
PoPP-Service empfangene Nachricht _Scenario_ gemäß
[I_PoPP_Token_Generation.yaml][] keinen, einen oder mehrere _ScenarioStep_ 
mit folgenden Informationen (**ACHTUNG:** Die folgende Abbildung ist gegenüber
[I_PoPP_Token_Generation.yaml][] auf den hier wesentlichen Inhalt gekürzt):

```yaml
ScenarioStep:
  type: object
  title: "Single command to be executed by an eHC"
  properties:
    commandApdu:
      type: string
      description: ISO/IEC 7816-4 command APDU
    expectedStatusWords:
      type: array
      description: |
        List of expected status words in the corresponding response APDU
      items:
        type: string
```

Der PoPP-Client schickt dann nacheinander die ISO/IEC 7816-4 Kommando-APDU(s) 
aus _ScenarioStep_ an die eGK im Standard-Kartenleser und empfängt von der 
eGK die zugehörigen Antwort-APDU(s).
Es wird empfohlen, dass der PoPP-Client das Statuswort der Antwort-APDU 
mit den erwarteten Statuswörtern (_expectedStatusWords_) vergleicht.
Falls das empfangene Statuswort nicht Element der Menge 
_expectedStatusWords_ ist, dann wird aus Performanzgründen empfohlen keine 
weiteren Kommando-APDU dieses _Scenarios_ an die eGK zu schicken.
Falls das empfangene Statuswort Element der Menge _expectedStatusWords_ ist,
fährt der PoPP-Client mit der Kommando-APDU aus dem nächsten _ScenarioStep_
fort.

Wenn der PoPP-Client alle erforderlichen Antwort-APDU empfangen hat, dann 
schickt er dem PoPP-Service eine _ScenarioResponseMessage_ Nachricht und 
wartet auf weitere Nachrichten vom PoPP-Service.

Falls die eGK im Standard-Kartenleser kontaktlos angebunden ist, dann hat 
der PoPP-Client zu dieser eGK einen PACE-Kanal aufgebaut, siehe
[hier](10_egk-stecken.md#kontaktloser-standard-kartenleser).
In diesem Fall muss der PoPP-Client die Kommando-APDU aus _ScenarioStep_
gemäß den Vorgaben aus dem Kapitel
[Sicherung einer Kommando-APDU][SecureCmd]
in eine gesicherte Kommando-APDU transformieren, bevor er sie über den
Standard-Kartenleser an die eGK schickt.
Zudem muss der PoPP-Client die Antwort-APDU aus der eGK gemäß den Regeln aus
Kapitel
[Sicherung einer Antwort-APDU][UnsecureRsp]
in eine ungesicherte Antwort-APDU transformieren, bevor er sie an den 
PoPP-Service schickt.

# Nachbedingung

Der PoPP-Client hat die Schleife zum Verarbeiten von _Scenario_ verlassen, 
weil die vom PoPP-Service empfangene Nachricht nicht vom Typ _Scenario_ ist.
Im Gutfall enthält die zuletzt vom PoPP-Service empfangene Nachricht das 
angefragte PoPP-Token.
Andernfalls eine Fehlermeldung, siehe [hier](90_fehler.md).

Falls eine eGK in einem eH-KT für PoPP genutzt wurde, der PoPP-Client also wie
[hier](10_egk-stecken.md#informationen-für-die-startmessage) beschrieben die
Methode [StartCardSession][] nutzte, dann muss der PoPP-Client zum Abschluss 
die Konnektormethode [StopCardSession][] aufrufen.

[I_PoPP_Token_Generation.yaml]:https://github.com/gematik/api-popp/blob/main/src/openapi/I_PoPP_Token_Generation.yaml

[SecureCmd]:https://gemspec.gematik.de/docs/gemSpec/gemSpec_COS/latest/#13.2

[SecureSendAPDU]:https://gemspec.gematik.de/docs/gemSpec/gemSpec_Kon/latest/#4.1.5.5.8

[StartCardSession]:https://gemspec.gematik.de/docs/gemSpec/gemSpec_Kon/latest/#4.1.5.5.7

[StopCardSession]:https://gemspec.gematik.de/docs/gemSpec/gemSpec_Kon/latest/#4.1.5.5.9

[UnsecureRsp]:https://gemspec.gematik.de/docs/gemSpec/gemSpec_COS/latest/#13.3

[gemSpec_PoPP_Service]:https://gemspec.gematik.de/prereleases/Draft_PoPP_25_1/gemSpec_PoPP_Service_V1.0.0_CC2/
