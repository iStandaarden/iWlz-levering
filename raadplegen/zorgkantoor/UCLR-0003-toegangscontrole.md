# Toegangscontrole: Raadplegen van Levering door Zorgkantoor (UCLR-0003-ZK)

Beschrijving van de **toegangscontrole** door de Policy Decision Point (PDP) en indien van toepassing Policy Information Point (PIP).

N.b. Het valideren van de Acces-token door de PEP is geen onderdeel van deze beschrijving. Zie hiervoor het [Afsprakenstelsel iWlz - nID netwerkstelsel - 5. policy Enforcement Point](https://wlz.atlassian.net/wiki/spaces/IWLZAS/pages/229441537/nID+netwerkstelsel#5.-Policy-Enforcement-Point-(PEP)).

## Toegangscontrole PDP

### Subject
- **Eniteit:** Zorgkantoor
- **Kenmerk:** In bezit van een acces-token met daarin de eigen `uzovicode`

### Acion
- **Type:** `raadplegen` (read)
- **Omschrijving:** Uitvoeren van GraphQL-query [QLR-0003-ZK](/gql-query/zorgkantoor/QLR-0003-ZK.graphql) op het Leveringsregister door een zorgkantoor.

### Resource
- **Type:** `Leveringsregister`
- **ID:** `bemiddelingspecificatieID`
- **Beperking:** Alleen toegang tot gegevens over de Levering die horen bij een bemiddelingspecificatie waarvoor het zorgkantoor verantwoordelijk is of waarvoor het zorgkantoor door het verantwoordelijk zorgkantoor betrokken is bij de levering.
- **Inhoud:** De nodes Levering en de gerelateerde Leveringperiode, Behandelingperiode, Uitstelperiode, Afstel en Client. 

### Context
- **Query-parameters vereist:** Het `bemiddelingspecificatieID` moet aanwezig zijn in de query.
- **Toegangsvoorwaarde:** Er is alleen toegang als aan alle volgende voorwaarde is voldaan:
    - De parameter `bemiddelingspecificatieID` is meegegeven in de query
    - De acces-token bevat een geldige `uzovicode`
    - De `uzovicode` in de acces-token komt overeen met `uitvoerendZorgkantoor` in `Bemiddelingspecificatie` die hoort bij het `bemiddelingspecificatieID` aanwezig in de query of in `verantwoordelijkZorgkantoor` in `Bemiddeling` die hoort bij de `bemiddelingspecificatie` die hoort bij het `bemiddelingspecificatieID` aanwezig in de query.

### Resultaat
> Toegang tot het Leveringsregister via query [QLR-0003-ZK](/gql-query/zorgkantoor/QLR-0003-ZK.graphql) is alleen toegestaan als:
> - Parameter `bemiddelingspecificatieID` is meegegeven in de query
> - In het bemiddelingsregister een match is gevonden tussen:
>   - De `uzovicode` (uit de acces-token)
>   - En een `uitvoerendZorgkantoor` in `Bemiddelingspecificatie` die hoort bij het `bemiddelingspecificatieID`in de query **óf** een `verantwoordelijkZorgkantoor` in `Bemiddeling` die hoort bij dezelfde `Bemiddeling` als de `Bemiddelingspecificatie` waar het `bemiddelingspecificatieID` uit de query bij hoort
>
> Als aan deze voorwaarden is voldaan, mogen de volgende gegevens worden opgevraagd:
> - De `Levering` met bijbehorende `Leveringperiode`, `Behandelingperiode`, `Uitstelperiode`, `Afstel` en `Client`.

## Toegangscontrole-flows Zorgkantoor: QLR-0003-ZK

Beschrijving van het autorisatieproces door de PEP.

**schematisch**

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
  checkInput01:bemiddelingspecificatieID aanwezig?
  error:geen toegang tot Resource
  PIP:Contextinformatie controle PIP
  checkInput02:Is het zorgkantoor volgens Bemiddelingsregister betrokken bij bemiddelingspecificatie?
  access:toegang tot Resource
  indienen:Ontvang QLR-0003-ZK + Access token
  resource:Query mag door naar Leveringsregister
  style validerenR,checkInput01,checkInput02 fill:#FFD600
  style error fill:#D50000
  style access,resource fill:#00C853
  style indienen fill:#BBDEFB,color:none

  ```
| **#** | **Toelichting** | 
| --- | :--- | 
| 1. | Ontvangst GraphQL-request + acces-token door PEP. |
| 2. | De PEP valideert de acces-token en geeft na goedkeur het request door aan de PDP. |
| 3. | De PDP controleert op: <br/><ol><li> Of het request voldoet aan de template en er geen ongeoorloofde gegevens worden opgevraagd; <br/> <li> Aanwezigheid van verplichte parameters in het request. <br/></ol> Is aan alle voorwaarde voldaan? <br/> - **Ja** -> Controle context-informatie door PIP (stap 4). <br/> - **Nee** -> Geen toegang tot resource (einde proces (geen toegang)). |
| 4. | De PIP controleert in het `Bemiddelingsregister` op de aanwezigheid van een `Bemiddelingspecificatie` waarbij: <br/><ol><li> Het `zorgkantoor` overeenkomt met de `uzovicode` uit de acces-token, **én** <br/><li> De `Bemiddelingspecificatie` behoort tot een `Bemiddeling` waarvoor het `bemiddelingspecificatieID` overeenkomt met de `bemiddelingspecificatieID` in de query. |
| 5. | Het zorgkantoor krijgt toegang tot de `Levering`, met bijbehorende `Leveringperiode`, `Behandelingperiode`, `Uitstelperidoe', `Afstel` en `Client`. |
| 6. | *Einde* | 

## Toegangscontrole PIP:
```gql
Moet nog gedaan worden
```
----

Ga naar [UC beschrijving raadplegen](/raadplegen/zorgkantoor/UCLR-0003-raadplegen.md) -- Terug naar [Raadplegen](/raadplegen/README.md)
