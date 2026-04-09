# Raadplegen Leveringsregister 

> [!Caution] - Release Candidate 1 - 29-01-2029
> Hieronder staan de eerste beschrijvingen van de basis raadplegingen op het Leveringsregister.
> 
> De raadplegingen volgen in de basis de [notificaties](/notificaties/README.md) omdat het uitgangspunt is dat de ontvanger van een notificatie op basis daarvan een raadpleging wil (kunnen) uitvoeren. 
> 
> Waar mogelijk is er een raadpleeg use-case en toegangscontrole beschrijving beschikbaar. Waar dat nog ontbreekt volgen ze zo snel mogelijk maar dat is afhankelijk of de raadpleging technisch mogelijk is rekeninghoudend met de vereiste toegangscontrole. Dit kan leiden tot aanpassing van de query en aanpassing van het schema. Daar waar dit speelt is dat afzonderlijk aangegeven. 


Het raadplegen van het Leveringsregister is gebonden aan voorwaarden. De raadpleger moet bevoegd zijn én het vastgestelde raadpleegpatroon volgen. Dit patroon is essentieel voor het valideren van de toestemming.

Als het patroon niet wordt gevolgd — bijvoorbeeld door ontbrekende autorisatie, onjuiste of incomplete input, of het opvragen van ongeoorloofde gegevens — wordt de toegang geweigerd of het resultaat beperkt.

Uses-cases beschrijven hoe een deelnemer het register correct raadpleegt. Per use-case zijn er toegangscontroles beschreven zodat de verbinding met de bijbehorende autorisatie en de benodigde policy gemaakt kan worden.


