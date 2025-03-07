+++
date        = "2021-09-28"
title       = "Comunicació proveïdors d'aplicacions amb Suport Cloud"
description = "Canals de comunicació disponibles pels proveïdors d'aplicacions amb Suport Cloud"
sections    = "Cloud"
categories  = ["cloud"]
weight      = 6
+++

S'oficialitzen el [**CSTD (Centre de Suport Tecnològic al Desenvolupament)**](https://cstd.ctti.gencat.cat/jiracstd/browse/ACOCLD) i [**Remedy**](https://pautic.gencat.cat/) com a canals de comunicació amb l'equip de Suport Cloud. A continuació es descriu quin és l'ús que s'ha de fer de cadascun d'ells:

<br/>

#### Aplicacions en servei

Per les aplicacions que estiguin en servei:

- **Remedy**

_Incidències_, _Consultes_ (*) i _Canvis_ que es vulguin fer arribar a l'equip de Suport Cloud s'han de crear a Remedy informant l'aplicació afectada com a "Service" i "AM10_19-N3-CLOUD" com a "Assigned Group".

(*) per les consultes cal informar el camp “INCIDENT TYPE” el valor “USER SERVICE REQUEST”.

<div class="message information">
Donat que a les plataformes Cloud gestionades des de Suport Cloud es dóna accés a <b>logs</b> i <b>monitoratge</b> de l'aplicació, per l'agilitat en la resolució de les incidències és vital que el proveïdor d'aplicacions hagi revisat abans tota la informació de la qual disposa. També gràcies als <b>jobs de desplegament</b> del <a href="http://canigo.ctti.gencat.cat/sic/">SIC</a> és autònom per redesplegar el servei afectat. Per tant, només haurien d'arribar incidències a Suport Cloud relacionades amb la plataforma (Ex. indisponibilitat global de la plataforma), que ni tan sols redesplegant el servei puguin resoldre's.
</div>

<br/>

#### Aplicacions en fase de projecte

Per les aplicacions en fase de projecte la comunicació s'ha de fer via **CSTD** al servei [**Servei Acompanyament Suport Cloud**](https://cstd.ctti.gencat.cat/jiracstd/browse/ACOCLD). El proveïdor d'aplicacions ha de crear una petició en aquest servei informant el camp Sumari "Suport projecte NOM_PROJECTE". Mentre l'aplicació estigui en fase de projecte tot el suport (Ex. definició d'imatges Docker, definició de descriptors de desplegament,...) es canalitzarà en aquesta petició.

<br/>

#### Aplicacions que no han iniciat el procés d'alta dins CTTI

_Consultes_ i _Suports_ han de realitzar-se al servei [**CS Suport Cloud**](https://cstd.ctti.gencat.cat/jiracstd/browse/CLD).

<br/>

En cas que no es disposi d'usuari al CSTD, cal que el gestor de projecte CTTI de l'aplicació faci arribar la petició d'alta d'usuari al propi [servei CSTD](https://cstd.ctti.gencat.cat/jiracstd/browse/CSTD) o bé enviant un correu a la bústia [cstd.ctti@gencat.cat](mailto:cstd.ctti@gencat.cat).
