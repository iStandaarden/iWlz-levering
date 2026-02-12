# Autorisatiematrix Leveringsregister 1
*Leveringsregister 1 - versie 1.0-rc1: 29-01-2026*

In de autorisatiematrix is per autorisatieregel de toegang op attribuutniveau vastgelegd.

[Informatiemodel Leveringsregister 1](https://informatiemodel.istandaarden.nl/informatiemodel/iwlz/netwerk/leveringsregister-1/)

Ga direct naar: - [Autorisatie Zorgaanbieder](#ZA) - [Autorisatie Zorgkantoor](#ZK)

| **ENTITEIT/ATTRIBUUT** 	| **[LRA0001](https://informatiemodel.istandaarden.nl/informatiemodel/iwlz/netwerk/leveringsregister-1/regels/autorisatieregel/lra0001/)** 	| **[LRA0002](https://informatiemodel.istandaarden.nl/informatiemodel/iwlz/netwerk/leveringsregister-1/regels/autorisatieregel/lra0002/)** 	| **[LRA0003](https://informatiemodel.istandaarden.nl/informatiemodel/iwlz/netwerk/leveringsregister-1/regels/autorisatieregel/lra0003/)** 	| **[LRA0004](https://informatiemodel.istandaarden.nl/informatiemodel/iwlz/netwerk/leveringsregister-1/regels/autorisatieregel/lra0004/)** 	| **[LRA0005](https://informatiemodel.istandaarden.nl/informatiemodel/iwlz/netwerk/leveringsregister-1/regels/autorisatieregel/lra0005/)** 	| **[LRA0006](https://informatiemodel.istandaarden.nl/informatiemodel/iwlz/netwerk/leveringsregister-1/regels/autorisatieregel/lra0006/)** 	|
|---:	|:---:	|:---:	|:---:	|:---:	|:---:	|:---:	|
| **Raadpleger** 	| <h5 id="ZK">**zorgkantoor**</h5> 	| **zorgkantoor** 	| **zorgkantoor** 	| **zorgkantoor** 	| <h5 id="ZA">**zorgaanbieder**</h5> 	| **zorgaanbieder** 	|
| **Client** 	|  	|  	|  	|  	|  	|  	|
| clientID 	| R 	| R 	| R 	| R 	| R 	| R 	|
| bsn 	| R 	| R 	| R 	| R 	| R 	| R 	|
|  	|  	|  	|  	|  	|  	|  	|
| **Levering** 	|  	|  	|  	|  	|  	|  	|
| leveringID 	| R 	| R 	| R 	| R 	| R 	| R 	|
| bemiddelingspecificatieID 	| R 	| R 	| R 	| R 	| R 	| R 	|
|  	|  	|  	|  	|  	|  	|  	|
| **Leveringperiode** 	|  	|  	|  	|  	|  	|  	|
| leveringperiodeID 	| R 	| R 	|  	| R 	| R 	|  	|
| ingangsdatum 	| R 	| R 	|  	| R 	| R 	|  	|
| vaststellingMoment 	| R 	| R 	|  	| R 	| R 	|  	|
| sleuteldatum 	| R 	| R 	|  	| R 	| R 	|  	|
| einddatum 	| R 	| R 	|  	| R 	| R 	|  	|
| beeindigingReden 	| R 	| R 	|  	| R 	| R 	|  	|
| aanbiederBestemming 	| R 	| R 	|  	| R 	| R 	|  	|
|  	|  	|  	|  	|  	|  	|  	|
| **BehandelingPeriode** 	|  	|  	|  	|  	|  	|  	|
| behandelingPeriodeID 	| R 	| R 	|  	| R 	| R 	|  	|
| ingangsdatum 	| R 	| R 	|  	| R 	| R 	|  	|
| einddatum 	| R 	| R 	|  	| R 	| R 	|  	|
|  	|  	|  	|  	|  	|  	|  	|
| **Uitstelperiode** 	|  	|  	|  	|  	|  	|  	|
| uitstelperiodeID 	| R 	| R 	|  	| R 	| R 	|  	|
| ingangsdatum 	| R 	| R 	|  	| R 	| R 	|  	|
| vaststellingMoment 	| R 	| R 	|  	| R 	| R 	|  	|
| reden 	| R 	| R 	|  	| R 	| R 	|  	|
| leveringstatus 	| R 	| R 	|  	| R 	| R 	|  	|
| leveringstatusClassificatie 	| R 	| R 	|  	| R 	| R 	|  	|
| einddatum 	| R 	| R 	|  	| R 	| R 	|  	|
|  	|  	|  	|  	|  	|  	|  	|
| **Afstel** 	|  	|  	|  	|  	|  	|  	|
| afstelID 	| R 	| R 	|  	| R 	| R 	|  	|
| afsteldatum 	| R 	| R 	|  	| R 	| R 	|  	|
| vaststellingMoment 	| R 	| R 	|  	| R 	| R 	|  	|
| reden 	| R 	| R 	|  	| R 	| R 	|  	|
| aanbiederBestemming 	| R 	| R 	|  	| R 	| R 	|  	|
|  	|  	|  	|  	|  	|  	|  	|
| **Verzoek** 	|  	|  	|  	|  	|  	|  	|
| verzoekID 	|  	|  	| R 	|  	|  	| R 	|
| verzoekdatum 	|  	|  	| R 	|  	|  	| R 	|
| product 	|  	|  	| R 	|  	|  	| R 	|
| toewijzingSoort 	|  	|  	| R 	|  	|  	| R 	|
| leveringsvorm 	|  	|  	| R 	|  	|  	| R 	|
| volume 	|  	|  	| R 	|  	|  	| R 	|
| eenheid 	|  	|  	| R 	|  	|  	| R 	|
| frequentie 	|  	|  	| R 	|  	|  	| R 	|
| doelmatig 	|  	|  	| R 	|  	|  	| R 	|
| toeslagreden 	|  	|  	| R 	|  	|  	| R 	|
| verantwoord 	|  	|  	| R 	|  	|  	| R 	|
|  	|  	|  	|  	|  	|  	|  	|
| **VerzoekAanbieder** 	|  	|  	|  	|  	|  	|  	|
| verzoekAanbiederID 	|  	|  	| R 	|  	|  	| R 	|
| aanbieder 	|  	|  	| R 	|  	|  	| R 	|
| toewijzingIngangsdatum 	|  	|  	| R 	|  	|  	| R 	|
| toewijzingEinddatum 	|  	|  	| R 	|  	|  	| R 	|
| toewijzingPercentage 	|  	|  	| R 	|  	|  	| R 	|
| opname 	|  	|  	| R 	|  	|  	| R 	|
| regiehouder 	|  	|  	| R 	|  	|  	| R 	|

