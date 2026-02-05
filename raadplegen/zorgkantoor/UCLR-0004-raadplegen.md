# Raadplegen Verzoek door verantwoordelijk zorgkantoor (UCL-0004-ZK)

```mermaid
---
config:
  theme: mc
  look: classic
  layout: elk
---
flowchart LR
 subgraph s1["Leveringsregister"]
          B["Raadplegen Verzoek"]
  end
    A["Zorgkantoor <br/>(verantwoordelijk)"] --> B
    B@{ shape: terminal}
    A@{ shape: rounded}
    style s1 fill:#FFF9C4,stroke:#FFF9C4
```

## Use Case Beschrijving

**Titel:** Raadplegen van verzoek door een Zorgkantoor<br/>
**Actoren:** Zorgkantoor dat verantwoordelijk is voor bemiddelingspecificatie

### Precondities: 
- Het Verzoek is opgenomen in het Leveringsregister
- Het zorgkantoor is verantwoordelijk voor de bemiddelingspecificatie waar het Verzoek bij hoort.

### Autorisatie:
Een zorgkantoor mag voor het toeleiden van een cliënt het Verzoek raadplegen die hoort bij een bemiddelingspecificatie waarvoor het zorgkantoor veranntwoordelijk is. 
- Volledige autorisatieregel: [LRA0003](https://informatiemodel.istandaarden.nl/informatiemodel/iwlz/netwerk/leveringsregister-1/regels/autorisatieregel/lra0003/)
- Autorisatiematix: [LRA0003](/iWlz-levering/raadplegen/autorisatiematrix_leveringsregister.md)

### Trigger: 
- Het zorgkantoor wil het Verzoek raadplegen te ondersteuning van het toeleidingsproces van een cliënt.

## Query-template beschrijving
| **Query** | **Beschrijving** | **Verplichte input** | **Resultaat** |
| --- | :--- | :--- | :--- |
| QLR-0004-ZK | Op basis van de (ontvangen) verzoekID, het Verzoek, VerzoekAanbieder, Levering en Client raadplegen | `verzoekID` | Verzoek / VerzoekAanbieder / Levering / Client |

## Proces raadplegen
Het zorgkantoor dat verantwoordelijk is voor de bemiddingspecificatie die hoort bij het Verzoek ontvangt de notificatie `NIEUW_VERZOEK_ZORGKANTOOR`. Nadat het zorgkantoor de notificatie heeft ontvangen mag dat zorgkantoor een raadpleging uitvoeren. De notificatie bevat het `verzoekID` om het Verzoek te raadplegen.


**Schematisch:**

```mermaid
---
config:
  theme: neutral
  look: classic
---
stateDiagram
  direction TB
  state chooseQuery <<choice>>
  [*] --> Raadplegen
  Raadplegen --> idAvailable
  idAvailable --> notifyWait:nee
  notifyWait --> notifyReceive
  idAvailable --> chooseQuery:ja
  notifyReceive --> chooseQuery
  chooseQuery --> inputQuery
  inputQuery --> Query
  Query --> PEP
  PEP --> [*]:geen toegang
  PEP --> resource:toegang
  resource --> [*]
  Raadplegen:(1) Raadplegen Verzoek
  idAvailable:(2) VerzoekID bekend?
  notifyWait:(3) Wacht op notificatie
  notifyReceive:notificatie NIEUW_VERZOEK_ZORGKANTOOR ontvangen
  inputQuery:(4) Gebruik verzoekID
  Query:(5) Gebruik query QLR-0004-ZK
  resource:Leveringsregister
  style Raadplegen fill:#BBDEFB,color:none
  style notifyWait fill:#FFD600
  style notifyReceive,inputQuery fill:#C8E6C9
  style Query fill:#00C853
```

| **#** | **Toelichting** |
| --- | :--- |
| 1. | *Start* |
| 2. | Is het `verzoekID` bekend? <br/> - **Ja** -> Ga verder naar stap 4 <br/> - **Nee** -> Wacht op notificatie [NIEUW_VERZOEK_ZORGKANTOOR](/iWlz-levering/notificaties/zorgkantoor/nieuw_verzoek_zorgkantoor.md) 
| 4. | Het zorgkantoor vult het verplichte `verzoekID` in query-template [QLR-0004-ZK](/iWlz-levering/gql-query/zorgkantoor/QLR-0004-ZK.graphql) en initieert een raadpleging van het Verzoek in het Leveringsregister. |
| 4. | Het zorgkantoor stuurt Graphql-request + Acces-token naar het Policy Enforcement Point (PEP) |
| 5. | De PEP voert de [toegangscontrole](/iWlz-levering/raadplegen/zorgkantoor/UCLR-0004-toegangscontrole.md) uit en stuurt bij toegang het request door naar het leveringsregister.
| 6. | De aanbieder ontvangt response van de PEP (bij ongeldig verzoek) of vanuit het Leveringsregister (resource). |
| 7. | *Einde proces* | 

---
Ga naar beschrijving van de bijbehorende [toegangscontrole](/iWlz-levering/raadplegen/zorgkantoor/UCLR-0004-toegangscontrole.md) | Terug naar [Raadplegen](/iWlz-levering/raadplegen/README.md)
