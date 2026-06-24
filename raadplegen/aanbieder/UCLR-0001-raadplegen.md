# Raadplegen van de Levering die horen bij overlappende Bemiddelingspecificatie(s) door de Aanbieder (UCLR-0001)

> [!CAUTION] 
> Voor de controle op de toegang van deze query is er een PIP controle nodig. De toets of dit mogelijk met de huidige informatie mogelijk is, is moet nog plaatsvinden. De query kan nog wijzigen, wat effect kan hebben op het schema. 

```mermaid
---
config:
  theme: mc
  look: classic
  layout: elk
---
flowchart LR
 subgraph s1["Leveringsregister"]
          B["Raadplegen Levering overlappende Bemiddelingspecificatie"]
  end
    A["Aanbieder<br>(uitvoerend)"] --> B
    B@{ shape: terminal}
    A@{ shape: rounded}
    style s1 fill:#FFF9C4,stroke:#FFF9C4
```

## Use Case Beschrijving

**Titel:** Raadplegen van de Levering die horen bij overlappende Bemiddelingspecificatie(s) door de Aanbieder<br/>
**Actoren:** Aanbieder betrokken bij de levering van zorg en ondersteuning aan een cliënt

### Precondities:
- De Levering is opgenomen in het Leveringsregister
- De aanbieder is door het verantwoordelijk zorgkantoor betrokken bij de levering van zorg en ondersteuning aan de cliënt door de registratie van een bemiddelingspecificatie.

### Autorisatie
Een aanbieder mag voor het leveren van zorg en ondersteuning aan een cliënt de gegevens over de status van de levering van de zorg of ondersteuning raadplegen die horen bij overlappende bemiddelingspecificaties.
- Volledige autorisatieregel: [LRA005](https://informatiemodel.istandaarden.nl/informatiemodel/iwlz/netwerk/leveringsregister-1/regels/autorisatieregel/)
- Autorisatiematrix: [LRA0005](/iWlz-levering/raadplegen/autorisatiematrix_leveringsregister.md) 

**Trigger:**
- Een aanbieder wil voor het leveren van zorg of ondersteuning aan een cliënt de levering die horen bij de overlappende bemiddelingspecificaties raadplegen.

## Query-template beschrijving

| **Query ID** | **Beschrijving** | **Verplichte input** | **resultaat** |
|---|---|---|---|
| [QLR-0001-ZA](/gql-query/aanbieder/QLR-0001-ZA.graphql) |Op basis van de bemiddelingspecificatieID van de informatieve toewijzing de Levering (en overige toegestane informatie) raadplegen, die hoort bij de informatieve toewijzing. |   `bemiddelingspecificatieIDInformatieve` | Levering / Leveringperiode / Behandelingperiode / Uitstelperiode / Afstel / Client | 



## **Proces raadplegen**

Een aanbieder is bij de zorg van een cliënt betrokken door het zorgkantoor. Met aanvullende informatie uit de overlappende bemiddelingspecificatie  (zie ook [UCBR-0002_3](https://github.com/iStandaarden/iWlz-bemiddeling/blob/Bemiddelingsregister-1/raadplegen/zorgaanbieder/UCBR-0002_3-raadplegen.md)) kan de aanbieder de status van de levering zien die bij de informatieve toewijzing (bemiddelingspecificatie) https://github.com/iStandaarden?view_as=public.  

Hiervoor is de informatieve `bemiddelingspecificatieID` nodig. 

### Schematisch:
```mermaid
---
config:
  theme: neutral
  look: classic
---
stateDiagram
  direction TB
  state s4 {
    direction TB
    s4_1
  }

  s1 --> s2
  s2 --> s4:Nee
  [*] --> s1
  s4_1 --> [*]
  s2 --> s6:Ja
  s6 --> s13
  s13 --> s14
  s14 --> s15:Toegang
  s14 --> [*]:Geen toegang
  s15 --> [*]

    s1:(1) Raadplegen Leveringsregister voor status Levering
    s2:(2) informatieve bemiddelingspecificatieID bekend?
    s4:Eigen- en overlappende toewijzingen raadplegen
    s4_1:(3) Ga naar de andere beschrijving
    s4_1: UCBR-0002_3-raadplegen

    s6: (4) Gebruik template QLR-0001-ZA


  s13:(5) Insturen Query
  s14:(6) Toegangscontrole PEP
  s15:Resource
  style s4_1 fill:#FFD600
  style s6 fill:#00C853

```

| #    | Toelichting |
| :--- | :----- |
| 1.   | Start raadplegen Leveringsregister   |
| 2.   | Is de informatieve `bemiddelingsspecificatieID` bekend?<br> - **Ja** -> Ga verder naar stap 4. <br> - **Nee** -> Ga verder naar stap 3  |
| 3.   | Gebruik eerst query-template `QBR-0002_3-ZA` (zie beschrijving [UCBR-0002_3-raadplegen](https://github.com/iStandaarden/iWlz-bemiddeling/blob/Bemiddelingsregister-1/raadplegen/zorgaanbieder/UCBR-0002_3-raadplegen.md)). |
| 4.   |  Gebruik query-template `QLR-0001-ZA` en vul de verplichte parameters:  <br> - `bemiddelingspecificatieIDInformatieve`;   |
| 5.   | De zorgaanbieder stuurt GraphQL-request + Acces-token naar het Policy Enforcement Point (PEP). |
| 6.   | De PEP voert de [toegangscontrole](/raadplegen/aanbieder/UCLR-0001-toegangscontrole.md) uit en stuurt bij toegang het request door naar het Leveringsgregister.  |
| 7.   | De zorgaanbieder ontvangt response van de PEP (bij ongeldig verzoek) of vanuit het Leveringsregister (resource). |
| 8.   | *Einde proces*  |
---

Ga naar beschrijving van de bijbehorende [toegangscontrole](/raadplegen/aanbieder/UCLR-0001-toegangscontrole.md) | Terug naar [Raadplegen](/raadplegen/README.md)
