# NIEUW_VERZOEK_ZORGKANTOOR

```mermaid
---
config:
  theme: neutral
  look: classic
---
stateDiagram
  direction LR
  state verzender {
    direction TB
    trigger --> opstellen
    opstellen --> verstuur
    trigger
    opstellen
    verstuur
  }
  state ontvanger {
    direction TB
    ontvang --> verwerk
    ontvang
    verwerk
  }
  [*] --> trigger
  verstuur --> ontvang
  verwerk --> [*]
  verzender:Registrerende aanbieder
  trigger:Trigger
trigger:- Registratie van
trigger:- nieuw Verzoek
  opstellen:Stel notificatie
opstellen:NIEUW_VERZOEK_ZORGKANTOOR
opstellen:- voor het zorgkantoor
opstellen:- dat verantwoordelijk is
  verstuur:Verstuur 
  verstuur: notificatie
  ontvanger: Zorgkantoor
  ontvang:Ontvang 
  ontvang:notificatie
  verwerk:Verwerk 
  verwerk:notificatie

```

**Inhoud**

- [NIEUW\_VERZOEK\_ZORGKANTOOR](#nieuw_verzoek_zorgkantoor)
  - [Documentatie](#documentatie)
  - [Trigger](#trigger)
  - [Instructie](#instructie)
  - [Type](#type)
  - [Inhoud notificatie](#inhoud-notificatie)
- [Overige notificaties Leveringsregister](#overige-notificaties-leveringsregister)


## Documentatie
Notificatie aan het verantwoordelijke zorgkantoor wanneer de zorgaanbieder – die van dit zorgkantoor een Bemiddelingsspecificatie heeft ontvangen – een nieuw verzoek registreert.

Het zorgkantoor is daarmee geïnformeerd van de registratie van een nieuw verzoek. 

De notificatie bevat informatie waarmee het zorgkantoor het verzoek kan raadplegen.

## Trigger
De trigger voor het opstellen van de notificatie is: 
 > De registratie van een `Verzoek` in het Leveringsregister

## Instructie
Stel de notificatie op voor: 
> het zorgkantoor dat verantwoordelijk is voor de bemiddelingspecificatie waarnaar is verwezen met `BemiddelingspecificatieID` in de `Levering` waaronder het `Verzoek` is geregistreerd .

## Type
Het type notificatie is:
> VERPLICHT

## Inhoud notificatie
| **Variabele** 	| **Waarde** 	| **Voorbeeld** 	|
|---	|---	|---	|
| timestamp 	| {timestamp} 	| timestamp: "2024-07-02T00:00:00Z" 	|
| afzenderIDType 	| AGBCODE 	| ontvangerIDType: "AGBCODE" 	|
| afzenderID 	| {agb-code verzender} 	| ontvangerID: "12345678" 	|
| ontvangerIDType 	| UZOVI 	| ontvangerIDType: "UZOVI" 	|
| ontvangerID 	| {uzovi-code verzender} 	| ontvangerID: "5555" 	|
| ontvangerKenmerk 	| NULL 	|  	|
| eventType 	| NIEUW_VERZOEK_ZORGKANTOOR 	| eventType: "NIEUW_VERZOEK_ZORGKANTOOR" 	|
| subjectList 	|  	| subjectList: [{ 	|
| ../subject 	| Levering/{leveringID}	| subject: "Levering/ef88ce35-58fa-4e6d-ac7a-6e298dd211d6" 	|
| ../recordID 	| Verzoek/{VerzoekID} 	| subject: "Verzoek/76f17bb6-31b3-4042-9417-6e6bb101ce30" 	|
| | | }] |

# Overige notificaties Leveringsregister
De overige notificaties van het Leveringsregister staan [hier](/notificaties/README.md)