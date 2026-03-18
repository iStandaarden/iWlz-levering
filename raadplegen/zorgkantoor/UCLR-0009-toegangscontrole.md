## Toegangscontrole: Raadplegen Levering van een informatieve bemiddelingspecificaties (UCLR-0009)

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

   
    
    
  