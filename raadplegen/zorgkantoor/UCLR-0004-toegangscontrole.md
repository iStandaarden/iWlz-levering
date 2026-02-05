# Toegangscontrole: Raadpleeg Verzoek door verantwoordelijk zorgkantoor (UCLR-0004-ZK)

Beschrijving van de **toegangscontrole** door de Policy Decision Point (DPD) en indien van toepassing Policy Information Point (PIP).

N.b. Het valideren van de Acces-token door de PEP is geen onderdeel van deze beschrijving. Zie hiervoor het [Afsprakenstelsel iWlz - nID netwerkstelsel - 5. policy Enforcement Point](https://wlz.atlassian.net/wiki/spaces/IWLZAS/pages/229441537/nID+netwerkstelsel#5.-Policy-Enforcement-Point-(PEP)).

## Toegangscontrole PDP

### Subject
- **Entiteit:** Zorgkantoor
- **Kenmerk:** In bezit van een acces-token met daarin de eigen `uzovicode`

### Action
- **Type:** `raadplegen` (read)
- **Omschrijving:** Uitvoeren van GraphQL-query [QLR-0004-ZK](/iWlz-levering/gql-query/zorgkantoor/QLR-0004-ZK.graphql) op het Leveringsregister door een zorgkantoor.

### Resource 
- **Type:** `Leveringsregister`
- **ID:** `verzoekID`
- **Beperking:** Alleen toegang tot gegeven over het Verzoek die horen bij een bemiddelingspecificatie waarvoor het zorgkantoor verantwoordelijk is.
- **Inhoud:** De node Verzoek en de gerelateerde nodes VerzoekAanbieder, Levering en Client.

### Context 
- **Query-parameters vereist:** Het `verzoekID` moet aanwezig zijn in de query.
- **Toegangsvoorwaarde:** Er is alleen toegang als aan alle volgende voorwaarde is voldaan:
    - De parameter `verzoekID` is meegegeven in de query
    - De acces-token bevat een geldige `uzovicode`
    - De `uzovicode` in de acces-token komt overeen met `verantwoordelijkZorgkantoor` in `Bemiddeling` die hoort bij de `bemiddelingspecificatie` waarvan het `verzoekID` is meegegeven in de query.

    ## Resultaat
    > Toegang tot het Leveringsregister via QLR-0004-ZK is alleen toegestaan als:
    > - Parameter `verzoekID` is meegegeven in de query
    > - In het bemiddelingregister een match is gevonden tussen:
    >   - De `uzovicode` (uit de acces-token)
    >   - En `verantwoordelijkZorgkantoor` in `Bemiddeling` die hoort bij `bemiddelingspecificatie` waarvoor 

