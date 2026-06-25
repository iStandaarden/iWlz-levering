# iWlz-levering 1

> [!IMPORTANT] 
> Hier volgt de koppelvlakspecificatie van het **Leveringsregister 1.** Naast het GraphQL-schema komen hier de beschrijvingen van de notificaties, query-templates met beschrijving en toegangscontroles
>
> Het informatiemodel is op 29-01-2026 gepubliceerd en is te vinden via het Informatiemodel op: https://informatiemodel.istandaarden.nl/
> 
> Deze pagina zal de komende weken worden aangevuld, te beginnen met het schema, de notificaties, de queries en vervolgens de usecases en bijbehorende toegangscontroles. 
>
> **Omdat de technische haalbaarheid van de vereiste toegangscontrole van de autorisatieregels en autorisatiematrix op het raadplegen van het leveringsregister nog moet worden bepaald kunnen de specificaties nog wijzigen.**
> 


**iWlz-Levering bevat de [Graphql-schema](/gql-specificatie) koppelvlak specificatie en de [GraphQL-query templates](/gql-query/) voor het raadplegen van Wlz zorgleveringsgegevens in het leveringsregister.**

Het leveringsregister is in beheer bij de zorgaanbieder en is de bronhouder. Het leveringsregister is onderdeel van het iWlz-netwerkmodel.

