# VERWIJDERD_AFSTEL_ZORGKANTOOR

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
  verzender: Aanbieder (bronhouder)
  trigger: Trigger
  trigger: - Verwijdering van
  trigger: - een Afstel
  opstellen: Stel notificatie
  opstellen: VERWIJDERD_AFSTEL_ZORGKANTOOR
  opstellen: - voor het zorgkantoor
  opstellen: - dat verantwoordelijk of uitvoerend is
  verstuur: Verstuur 
  verstuur: notificatie
  ontvanger: Zorgkantoor
  ontvang: Ontvang 
  ontvang: notificatie
  verwerk: Verwerk 
  verwerk: notificatie

```

**Inhoud**

- [VERWIJDERD\_AFSTEL\_ZORGKANTOOR](#verwijderd_afstel_zorgkantoor)
  - [Documentatie](#documentatie)
  - [Trigger](#trigger)
  - [Instructie](#instructie)
  - [Type](#type)
  - [Inhoud notificatie](#inhoud-notificatie)
- [Overige notificaties Leveringsregister](#overige-notificaties-leveringsregister)


## Documentatie
Notificatie aan het zorgkantoor wanneer de aanbieder een afstel verwijdert.

Het zorgkantoor is daarmee geïnformeerd van de verwijdering van een afstel. 

De notificatie bevat informatie waarmee het zorgkantoor op de hoogte is van de verwijdering van een afstel.

## Trigger
De trigger voor het opstellen van de notificatie is: 
 > De verwijdering van een `Afstel` in het Leveringsregister

## Instructie
Stel de notificatie op voor: 
> 1. het zorgkantoor dat **verantwoordelijk** is voor de bemiddelingspecificatie waarnaar is verwezen met `bemiddelingspecificatieID` in de `Levering` waaronder de verwijderde `Afstel` was geregistreerd.
> 2. het zorgkantoor dat **uitvoerend** is voor de bemiddelingspecificatie waarnaar is verwezen met `bemiddelingspecificatieID` in de `Levering` waaronder de verwijderde `Afstel` was geregistreerd, indien deze afwijkend is aan het verantwoordelijk zorgkantoor.

## Type
Het type notificatie is:
> VERPLICHT

## Inhoud notificatie
| **Variabele** 	| **Waarde** 	| **Voorbeeld** 	|
|---	|---	|---	|
| timestamp 	| {timestamp} 	| `timestamp: "2024-07-02T00:00:00.000Z"` 	|
| afzenderIDType 	| AGBCODE 	| `afzenderIDType: "AGBCODE"` 	|
| afzenderID 	| {agb-code verzender} 	| `afzenderID: "12345678"` 	|
| ontvangerIDType 	| UZOVI 	| `ontvangerIDType: "UZOVI"` 	|
| ontvangerID 	| {uzovi-code ontvanger} 	| `ontvangerID: "5555"` 	|
| ontvangerKenmerk 	| NULL 	|  	|
| eventType 	| VERWIJDERD_AFSTEL_ZORGKANTOOR 	| `eventType:"VERWIJDERD_AFSTEL_ZORGKANTOOR"` 	|
| subjectList 	|  	| `subjectList: [{` 	|
| ../subject 	| Levering/{bemiddelingspecificatieID}	| `subject:"Levering/ef88ce35-58fa-4e6d-ac7a-6e298dd211d6"` 	|
| ../recordID 	| Afstel/{AfstelID} 	| `subject:"Afstel/76f17bb6-31b3-4042-9417-6e6bb101ce30"` 	|
| | | `}]` |

# Overige notificaties Leveringsregister
De overige notificaties van het Leveringsregister staan [hier](/notificaties/README.md)
