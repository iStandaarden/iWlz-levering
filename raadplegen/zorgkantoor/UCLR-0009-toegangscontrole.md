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
    - De parameters zoals hierboven aanwezig zijn;
    - De **access-token** bevat een geldige `uzovicode`van het zorgkantoor;
    - De `uzovicode` van de in de query meegegeven `uitvoerendZorgkantoor`komt overeen met de `uzovicode`in de acces-token;
    - In het **Bemiddelingsregister** bestaat er een `Bemiddelingspecificatie` waarbij:<br>
        1. het `uitvoerendZorgkantoor` overeenkomt met de `uzovicode`uit de acces-token **én;**<br>
        2. deze `bemiddelingspecificatie` behoort tot dezelfde `Bemiddeling` als waar de `bemiddelingspecificatie` waarvoor de `Levering` opgevraagd wordt ook bij hoort **én;**<br>
        3. deze `bemiddelingspecificaties`overlappen in periode met elkaar **én;**<br>
        4. de `toewijzingEinddatum` is leeg of de `toewijzingEinddatum` + 31 mei is  groter dan of gelijk aan het opvraagmoment.    
 

### Resultaat 

> Toegang tot het Leveringsregister via query [QLR-0009-ZK](/gql-query/zorgkantoor/QLR-0009-ZK.graphql) is **alleen toegestaan** als:
>- De relevante parameters aanwezig zijn per query;
>- De acces-token bevat een geldige `uzovicode`;
>- Er een `Bemiddelingspecificatie` is voor:
>   - de uzovicode (uit de acces-token) én;
>   - die hoort bij dezelfde `Bemiddeling` als de `bemiddelingspecificatie` waarvoor de `levering` opgevraagd wordt én;
>   - die overlapt met de `bemiddelingspecificatie` waarvoor de `levering` opgevraagd wordt
>
>Indien aan deze voorwaarden is voldaan, mogen alle bijbehorende GraphQL-nodes worden opgevraagd conform de structuur van de query-template. 


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
  checkInput02: Heeft het zorgkantoor een Bemiddelingspecificatie met overlap met de Bemiddelingspecificatie waarvan de Levering wordt opgevraagd?
  access:toegang tot Resource
  indienen:Ontvang QLR-0009-ZK + Access token
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
  | 4. | De **PIP** controleert in het `Bemiddelingsregister` op de aanwezigheid van een `Bemiddelingspecificatie` voor het raadplegende zorgkantoor dat overlap heeft met de `Bemiddelingspecificatie` waarvoor de Levering(status) wordt geraadpleegd:<BR/>Hiervoor zijn er twee PIP-requests nodig:<BR/><ol><BR/><li> PIP-context data: Haal context-informatie op van de `Bemiddelingspecificatie` waarvoor de Levering(status) geraadpleegd wordt;<BR/><li> PIP-context validatie: Gebruik de context-informatie uit het PIP-request onder 1 en voeg deze toe aan het PIP-request om te bepalen of er een `Bemiddelingspecificatie is voor het raadplegende zorgkantoor met overlap.<BR/></ol><BR/> Is er (minimaal) één `Bemiddelingspecificatie` voor het raadplegende zorgkantoor aanwezig? <BR/><BR/>- **Ja**  -> Toegang tot de resource: stap 5. <BR/>- **Nee** -> Geen toegang tot de resource - *Einde proces (geen toegang)*. |
  | 5. | Het zorgkantoor krijgt toegang tot `Levering`, met bijbehorende `Leveringperiode`, `Behandelingperiode`, `Uitstelperiode` en `Afstel`. |
  | 6. | *Einde* | 

  ## Toegangscontrole PIP

### 1. Ophalen Context data Bemiddelingspecificatie
```gql
# Raadplegen PIP contextdata
# Haal context data op voor de Bemiddelingspecificatie waar inzage in de levering gewenst is.
# Gebruik deze context data in de toegangscontrole "PIPcontextBSvalidatie"

    query PIPcontextBSdata(
    $bemiddelingspecificatieID: UUID! # bemiddelingspecificatieID uit initiele raadpleging
    ) {
    bemiddelingspecificatie(
        where: {bemiddelingspecificatieID: {eq: $bemiddelingspecificatieID}}
    ) {
        bemiddelingspecificatieID
        toewijzingIngangsdatum
        toewijzingEinddatum
        vaststellingMoment
    }
    }

```

### 2. PIP context validatie
Validatie aanwezigheid *Eigen* Bemiddelingspecificatie met overlap op te vragen Bemiddelingspecificatie (Informatieve)

```gql
# Op basis van de gegevens van de bemiddelingsspecificatie waarvan de leveringstatus geraadpleegd wordt,
# controleren of er voor het raadplegende zorgkantoor een bemiddelingspecifcatie is dat overlapt heeft.
# Als het resultaat leeg is, bestaat er geen geldige Bemiddelingspecificatie met overlap
# voor het raadplegende zorgkantoor.

    query PIPcontextBSvalidatie(
    $bemiddelingspecificatieID: UUID!    # bemiddelingspecificatieID uit initiele query
    $uitvoerendZorgkantoorToken: String! # afkomstig uit token
    $toewijzingIngangsdatum: Date!       # toewijzingIngangsdatum uit PIPcontextdata
    $toewijzingEinddatum: Date           # eventueel toewijzingEinddatum uit PIPcontextdata
    $toewijzingEinddatumMoment: DateTime # als er een einddatum is + T00:00:00.000+01:00
    $datumvaststellingMoment: Date!      # datumdeel vaststellingsmoment
    ) {
    bemiddelingspecificatie(
        where: {bemiddelingspecificatieID: {eq: $bemiddelingspecificatieID}}
    ) {
        bemiddelingspecificatieID
        uitvoerendZorgkantoor
        vaststellingMoment
        toewijzingIngangsdatum
        toewijzingEinddatum
        vaststellingMoment
        bemiddeling {
        bemiddelingID
        bemiddelingspecificatie(
            where: {
            and: [
                # Er moet een eigen.bemiddelingspecificatie zijn voor opvragende zorgkantoor
                {uitvoerendZorgkantoor: {eq: $uitvoerendZorgkantoorToken}}
                # eigen.bspec.toewijzingIngangsdatum lte opgevraagde.Bspec.toewijzingEinddatum of
                # eigen.bspec.vaststellingsmoment lte opgevraagde.bspec.toewijzingeinddatum
                {
                or: [
                    {toewijzingIngangsdatum: {lte: $toewijzingEinddatum}}
                    {vaststellingMoment: {lte: $toewijzingEinddatumMoment}}
                ]
                }
                # eigen.bspec.toewijzingEinddatum is null of
                # eigen.bspec.toewijzingEinddatum gte opgevraagde.bspec.toewijzingIngangsdatum of
                # eigen.bspec.toewijzingEinddatum gte opgevraagde.bspec.vaststellingMoment
                {
                or: [
                    {toewijzingEinddatum: {eq: null}}
                    {toewijzingEinddatum: {gte: $toewijzingIngangsdatum}}
                    {toewijzingEinddatum: {gte: $datumvaststellingMoment}}
                ]
                }
                # die toegang geldt t/m 31 mei van het jaar dat volgt op de einddatum van de eigen Bemiddelingspecificatie.
            ]
            }
        ) {
            bemiddelingspecificatieID
        }
        }
    }
    }

```

