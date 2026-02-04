# Raadplegen van de Uitstelperiode door het zorgkantoor (UCLR-0007) 

```mermaid
---
config:
  theme: mc
  look: classic
  layout: elk
---
flowchart LR
 subgraph s1["Leveringsregister"]
          B["Raadplegen Uitstelperiode"]
  end
    A["Zorgkantoor"] --> B
    B@{ shape: terminal}
    A@{ shape: rounded}
    style s1 fill:#FFF9C4,stroke:#FFF9C4
```


## **Use Case Beschrijving**  
**Titel:** Raadplegen van de uitstelperiode (na notificatie) door het zorgkantoor  
**Actoren:** Zorgkantoor die betrokken is bij de levering van zorg aan een client.  

### Precondities:
- De Uitstelperiode is opgenomen in het Leveringsregister.
- Het zorgkantoor is betrokken bij de levering van zorg aan de client door de registratie van een bemiddelingspecificatie door het verantwoordelijk zorgkantoor.


### Autorisatie:
Het zorgkantoor mag de uitstelperiode (en overige gegevens) raadplegen. 
- Volledige autorisatieregel: [LRA0002](https://informatiemodel.istandaarden.nl/informatiemodel/iwlz/netwerk/leveringsregister-1/regels/autorisatieregel/lra0002) (verantwoordelijk zorgkantoor), [LRA0001](https://informatiemodel.istandaarden.nl/informatiemodel/iwlz/netwerk/leveringsregister-1/regels/autorisatieregel/lra0001/) (uitvoerend zorgkantoor)
- Autorisatiematrix: [LRA0001, LRA0002](/raadplegen/autorisatiematrix_leveringsregister.md)

**Trigger:**
- Een zorgkantoor wil de (actuele) uitstelperiode (en overige informatie) raadplegen waarvan het de notificatie heeft ontvangen.


## Query-template beschrijving

| **Query ID** | **Beschrijving** | **Verplichte input** | **resultaat** |
|---|---|---|---|
| [**QLR-0007-ZK**](/gql-query/zorgkantoor/QLR-0007-ZK.graphql) | Op basis van de (ontvangen) UitstelperiodeID en eigen identificatie, de Uitstelperiode en overig toegestane informatie raadplegen raadplegen | `uitstelperiodeID` | Uitstelperiode /  Levering /  Client / Leveringperiode / Afstel |

## **Proces raadplegen**

Een zorgkantoor is (via een zorgaanbieder) betrokken bij de zorg van een client. Als de zorgaanbieder een uitstelperiode registreert, aanpast of verwijdert, ontvangt het zorgkantoor daarvan een notificatie. (Ga naar het overzicht [notificaties](/notificaties#notificaties-aan-het-zorgkantoor) om te bekijken welke dit zijn).

Op basis van deze notificatie kan het zorgkantoor de informatie in het leveringsregister raadplegen. 

### Schematisch:

```mermaid
---
config:
  theme: neutral
  look: classic
---
stateDiagram
  direction TB
  [*] --> raadplegen
  idAvailable --> notifyWait:nee
  idAvailable --> QBR0001ZAiq:ja
  notifyWait --> notifyReceive
  notifyReceive --> QBR0001ZAiq
  QBR0001ZAiq --> QBR0001ZA
  QBR0001ZA --> PEP
  PEP --> [*]:geen toegang
  PEP --> resource:toegang
  resource --> [*]
  raadplegen --> idAvailable
  raadplegen:Raadplegen Leveringsregister voor (actuele) status uitstelperiode
  idAvailable:uitstelperiodeID bekend?
  notifyWait:Wacht op notificatie
  QBR0001ZAiq:Gebruik uitstelperiodeID
  notifyReceive:notificatie NIEUWE_UITSTELPERIODE_ZORGKANTOOR of GEWIJZIGDE_UITSTELPERIODE_ZORGKANTOOR ontvangen
  QBR0001ZA:Gebruik template QLR-0007-ZK
  PEP:Toegangscontrole PEP
  style raadplegen fill:#BBDEFB,color:none
  style notifyWait fill:#FFD600
  style notifyReceive fill:#C8E6C9
  style QBR0001ZA fill:#00C853

```


| # | Toelichting |
| --: | :-- |
| 1. | *Start* raadplegen Leveringsregister | 
| 2. | Is de **`uitstelperiodeID`** bekend? <br/> - **Ja** →  Ga verder naar stap 6 <br/> - **Nee** → Wacht op notificatie [**`NIEUWE_UITSTELPERIODE_ZORGKANTOOR`** of **`GEWIJZIGDE_UITSTELPERIODE_ZORGKANTOOR`**](/notificaties/)  | 
| 4. | Notificatie is ontvangen | 
| 5. | Gebruik de informatie uit de notificatie voor het raadplegen van het leveringsregister |
| 6. | Het zorgkantoor vult de verplichte **`uitstelperiodeID`** in query-template [QLR-0007-ZK.graphql](/gql-query/zorgkantoor/QLR-0007-ZK.graphql) en initieert een raadpleging van de uitstelperiode in het Leveringsregister. | 
| 7. | Het zorgkantoor stuurt Graphql-request + Access-token naar het Policy Enforcement Point (PEP) |
| 8. | De PEP voert de [toegangscontrole](UCLR-0007-toegangscontrole.md) uit en stuurt bij toegang het request door naar het Leveringsregister. |
| 9. | Het zorgkantoor ontvangt response van de PEP (bij ongeldig verzoek) of vanuit het Leveringsregister (resource) |
| 10. | *Einde proces* | 


---

Ga naar beschrijving van de bijbehorende [toegangscontrole](UCLR-0007-toegangscontrole.md) | Terug naar [Raadplegen](/raadplegen/README.md)