## Toegangscontrole: Raadplegen Levering van een informatieve bemiddelingspecificaties (UCLR-0009_10)

> [!Caution]
Voor de controle op toegang van deze query is er een PIP controle nodig. De toets of dit mogelijk met de huidige informatie mogelijk is, is moet nog plaatsvinden. De query kan nog wijzigen, wat effect kan hebben op het schema.

Beschrijving van de **toegangscontrole** door de Policy Decision Point (PDP) en indien van toepassing Policy Information Point (PIP).

N.b. Het valideren van de Acces-token door de PEP is geen onderdeel van deze beschrijving. Zie daarvoor het [Afsprakenstelsel iWlz - nID netkwerkstelsel - 5. Policy Enforcement Point.](https://wlz.atlassian.net/wiki/spaces/IWLZAS/pages/229441537/nID+netwerkstelsel#5.-Policy-Enforcement-Point-(PEP))

## Toegangscontrole PDP

### Subject
* **Entiteit:** Uitvoerend zorgkantoor
* **Kenmerk:** In bezit van een access-token met daarin de eigen uzovicode

### Action
* **Type:** `raadplegen` (read)
* **Omschrijving:** Uitvoeren van GraphQl-query [QLR-0009-ZK.graphql](/iWlz-levering/gql-query/zorgkantoor/QLR-0009-ZK.graphql) of [QLR-0010-ZK.graphql](/iWlz-levering/gql-query/zorgkantoor/QLR-0010-ZK.graphql) (wanneer de eigen `bemidelingspecificatie` een einddatum heeft) op het Leveringsregister door een uitvoerend zorgkantoor.

### Resource
* **Type:** `Leveringsregister`
* **ID:** `bemiddelingspecificatieID` van de informatieve bemiddelingspecificatie
* **beperking:** Toegang tot de gegevens over de Levering van de informatieve toewijzing, indien deze toewijzing periode-overlap heeft met een eigen bemiddelingspecificatie. 
* **Inhoud:** Alle nodes, behalve `Verzoek` en `VerzoekAanbieder`, in het GraphQL-schema die horen bij deze `Levering`, mogen direct worden opgevraagd. 

### Context
- **Query-parameters vereist:**

| QLR-0009-ZK | QLR-0010-ZK |
| :--- | :--- |
| - eigen `bemiddelingspecificatieID` | - eigen `bemiddelingspecificatieID` |
| - informatieve `bemiddelingspecificatieID` | - informatieve `bemiddelingspecificatieID` | 
| - `uitvoerendZorgkantoor` | - `uitvoerendZorgkantoor` |
| - `vaststellingMoment` | - `vaststellingMoment`|
| - `dagVaststellingMoment` | - `dagVaststellingMoment` |
| - `toewijzingEindatum` |  |

- **Toegangsvoorwaarde:** Er is alleen toegang als aan alle volgende voorwaarden is voldaan:
    - De parameters zoals hierboven aanwezig zijn;
    - De **access-token** bevat een geldige `uzovicode`van het zorgkantoor;
    - De `uzovicode` van de in de query meegegeven `uitvoerendZorgkantoor`komt overeen met de `uzovicode`in de acces-token;
    - In het **Bemiddelingregister** bestaat er een `Bemiddelingspecificatie` waarbij:<br>
            a. het `uitvoerendZorgkantoor` overeenkomt met de `uzovicode`uit de acces-token **én;**<br>
            b. deze `bemiddelingspecificatie` behoort tot dezelfde `Bemiddeling` als waar de `bemiddelingspecificatie` waarvoor de `Levering` opgevraagd wordt ook bij hoort **én;**<br>
            c. deze `bemiddelingspecificaties`overlappen in periode met elkaar **én;**<br>
            d. de `toewijzingEinddatum` is leeg of de `toewijzingEinddatum` + 31 mei is  groter dan of gelijk aan het opvraagmoment.    
 

### Resultaat 

> Toegang tot het Leveringsregister via query [QLR-0009-ZK](/iWlz-levering/gql-query/zorgkantoor/QLR-0009-ZK.graphql) of [QLR-0010-ZK](/iWlz-levering/gql-query/zorgkantoor/QLR-0010-ZK.graphql) is **alleen toegestaan** als:
>- De relevante parameters aanwezig zijn per query;
>- De acces-token bevat een geldige `uzovicode`;
>- De in de query meegegeven `uzovicode` in `uitvoerendZorgkantoor` komt overeen met de `uzovicode` in de acces-token;
>- In het Bemiddelingsregister is een match gevonden tussen:
>    - de uzovicode (uit de acces-token) én;
>   - een `bemiddelingspecificatie` die hoort bij dezelfde `Bemiddeling` als de `bemiddelingspecificatie` waarvoor de `levering` opgevraagd is én;
>    - de `toewijzingEinddatum` is leeg of de `toewijzingEinddatum` + 31 mei is  groter dan of gelijk aan het opvraagmoment én;
>    - de `bemiddelingspecificaties` overlappen. 
>
>Indien aan deze voorwaarden is voldaan, mogen alle bijbehorende GraphQL-nodes worden opgevraagd conform de structuur van de query-template. 


## Toegangscontrole-flows Zorgkantoor:"QLR-0009-ZK of QLR-0010-ZK
Beschrijving van het autorisatieproces door de PEP.

**Schematisch:**

```mermaid
---
config:
  theme: neutral
  look: classic
---
stateDiagram
  direction TB
  state PEP {
    direction TB
    validerenT
    state PDP {
      direction TB
      validerenR --> checkInput01
      state check01 <<choice>>

      checkInput01 --> check01
      check01 --> error:nee
      state PIP {
        direction TB
        state check02 <<choice>>

        checkInput02 --> check02
        check02
        checkInput02
      }
      access
    }
  }
  [*] --> indienen
  indienen --> validerenT
  validerenT --> validerenR:access-token is geldig
  check01 --> checkInput02:ja
  check02 --> error:nee
  check02 --> access:ja
  error --> [*]
  access --> resource
  resource --> [*]
  PEP:Autorisatie controle PEP
  validerenT:Valideer access token
  PDP:Toegangscontrole PDP
  validerenR:Valideer Request
  checkInput01:Check verplichte input aanwezig?
  error:geen toegang tot Resource
  PIP:Contextinformatie controle PIP
  checkInput02:Heeft het zorgkantoor volgens Bemiddelingsregister een overlappende Bemiddelingspecificatie met de Bemiddelingspecificatie waarvoor de Levering wordt opgevraagd?
  access:toegang tot Resource
  indienen:Ontvang QLR-0009-ZK of QLR-0010-ZK + Access token
  resource:Query mag door naar Leveringsregister
  style validerenR,checkInput01,checkInput02 fill:#FFD600
  style error fill:#D50000
  style access,resource fill:#00C853
  style indienen fill:#BBDEFB,color:none
  ```

  | # | Toelichting |
  |:--- | :--- |
  | 1. | Ontvangst GraphQL-request + acces-token door **PEP**. |
  | 2. | De **PEP** valideert de acces-token en geeft na goedkeur het request door aan de PDP. |
  |3. | De **PDP** controleert op: <ol><li> Of het request voldoet aan de template en er geen ongeoorloofde gegevens worden opgevraagd; <li> Aanwezigheid van de verplichte parameters in het request. </ol> Is aan alle voorwaarden voldaan? <br/> - **Ja** -> Controle context-informatie door **PIP**: stap 4. <br/> - **Nee** -> geen toegang tot de resource - *Einde proces (geen toegang)*. |
  | 4. | De **PIP** controleert in het `Bemiddelingsregister` op de aanwezigheid van een `Bemiddelingspecificatie` waarbij: <ol><li> Het `uitvoerendZorgkantoor` overeenkomt met de `uzovicode` uit de acces-token **én** <li> De `bemiddelingspecificatie` hoort tot een `Bemiddeling` waarook de `bemiddelingspecificatie` waarvoor de `levering` opgevraagd is bij hoort **én** <li> De `bemiddelingspecificatie` een `toewijzingEinddatum` heeft die leeg is óf de `toewijzingEinddatum` + 31 mei is groter dan of gelijk aan het opvraagmoment **én** <li> Er overlap is tussen de beide `bemiddelingspecificaties`. </ol> Is aan de voorwaarde voldaan? <br/> - **Ja** -> Toegang tot de resource: stap 5. <br/> - **Nee** -> geen toegnag tot de resource - *Einde proces (geen toegang)*. |
  | 5. | het zorgkantoor krijgt toegang tot alle entiteiten die bij de Wlz-indicatie horen. |
  | 6. | *Einde* | 

  ## Toegangscontrole PIP

Voor QLR-0009-ZK

  ```gql
  query Bemiddelingspecificatie(
    $bemiddelingspecificatieIDEigen: UUID! # afkomstig uit query
    $uitvoerendZorgkantoor: String! # afkomstig uit acces-token
    $toewijzingIngangsdatum: Date! # afkomstig uit query
    $vaststellingMoment: DateTime! # afkomstig uit query
    $dagVaststellingMoment: Date! # afkomstig uit query
    $toewijzingEinddatum: Date! # afkomstig uit query
    $bemiddelingID: UUID! # afkomstig uit query
  ) {
    bemiddelingspecificatie(
        where: {
            bemiddelingspecificatieID: {eq: $bemiddelingspecificatieIDEigen}
            uitvoerendZorgkantoor: {eq: $uitvoerendZorgkantoor}
            toewijzingIngangsdatum: {eq: $toewijzingIngangsdatum}
            vaststellingMoment: {eq: $vaststellingMoment}
            toewijzingEinddatum: {eq: $toewijzingEinddatum}
            and: [ {
                bemiddeling: {
                    and: {
                        bemiddelingID: {eq: $bemiddelingID}
                    }
                }
            }]
         }
    ) {
        bemiddelingspecificatieID
        bemiddeling{
            bemiddelingID
            bemiddelingspecificatie(
                where: {
                    and: [
                    {
                       or: [{ toewijzingEinddatum: { eq: null } }, 
                       { toewijzingEinddatum: { gte: $toewijzingIngangsdatum } },
                       { toewijzingEinddatum: { gte: $dagVaststellingMoment } }]
                    }
                    { toewijzingIngangsdatum: { ngt: $toewijzingEinddatum } }
                ]
            }
        ){
                bemiddelingspecificatieID
        }
    }
   }
  }
  ```

Voor QLR-0010-ZK
```gql

 query Bemiddelingspecificatie(
    $bemiddelingspecificatieIDEigen: UUID! # afkomstig uit query
    $uitvoerendZorgkantoor: String! # afkomstig uit acces-token
    $toewijzingIngangsdatum: Date! # afkomstig uit query
    $vaststellingMoment: DateTime! # afkomstig uit query
    $dagVaststellingMoment: Date! # afkomstig uit query
    $bemiddelingID: UUID! # afkomstig uit query
  ) {
    bemiddelingspecificatie(
        where: {
            bemiddelingspecificatieID: {eq: $bemiddelingspecificatieIDEigen}
            uitvoerendZorgkantoor: {eq: $uitvoerendZorgkantoor}
            toewijzingIngangsdatum: {eq: $toewijzingIngangsdatum}
            vaststellingMoment: {eq: $vaststellingMoment}
            toewijzingEinddatum: {eq: null}
            and: [ {
                bemiddeling: {
                    and: {
                        bemiddelingID: {eq: $bemiddelingID}
                    }
                }
            }]
         }
    ) {
        bemiddelingspecificatieID
        bemiddeling{
            bemiddelingID
            bemiddelingspecificatie(
                where: {
                    and: [
                    {
                       or: [{ toewijzingEinddatum: { eq: null } }, 
                       { toewijzingEinddatum: { gte: $toewijzingIngangsdatum } },
                       { toewijzingEinddatum: { gte: $dagVaststellingMoment } }]
                    }
                    { toewijzingIngangsdatum: { ngt: $toewijzingEinddatum } }
                ]
            }
        ){
                bemiddelingspecificatieID
        }
    }
   }
  }




  