> [!NOTE] 
> [![Latest Release](https://img.shields.io/github/v/release/iStandaarden/iWlz-Levering?label=latest%20release&style=flat-square)](https://github.com/iStandaarden/iWlz-levering/releases)
> ![Release Date](https://img.shields.io/github/release-date/iSTandaarden/iWlz-Levering?style=flat-square)

---
**Inhoud**
- [iWlz-levering 1](#iwlz-levering-1)
  - [Informatiemodel Leveringsregister](#informatiemodel-leveringsregister)
  - [Afsprakenstelsel Netwerkmodel](#afsprakenstelsel-netwerkmodel)
  - [Onderdelen koppelvlakspecificatie](#onderdelen-koppelvlakspecificatie)
    - [Graphql-schema](#graphql-schema)
    - [Graphql-query](#graphql-query)
    - [Open Agent Policy](#open-agent-policy)
    - [Notificaties](#notificaties)
  - [**Raadplegen Leveringsregister**](#raadplegen-leveringsregister)
    - [Scopes - Leveringsregister](#scopes---leveringsregister)
      - [Raadplegen van het Leveringsregister](#raadplegen-van-het-leveringsregister)
      - [Notificeren](#notificeren)
      - [Melden aan het zorgkantoor](#melden-aan-het-zorgkantoor)
  - [Autorisatieregels en autorisatiematrix](#autorisatieregels-en-autorisatiematrix)
  - [Versies en Status](#versies-en-status)
  - [Meer informatie](#meer-informatie)
  - [Contactpersonen:](#contactpersonen)

---
## Informatiemodel Leveringsregister
De specificaties van de andere onderdelen, zoals ERD, regels, procesbeschrijving, autorisatieregels zijn beschreven in het in het [**Informatiemodel iWlz**](https://informatiemodel.istandaarden.nl/) dat te vinden is via de website: [https://informatiemodel.istandaarden.nl/](https://informatiemodel.istandaarden.nl/). 

## Afsprakenstelsel Netwerkmodel
Het afsprakenstelsel heeft als doel deelnemers aan het iWlz-netwerkmodel op een uniforme en eenduidige wijze te informeren over de geldende afspraken, procedures en regels. Het vormt daarmee de basis voor samenwerking en gegevensuitwisseling binnen het iWlz-netwerkmodel.

Het hoofdstuk [Applicatie](https://wlz.atlassian.net/wiki/x/cghgAQ) beschrijft de invulling van de applicatie laag van de architectuur op een generieke manier en bevat een gedetailleerde toelichting op de onderdelen die hieronder zijn beschreven. 

Het [**Afsprakenstel iWlz Netwerkmodel**](https://wlz.atlassian.net/wiki/spaces/IWLZAS/overview) is te vinden via deze link: [https://wlz.atlassian.net/wiki/spaces/IWLZAS/overview](https://wlz.atlassian.net/wiki/spaces/IWLZAS/overview)


## Onderdelen koppelvlakspecificatie
Deze specificaties van het Leveringsregister maken onderdeel uit van de **iStandaard iWlz**. 


**Schematisch overzicht onderdelen koppelvlakspecificatie**
![onderdelen](/src/Onderdelen_Netwerk.png)
v.l.n.r. Raadpleger doet via GraphQL-query een raadpleging. Open Policy agent controleert of query voldoet aan autorisatie-regels van dat register. GraphQL-schema definieert het data-schema van het register.

### Graphql-schema 
De [Graphql-schema specificatie](/gql-specificatie/) is bedoeld voor implementatie door de bronhouder en beschrijft hoe de data aan elkaar is gerelateerd. De specificatie is te vinden in de folder [/gql-specificatie](/gql-specificatie/). 

### Graphql-query
De [Graphql-queries](/gql-query/) beschrijven het template hoe een raadpleger vanuit zijn rol informatie kan raadplegen. Deze template volgt altijd het GraphQL-schema maar moet op bepaalde momenten aan vaste patronen voldoen vanwege de geldende autorisatie. Gaat een raadpleger buiten dit patroon dan zal de vraag worden afgekeurd en krijgt de raadpleger geen inzicht in de data. 

>  [!NOTE]
In de folder [**/gql-query**](/gql-query/) staat een overzicht van de beschikbare templates inclusief een toelichting voor welke partij de template is.

> [!TIP]
> Voor een beschrijving van de situatie wanneer een query moet worden toegepast of in welke volgorde om toegang te krijgen tot de gegevens ga dan eerst naar de beschrijving onder [Raadplegen Leveringsregister](#raadplegen-leveringsregister).


### Open Agent Policy
De Open Agent Policy controleert of een query voldoet aan de daarvoor afgesproken template. De policy is gebaseerd op de autorisatieregels van dat register. 

> [!Note]
> Momenteel zijn de policies nog niet publiekelijk beschikbaar. 


### Notificaties
Met een notificatie wordt een netwerk-deelnemer op de hoogte gebracht door een bronhouder dat er nieuwe (of gewijzigde) informatie is die directe of afgeleide betrekking heeft op die deelnemer. De notificatie bevat informatie die de deelnemer in staat stelt de relevante informatie te raadplegen bij de bron. Een notificatie loopt altijd van bron naar deelnemer.

> [!NOTE]
> De notificaties vanuit het Leveringsregister zijn te vinden in de folder [**/notificaties**](/notificaties/)

## **Raadplegen Leveringsregister**

Het raadplegen van het Leveringsregister is gebonden aan voorwaarden. De raadpleger moet bevoegd zijn én het vastgestelde raadpleegpatroon volgen. Dit patroon is essentieel voor het valideren van de toestemming. 

Als dat patroon niet wordt gevolgd — bijvoorbeeld door ontbrekende autorisatie, onjuiste of incomplete input, of het opvragen van ongeoorloofde gegevens — wordt de toegang geweigerd of het resultaat beperkt.

Use-cases beschrijven hoe een deelnemer het register correct raadpleegt.

> [!NOTE]
> De functionele beschrijving beschikbare raadpleging per deelnemer is beschikbaar in de folder **[/raadplegen](/raadplegen/)**

Meer informatie over de structuur van het raadplegen en het valideren ervan is te lezen in het [Afsprakenstelsel iWlz - Raadplegen](https://wlz.atlassian.net/wiki/x/KgpgAQ)

### Scopes - Leveringsregister
Het Access-token bevat specifieke gegevens die worden gebruikt om de toegang tot resources te autoriseren en te valideren. Een van de gegevens is de scope. Voor het Leveringsregister zijn er momenteel de volgende scopes actief.

####  Raadplegen van het Leveringsregister

Voor het mogen raadplegen van het leveringsregister moet de volgende scope worden meegegeven:

>   `registers/leveringsregister/leveringen:read`

#### Notificeren 

Voor het mogen notificeren door een aanbieder aan een zorgkantoor of een andere aanbieder moet er respectievelijk een van de volgende scopes worden meegegeven:

> `organisaties/zorgkantoor/notificaties/notificatie:create`

of

> `organisaties/zorgaanbieder/notificaties/notificatie:create`

#### Melden aan het zorgkantoor

Voor het mogen versturen van meldingen aan de aanbieder moet de volgende scope worden meegegeven:

> `organisaties/aanbieder/meldingen/melding:create`

## Autorisatieregels en autorisatiematrix
De toegang tot gegevens is vastgelegd doormiddel van **Autorisatieregels** en de **Autorisatiematrix**. De [autorisatieregels](https://informatiemodel.istandaarden.nl/informatiemodel/iwlz/netwerk/leveringsregister-1/regels/autorisatieregel/) zijn te vinden in het Informatiemodel Leveringsregister (via [hier](https://informatiemodel.istandaarden.nl/informatiemodel/iwlz/netwerk/leveringsregister-1/regels/autorisatieregel/)) en de [autorisatiematrix](/raadplegen/autorisatiematrix_leveringsregister.md) is [hier](/raadplegen/autorisatiematrix_leveringsregister.md) te vinden.


> [!NOTE]
> De Autorisatiematrix per autorisatieregel is beschikbaar in de folder **[/raadplegen](/raadplegen/autorisatiematrix_leveringsregister.md)**



## Versies en Status 

Er zijn altijd minimaal twee versies actueel. Een versie die in productie is, status is *Lopend* en een versie die in ontwikkeling is, status is *In ontwikkeling*.


| iWlz Informatiemodel | Status | versie koppelvlak |
|:-- |:-- | :-- |
| *nvt* | *Lopend* | *nvt* |
| **Leveringsregister 1** | In ontwikkeling |  |

Volledig versie overzicht zie: [Changelog](CHANGELOG.md)

> 
> **Status Koppelvlakspecificatie:** *In Ontwikkeling Leveringsregister 1* 
>
> Het Leveringsregister is nog in ontwikkeling. Momenteel is er nog geen versie in productie en ontbreekt er daarom een versie lopend.
> 



## Meer informatie
* Actieprogramma iWlz: van keten naar netwerk: [het Actieprogramma iWlz](https://www.istandaarden.nl/iwlz/actieprogramma/index "Over Actieprogramma iWlz")
* Informatiemodel iStandaarden iWlz: [Informatiemodellen](https://informatiemodel.istandaarden.nl)
* [**Afsprakenstel iWlz Netwerkmodel**](https://wlz.atlassian.net/wiki/spaces/IWLZAS/overview)
* Portaal voor iStandaarden in de
Zorg en Ondersteuning: [homepagina iStandaarden](https://www.istandaarden.nl)
* [GraphQL.org](https://graphql.org) 
* [Open Agent Policy](https://www.openpolicyagent.org) en [documentatie](https://www.openpolicyagent.org/docs/latest/)

## Contactpersonen:
* Dennis de Gouw - [@dennisdegouw](http://github.com/dennisdegouw)
* Remo van Rest - [@rvanrest](https://github.com/rvanrest)