<img align="right" width="250" src="../../images/Gematik_Logo_Flag_With_Background.png"/><br/>

**Inhaltsverzeichnis**

<!-- TOC -->
* [Einleitung](#einleitung)
* [Fehlerfälle](#fehlerfälle)
<!-- TOC -->

# Einleitung

In diesem Dokument werden typische Fehlerfälle betrachtet, mit denen der 
PoPP-Client rechnen muss und die auftreten können, wenn der PoPP-Client ein
PoPP-Token vom PoPP-Service anfragt und dabei eine eGK in der
Leistungserbringerinstitution verwendet wird.

Die hier gelisteten Fehlerfälle sind beispielhaft und unvollständig.

# Fehlerfälle

1. Ein Kartenlesegerät hat das Stecken einer Karte erkannt, aber die Karte 
   lässt sich nicht aktivieren. Mögliche Ursachen:
   1. Die Karte wurde in einer falschen Orientierung gesteckt, weshalb die 
      Kartenkontakte keine Verbindung zu den Kartenleserkontakten haben.  
      **Abhilfe:** Karte in der korrekten Orientierung einstecken.
   2. Die Karte unterstützt das bei der Aktivierung angegebene 
      Übertragungsprotokoll nicht.
      Da alle Karten der TI das Übertragungsprotokoll T=1 gemäß
      ISO/IEC 7816-3 unterstützen
      tritt dieser Fehler nur auf, wenn statt einer eGK eine Karte 
      anderen Typs gesteckt wird, etwa eine KVK, eine Telefonkarte oder 
      SIM-Karte.  
      **Abhilfe:** eGK stecken.
2. Der Vorgang "PoPP-Token abrufen" wurde manuell gestartet, aber es wird 
   keine Karte in das ausgewählte Kartenlesegerät gesteckt. Mögliche Ursachen:
   1. eGK nicht vorhanden / zu Hause vergessen.  
      **Abhilfe:** Vorgang abbrechen.
   2. Versicherter will die eGK nicht stecken.  
      **Abhilfe:** Vorgang abbrechen.
3. Eine Kommunikation mit der Karte ist nicht möglich. Mögliche Ursachen:
   1. Die Karte wurde zu früh aus dem Kartenlesegerät entfernt.  
      **Abhilfe:** Vorgang wiederholen.
   2. Die Übertragungsstrecke zum Kartenlesegerät ist gestört (USB-Stecker 
      des Standard-Kartenlesers gezogen, Netzwerkstörung zum Konnektor 
      / eH-KT, Konnektor oder eH-KT mit eGK ausgefallen, ...).  
      **Abhilfe:** Störung beseitigen, Vorgang wiederholen.
4. Verbindungsaufbau zum PoPP-Service scheitert. Mögliche Ursachen:
   1. Netzwerkstörung,
   2. PoPP-Service ausgefallen,
   3. **Abhilfe jeweils:** Vorgang abbrechen.
5. Die Kommunikation mit dem PoPP-Service ist gestört / der PoPP-Service 
   schickt keine Nachrichten. Mögliche Ursachen:
   1. Netzwerkstörung,
   2. PoPP-Service ausgefallen,
   3. **Abhilfe jeweils:** Vorgang abbrechen.
6. Der PoPP-Service schickt eine _ErrorMessage_ gemäß
   [I_PoPP_Token_Generation.yaml][].
   Die _ErrorMessage_ enthält mögliche Ursachen, die Hinweise auf Abhilfe 
   geben. 

[I_PoPP_Token_Generation.yaml]:https://github.com/gematik/api-popp/blob/main/src/openapi/I_PoPP_Token_Generation.yaml
