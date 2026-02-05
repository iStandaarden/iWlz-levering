# Toegangscontrole: Raadpleeg Verzoek door verantwoordelijk zorgkantoor (UCLR-0004-ZK)

Beschrijving van de **toegangscontrole** door de Policy Decision Point (DPD) en indien van toepassing Policy Information Point (PIP).

N.b. Het valideren van de Acces-token door de PEP is geen onderdeel van deze beschrijving. Zie hiervoor het [Afsprakenstelsel iWlz - nID netwerkstelsel - 5. policy Enforcement Point](https://wlz.atlassian.net/wiki/spaces/IWLZAS/pages/229441537/nID+netwerkstelsel#5.-Policy-Enforcement-Point-(PEP)).

## Toegangscontrole PDP

### Subject
- **Entiteit:** Zorgkantoor
- **Kenmerk:** In bezit van een acces-token met daarin de eigen `uzovicode`

### Action
- **Type:** `raadplegen` (read)
- **Omschrijving:** Uitvoeren van GraphQL-query [QLR-0004-ZK](@@@) op het Leveringsregister door een zorgkantoor.

### Resource 
- **Type:** `Leveringsregister`
- **ID:** `verzoekID`