```mermaid
---
config:
  theme: mc
  look: classic
  layout: elk
---
flowchart LR
 subgraph s1["PDP"]
          T["toegangscontrole"]
 end
 subgraph s2["Raadplegen"]
          R["Use-case"]
          
  end
    A["Raadpleger"] --> R
    R --> T
    T --> Rs["Register"]
    R@{ shape: terminal}
    T@{ shape: terminal}
    A@{ shape: rounded}
    Rs@{ shape: rounded}
    
```
Meer informatie over de structuur van het raadplegen en het valideren ervan is te lezen in het [Afsprakenstelsel iWlz](https://wlz.atlassian.net/wiki/spaces/IWLZAS/pages/23071274/Raadplegen)


## Autorisatieregels en autorisatiematrix
De toegang tot gegevens is vastgelegd doormiddel van **Autorisatieregels** en de **Autorisatiematrix**. De [autorisatieregels](https://informatiemodel.istandaarden.nl/informatiemodel/iwlz/netwerk/leveringsregister-1/regels/autorisatieregel/) zijn te vinden in het Informatiemodel leveringsregister (via [hier](https://informatiemodel.istandaarden.nl/informatiemodel/iwlz/netwerk/leveringsregister-1/regels/autorisatieregel/)) en de [autorisatiematrix](/raadplegen/autorisatiematrix_leveringsregister.md) is [hier](/raadplegen/autorisatiematrix_leveringsregister.md) te vinden.


# Use cases raadplegen Leveringsregister

De use-cases voor het raadplegen van het Leveringsregister per rol en bijbehorende beschrijving van de toegangscontrole door de PDP[^1].

Kies een use-case voor de beschrijving van het raadplegen of controleren van de toegang van die raadpleging.

### Aanbieder
| Doel | toelichting | raadplegen | toegangscontrole |
| :--- | :--- | :--- | :--- |
| Status levering andere aanbieder | **Als** aanbieder **wil ik** voor het leveren van zorg of ondersteuning gegevens over de status van de levering van de zorg of ondersteuning raadplegen die horen bij de (informatieve) toewijzingen (bemiddelingspecificaties) van andere aanbieders **zodat ik** het volledige inzicht heb in de leveringen en levering beter kan afstemmen. | [UCLR-0001-raadplegen](/raadplegen/aanbieder/UCLR-0001-raadplegen.md) *(concept)* | [UCLR-0001-toegangscontrole](/iWlz-levering/raadplegen/aanbieder/UCLR-0001-toegangscontrole.md) *(concept)* |  
| VerzoekAanbieder en Verzoek | **Als** aanbieder die een notificatie over `VerzoekAanbieder` heeft ontvangen, **wil ik** VerzoekAanbieder, het bijbehorende Verzoek en de cliënt kunnen raadplegen, zodat ik op de hoogte ben van de aanvraag voor een toewijzing. | [UCLR-0002-raadplegen](/raadplegen/aanbieder/UCLR-0002-raadplegen.md) *(concept)* | [UCLR-0002-toegangscontrole](/raadplegen/aanbieder/UCLR-0002-toegangscontrole.md) *(concept)* |  

### Zorgkantoor
| Doel | toelichting | raadplegen | toegangscontrole |
| :--- | :--- | :--- | :--- |
| Nieuwe of Gewijzigde Leveringperiode | **Als** zorgkantoor **wil ik** de status van de Leveringsperiode raadplegen (naar aanleiding van de notificatie die ik heb ontvangen) **zodat ik** de zorgstatus van een toewijzing (bemiddelingspecificatie) kan beoordelen. | [UCLR-0006-raadplegen](/raadplegen/zorgkantoor/UCLR-0006-raadplegen.md) *(concept)* | [UCLR-0006-toegangscontrole](/raadplegen/zorgkantoor/UCLR-0006-toegangscontrole.md) *(concept)* |
| Nieuwe of Gewijzigde Uitstelperiode | **Als** zorgkantoor **wil ik** de status van de Uitstelperiode raadplegen (naar aanleiding van de notificatie die ik heb ontvangen) **zodat ik** de zorgstatus van een toewijzing (bemiddelingspecificatie) kan beoordelen. | [UCLR-0007-raadplegen](/raadplegen/zorgkantoor/UCLR-0007-raadplegen.md) *(concept)* | [UCLR-0007-toegangscontrole](/raadplegen/zorgkantoor/UCLR-0007-toegangscontrole.md) *(concept)* |
| Nieuw of Gewijzigd Afstel | **Als** zorgkantoor **wil ik** de status van de Afstel raadplegen (naar aanleiding van de notificatie die ik heb ontvangen) **zodat ik** de zorgstatus van een toewijzing (bemiddelingspecificatie) kan beoordelen. | [UCLR-0008-raadplegen](/raadplegen/zorgkantoor/UCLR-0008-raadplegen.md) *(concept)* | [UCLR-0008-toegangscontrole](/raadplegen/zorgkantoor/UCLR-0008-toegangscontrole.md) *(concept)* |
| Nieuw of Gewijzigd Verzoek | **Als** verantwoordelijk zorgkantoor **wil ik** het Verzoek en bijbehorende VerzoekAanbieders kunnen raadplegen **zodat ik** de client naar de juiste zorg kan toeleiden. | [UCLR-0004-raadplegen](/raadplegen/zorgkantoor/UCLR-0004-raadplegen.md) *(concept)* | [UCLR-0004-toegangscontrole](/raadplegen/zorgkantoor/UCLR-0004-toegangscontrole.md) *(concept)*  |
| Status Levering van informatieve bemiddelingspecificaties | **Als**  uitvoerend zorgkantoor **wil ik** de status van de levering kunnen raadplegen horend bij een informatieve toewijzing (bemiddelingspecificaties), **zodat ik** inzicht heb in de leveringen die horen bij de (informatieve) toewijzingen van andere zorgkantoren. | [UCLR-0009_10-raadplegen](/raadplegen/zorgkantoor/UCLR-0009_10-raadplegen.md) *(concept)*  | [UCLR-0009_10-toegangscontole](/raadplegen/zorgkantoor/UCLR-0009_10-toegangscontrole.md) *(concept)* |



---
Terug naar [HOME](/README.md)

[^1]: PDP: Policy Decision Point. [Afsprakenstelsel iWlz - Raadplegen](https://wlz.atlassian.net/wiki/spaces/IWLZAS/pages/23071274/Raadplegen)
