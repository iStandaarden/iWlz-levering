## Toegangscontrole: Raadplegen Levering van een informatieve bemiddelingspecificaties (UCLR-0009)

> [!Caution]
Voor de controle op toegang van deze query is er een PIP controle nodig. De toets of dit mogelijk met de huidige informatie mogelijk is, is moet nog plaatsvinden. De query kan nog wijzigen, wat effect kan hebben op het schema.

Beschrijving van de **toegangscontrole** door de Policy Decision Point (PDP) en indien van toepassing Policy Information Point (PIP).

N.b. Het valideren van de Acces-token door de PEP is geen onderdeel van deze beschrijving. Zie daarvoor het [Afsprakenstelsel iWlz - nID netkwerkstelsel - 5. Policy Enforcement Point.](https://wlz.atlassian.net/wiki/spaces/IWLZAS/pages/229441537/nID+netwerkstelsel#5.-Policy-Enforcement-Point-(PEP))

## Toegangscontrole PDP

### Subject
* **Entiteit:** Uitvoerend zorgkantoor (bovenregionaal)
* **Kenmerk:** In bezit van een access-token met daarin de eigen `uzovicode`

### Action
* **Type:** `raadplegen` (read)
* **Omschrijving:** Uitvoeren van GraphQL-query [QLR-0009-ZK.graphql](/gql-query/zorgkantoor/QLR-0009-ZK.graphql) op het Leveringsregister door een uitvoerend zorgkantoor.

### Resource
* **Type:** `Leveringsregister`
* **ID:** `bemiddelingspecificatieID` van de informatieve bemiddelingspecificatie
* **beperking:** Toegang tot de gegevens over de Levering van de informatieve toewijzing, indien deze toewijzing periode-overlap heeft met een eigen bemiddelingspecificatie. 
* **Inhoud:** Alle nodes, behalve `Verzoek` en `VerzoekAanbieder`, in het GraphQL-schema die horen bij deze `Levering`, mogen direct worden opgevraagd. 

### Context
- **Query-parameters vereist:**
  - informatieve `bemiddelingspecificatieID`

- **Toegangsvoorwaarde:** Er is alleen toegang als aan alle volgende voorwaarden is voldaan:

1. Ophalen benodigde context data (PIP)
    Input:
    - `bemiddelingspecificatieID` uit de raadpleeg-query
    - `uzoviCode` uit de accestoken

```graphQl
query PIPcontextBSdata(
    $bemiddelingspecificatieID: UUID! # bemiddelingspecificatieID uit initiele raadpleging
    $tokenUzovi: String!
){
    # de opgevraagde bemiddelingspecificatie
    bemiddelingspecificatie(
        where: { bemiddelingspecificatieID: { EQ: $bemiddelingspecificatieID } }
    ) {
        bemiddelingspecificatieID
        toewijzingIngangsdatum
        toewijzingEinddatum
        vaststellingMoment
        bemiddeling {
            # de bemiddelingspecificatie van het raadplegende zorgkantoor
            bemiddelingspecificatie(
                where: { uitvoerendZorgkantoor: { eq: $tokenUzovi } }
            ) {
                toewijzingIngangsdatum
                toewijzingEinddatum
                vaststellingMoment
            }
        }
    }
}
```
2. Bepalen toegang op basis van de verkregen context-data uit stap 1.\
   De beoordeling gaat op basis van de ontvangen contextdata en zal plaatsvinden op basis van de (REGO) policy-beoordeling door de PDP. De policy zal de volgende afweging moeten doorlopen om te bepalen of het raadplegende zorgkantoor toegang krijgt tot de opgevraagde Leveringen bij de  Bemiddelingspecificatie.\
\
   Op basis van de context-data uit stap 1, is er: 
    1. **Geen enkele** `Bemiddelingspecificatie` voor het raadplegende zorgkantoor in de contextdata.   
    Resultaat: **Geen toegang**

    2. **Tenminste 1** `Bemiddelingspecificatie` voor het raadplegende zorgkantoor in de contextdata moet voldoen aan de volgende overlap-voorwaarden.  
    Er moet beoordeeld worden of tenminste 1 `Bemiddelingspecificatie` overlap heeft met de te raadplegen `Bemiddelingspecificatie` waarvan:
        1.  de `eigen.bspec.toewijzingIngangsdatum` *kleiner of gelijk* is aan de `opgevraagde.bspec.toewijzingEinddatum` ***of***  
          de `eigen.bspec.vaststellingMoment` *kleiner of gelijk* is aan de `opgevraagde.bspec.toewijzingEinddatum`;  
          **èn**
        2. de `eigen.bspec.toewijzingEinddatum` is null (leeg) ***of***  
          de `eigen.bspec.toewijzingEinddatum` *groter of gelijk* is aan de `opgevraagde.bspec.toewijzingIngangsdatum` ***of***  
          de `eigen.bspec.toewijzingEinddatum` *groter of gelijk* is aan de `opgevraagde.bspec.vaststellingMoment`  
          
      Voldoet geen van de gevonden `Bemiddelingspecificatie` van het raadplegende zorgkantoor aan de overlap voorwaarden?  
      Resultaat: **Geen toegang**\
\
   3. De toegang geldt t/m 31 mei van het jaar dat volgt op de einddatum van de eigen Bemiddelingspecificatie (`eigen.bspec.toewijzingEinddatum`).\
\
   Van de overlappende `Bemiddelingspecificatie`: 
    1. is er een `eigen.bspec.toewijzingEinddatum` is null (leeg) -> Resultaat: **Toegang** 
    2. Valt de datum van raadplegen *voor of op* 31 mei van het jaar dat volgt op de grootst gevonden `eigen.bspec.toewijzingEinddatum` -> Resultaat: **Toegang**\
\
    Voldoet geen van de gevonden `Bemiddelingspecificatie` van het raadplegende zorgkantoor aan de toegangs voorwaarden?  
    Resultaat: **Geen toegang**

### Resultaat 

Toegang tot het Bemiddelingsregister via query [QLR-0009](/gql-query/zorgkantoor/QLR-0009-ZK.graphql) is **alleen toegestaan** als:
- Parameter `bemiddelingspecificatieID` is meegegeven in de query
- De acces-token bevat een geldige `uzovicode`
- De PIP raadplegeing context-data oplevert die volgens de gestelde voorwaarden toegang geeft.

Als aan alle voorwaarden is voldaan, mogen de nodes `Levering`, `Leveringperiode`, `Behandelingperiode`, `Afstel`, `Uitstelperiode` en `Client` die horen bij deze `Bemiddelingspecificatie` direct worden opgevraagd. 


## Toegangscontrole-flows Zorgkantoor:"QLR-0009-ZK"
Beschrijving van het autorisatieproces.

**Schematisch:**

```mermaid
---
config:
  theme: neutral
  look: classic
---
stateDiagram
  direction TB
  [*] -->  indienen
  indienen --> validerenT
  state PEP {
    direction TB

    validerenT --> validerenR: access-token is geldig
    state PDP {
    validerenR --> checkInput01
    state check01 <<choice>>
    checkInput01 --> check01

    check01 --> checkInput02:ja
    check01 --> error:nee
    state PIP {
        checkInput02 --> checkInput03
        }
    state check02 <<choice>>
    state check03 <<choice>>
    
    checkInput03 --> check02
        
        check02 --> error:nee
        check02 --> checkOVerlap:ja           
    checkOVerlap --> check03
    check03 --> access:ja
    check03 --> error:nee
    checkInput03
    error
    access
    }

  }

  error --> [*]: deny
  access --> resource: allow
  resource --> [*]
  
  PEP:Autorisatie controle PEP
  PDP:Toegangscontrole PDP
  PIP:Contextinformatie controle PIP
  indienen: Ontvang QLR-0009-ZKu + Access token
  validerenT: Valideer access token
  validerenR: Valideer Request
  checkInput01:bemiddelingspecificatieID aanwezig?
  checkInput02:Contextdata ophalen
  checkInput03:Contextdata aanwezig?
  checkOVerlap:Overlapping contextdata aanwezig en binnen toegangsperiode?
  error:geen toegang tot Resource

  access:toegang tot Resource
  resource: Query mag door naar Leveringsregister
  style validerenR,checkInput01,checkInput02,checkInput03,checkOVerlap fill:#FFD600
  style error fill:#D50000
  style access,Query,resource fill:#00C853
  style indienen fill:#BBDEFB,color:none
```

| # | Toelichting |
|:--- | :--- |
| 1. | Ontvangst GraphQL-request + acces-token door **PEP**. |
| 2. | De **PEP** valideert de acces-token en geeft na goedkeur het request door aan de PDP. |
|3. | De **PDP** voert de volgende stappen uit:<br/>1. controleer of het request voldoet aan de template en er geen ongeoorloofde gegevens worden opgevraagd.<br/>2. Aanwezigheid van de verplichte parameters in het request;<br/>3. Laat **PIP** context-data ophalen;<br/>4. Beoordeel de aanwezigheid van de context-data en de voorwaarden van toegang. <br/><br/>Is aan alle voorwaarden voldaan?<br/> - **Ja** →  Ga verder naar stap 4<br/>- **Nee** → *Einde proces (geen toegang.)*   |
| 4. | Het zorgkantoor krijgt toegang tot het Leveringsregister.|
| 5. | *Einde* |

---
Ga naar [UC beschrijving raadplegen](UCLR-0009-raadplegen.md) -- terug naar [Raadplegen](../README.md)

