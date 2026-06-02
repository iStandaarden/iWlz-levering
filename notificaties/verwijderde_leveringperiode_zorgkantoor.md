# VERWIJDERDE_LEVERINGPERIODE_ZORGKANTOOR

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
  trigger:Trigger
  trigger:- Verwijdering van
  trigger:- Leveringperiode
  opstellen:Stel notificatie
  opstellen: VERWIJDERDE_LEVERINGPERIODE_ZORGKANTOOR
  opstellen:- voor het zorgkantoor
  opstellen:- dat verantwoordelijk of uitvoerend is
  verstuur:Verstuur 
  verstuur: notificatie
  ontvanger: Zorgkantoor
  ontvang:Ontvang 
  ontvang:notificatie
  verwerk:Verwerk 
  verwerk:notificatie

```

**Inhoud**

- [VERWIJDERDE\_LEVERINGPERIODE\_ZORGKANTOOR](#verwijderde_leveringperiode_zorgkantoor)
  - [Documentatie](#documentatie)
  - [Trigger](#trigger)
  - [Instructie](#instructie)
  - [Type](#type)
  - [Inhoud notificatie](#inhoud-notificatie)
- [Overige notificaties Leveringsregister](#overige-notificaties-leveringsregister)


## Documentatie
Notificatie aan het zorgkantoor wanneer de aanbieder een leveringperiode verwijdert.

Het zorgkantoor is daarmee geïnformeerd van de verwijdering van een leveringperiode. 

De notificatie bevat informatie waarmee het zorgkantoor op de hoogte is van de verwijderde leveringperiode.

## Trigger
De trigger voor het opstellen van de notificatie is: 
 > De verwijdering van een `Leveringperiode` in het Leveringsregister

## Instructie
Stel de notificatie op voor: 
> 1. het zorgkantoor dat **verantwoordelijk** is voor de bemiddelingspecificatie waarnaar is verwezen met `bemiddelingspecificatieID` in de `Levering` waaronder de *verwijderde* `Leveringperiode` was geregistreerd.
> 2. het zorgkantoor dat **uitvoerend** is voor de bemiddelingspecificatie waarnaar is verwezen met `bemiddelingspecificatieID` in de `Levering` waaronder de *verwijderde* `Leveringperiode` was geregistreerd, indien deze afwijkend is aan het verantwoordelijk zorgkantoor.

## Type
Het type notificatie is:
> VERPLICHT

## Inhoud notificatie
| **Variabele** 	| **Waarde** 	| **Voorbeeld** 	|
|---	|---	|---	|
| timestamp 	| {timestamp} 	| `timestamp: "2024-07-02T00:00:00.000Z"` 	|
| afzenderIDType 	| AGBCODE 	| `afzenderIDType: "AGBCODE"` 	|
| afzenderID 	| {agb-code afzender} 	| `afzenderID: "12345678"` 	|
| ontvangerIDType 	| UZOVI 	| `ontvangerIDType: "UZOVI"` 	|
| ontvangerID 	| {uzovi-code ontvanger} 	| `ontvangerID: "5555"` 	|
| ontvangerKenmerk 	| NULL 	|  	|
| eventType 	| VERWIJDERDE_LEVERINGPERIODE_ZORGKANTOOR 	| `eventType: "VERWIJDERDE_LEVERINGPERIODE_ZORGKANTOOR"` 	|
| subjectList 	|  	| `subjectList: [{` 	|
| ../subject 	| Levering/{bemiddelingspecificatieID}	| `subject: "Levering/ef88ce35-58fa-4e6d-ac7a-6e298dd211d6"` 	|
| ../recordID 	| Leveringperiode/{LeveringperiodeID} 	| `subject: "Leveringperiode/76f17bb6-31b3-4042-9417-6e6bb101ce30"` 	|
| | | `}]` |

# Overige notificaties Leveringsregister
De overige notificaties van het Leveringsregister staan [hier](/notificaties/README.md)
