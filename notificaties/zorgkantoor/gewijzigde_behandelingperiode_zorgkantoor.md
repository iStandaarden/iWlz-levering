# GEWIJZIGDE_BEHANDELINGPERIODE_ZORGKANTOOR

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
  trigger: - Registratie van
  trigger: - nieuwe Behandelingperiode
  opstellen: Stel notificatie
  opstellen: GEWIJZIGDE_BEHANDELINGPERIODE_ZORGKANTOOR
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

- [GEWIJZIGDE\_BEHANDELINGPERIODE\_ZORGKANTOOR](#gewijzigde_behandelingperiode_zorgkantoor)
  - [Documentatie](#documentatie)
  - [Trigger](#trigger)
  - [Instructie](#instructie)
  - [Type](#type)
  - [Inhoud notificatie](#inhoud-notificatie)
- [Overige notificaties Leveringsregister](#overige-notificaties-leveringsregister)


## Documentatie
Notificatie aan het zorgkantoor wanneer de zorgaanbieder een behandelingperiode wijzigt.

Het zorgkantoor is daarmee geïnformeerd van de wijziging van een behandelingperiode. 

De notificatie bevat informatie waarmee het zorgkantoor de gewijzigde behandelingperiode kan raadplegen.

## Trigger
De trigger voor het opstellen van de notificatie is: 
 > De wijziging van een `Behandelingperiode` in het Leveringsregister

## Instructie
Stel de notificatie op voor: 
> 1. het zorgkantoor dat **verantwoordelijk** is voor de bemiddelingspecificatie waarnaar is verwezen met `BemiddelingspecificatieID` in de `Levering` waaronder de gewijzigde `Behandelingperiode` is geregistreerd.
> 2. het zorgkantoor dat **uitvoerend** is voor de bemiddelingspecificatie waarnaar is verwezen met `BemiddelingspecificatieID` in de `Levering` waaronder de gewijzigde `Behandelingperiode` is geregistreerd, indien deze afwijkend is aan het verantwoordelijk zorgkantoor.

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
| eventType 	| GEWIJZIGDE_BEHANDELINGPERIODE_ZORGKANTOOR 	| eventType: "GEWIJZIGDE_BEHANDELINGPERIODE_ZORGKANTOOR" 	|
| subjectList 	|  	| subjectList: [{ 	|
| ../subject 	| Levering/{bemiddelingspecificatieID}	| subject: "Levering/ef88ce35-58fa-4e6d-ac7a-6e298dd211d6" 	|
| ../recordID 	| Behandelingperiode/{BehandelingperiodeID} 	| subject: "Behandelingperiode/76f17bb6-31b3-4042-9417-6e6bb101ce30" 	|
| | | }] |

# Overige notificaties Leveringsregister
De overige notificaties van het Leveringsregister staan [hier](/notificaties/README.md)