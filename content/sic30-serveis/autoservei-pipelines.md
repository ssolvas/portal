+++
date = "2021-05-20"
title = "Autoservei de pipelines"
description = "L'Autoservei de pipelines permet als proveïdors d'aplicacions ser autònoms per a integrar al SIC les seves aplicacions."
sections = "SIC"
toc = true
taxonomies = []
weight = 5
+++


## Introducció

L'Autoservei de pipelines permet la **generació automàtica de pipelines d'automatització de la construcció i del desplegament de les aplicacions** sense, en molts casos,
la intervenció de l'equip del SIC. D'aquesta manera, els equips de cada codi d'aplicació són autònoms per a preparar i mantenir la construcció de la pipeline de desplegament
associada a cada projecte repositat al Sistema de Custòdia de Codi (Gitlab).

A continuació, entrarem en més detall sobre com funciona aquest nou servei que ofereix el SIC.

## Motivació

Cobreix les següents necessitats:

* Dotar de **flexibilitat i independència** als principals actors que intervenen en la construcció i els desplegaments de les aplicacions.
* **Incrementar ràpidament el grau d’integració al SIC de les aplicacions** evitant traspassos innecessaris d'informació i responsabilitats.
* Proporcionar un **nivell d'abstracció** que permeti ser independent de les tecnologies emprades i permeti evolucionar el producte mantenint compatibilitat amb versions anteriors.
* Proporcionar un **entorn aïllat i immutable de construcció**, que a més pugui ser utilitzat i testejat pels mateixos proveïdors.
* Facilitar la cobertura de tecnologies contemplant l’ús d’**imatges Docker pròpies** dels lots d’aplicacions.
* **Acomplir les directrius** de CTTI sobre desplegaments i gestió de canvis.

La solució ha de:

* Ser **natural** per als usuaris del servei.
* Permetre definir **tot el necessari** per a construir i desplegar l’aplicació.
* Donar cobertura a **totes les tecnologies** amb les que el SIC és actualment compatible.
* Ser **mantenible, escalable i eficient**.

## Requeriments generals

S'estableixen una sèrie de requeriments per a estar en disposició d'integrar l'aplicació mitjançant aquest servei:

* Conèixer els **entorns** on es desplegarà l'aplicació i les [**modalitats de desplegament**](/sic30-serveis/ci/#modalitats-de-desplegament) aplicables en cada cas.
* L'aplicació [**ha de ser integrable**](/sic30-serveis/ci/#matriu-de-tecnologies-de-construcció) amb el servei d'automatització de la construcció i desplegament.
* Aplicació [**ha d'estar preparada i acomplir els requeriments**](/sic30-guies/preparar-aplicacio/) per a poder ser desplegada.
* Per als entorns amb modalitat de desplegament automàtic o delegat a Cpd, disposar dels **identificadors d'infraestructures** de desplegament que proporcionarà l'equip SIC o Cpd (respectivament).
* **Col·laboració**: el proveïdor d'aplicacions i el proveïdor d'infraestructures han d'estar disposats a col·laborar i mantenir una comunicació.

## Funcionament

Generalment, a cada codi d'aplicació li correspon un proveïdor d'aplicacions i un proveïdor d'infraestructures.
Aquests dos equips **han de participar i col·laborar** per tal d'utilitzar l'autoservei de pipelines del SIC aportant
la informació necessària de la qual cadascun és responsable.

* **Arxiu de Configuració d’Aplicació (ACA)**
* **Arxiu de Configuració d’Infraestructura (ACI)**

El funcionament previst és el següent:

* Els **proveïdors d'aplicacions aportaran el seu propi arxiu de configuració (ACA)**.
* Els **proveïdors d'infraestructures aportaran el seu propi arxiu de configuració (ACI)** requerit únicament quan l'aplicació es desplega en modalitat automàtica.
* Únicament quan es creïn o modifiquin aquests arxius de configuració, s'invocarà al sistema de generació de pipelines que s'encarregarà de recuperar la informació necessària
i generar (o regenerar) la pipeline de construcció i desplegament de l'aplicació.

![Pipeline del SIC](/images/news/AutoserveiJobs-Funcionament.png)
</br>

### Configuració

Pel que fa a l'**Arxiu de Configuració de l'Aplicació (ACA)**, la informació quedarà recollida a l'arxiu `/sic/aca.yml` dins del repositori del projecte.
Es tracta d'un arxiu requerit per Autoservei de Pipelines, en format YAML, en el que s'ha d'aportar la següent informació:

* **Version**: versió de l'arxiu (independent de la versió de l'aplicació o component) que es correspondrà amb els canvis en les especificacions
de construcció i/o desplegament.

* **Info**: informació sobre l'aplicació o component, incloent-hi la seva versió funcional i una descripció.

* **Global-env**: llistat de variables globals necessàries per al desplegament de l'aplicació o component.

* **Components**: informació per a la construcció, publicació i desplegament de l'aplicació o component

* **Notifications**: definició d'adreces de correu electrònic on es notificarà la necessitat d'accions manuals i/o resultats de l'execució.

Per a més informació: [Com construir el fitxer ACA](/sic30-guies/fitxer-aca/)
<br/>

### Generació de pipelines

Aquest servei s'encarregarà de generar automàticament totes les pipelines necessàries, tant per al **desplegament del component o aplicació com
altres pipelines per a dur a terme les operacions necessàries sobre plataformes cloud**. Aquestes pipelines operatives es generaran dins d'un directori diferenciat
`/Advanced` dins del directori de tasques Jenkins associat al projecte i seran les següents:

- **DEPLOY-START**: permet iniciar el servei.

- **DEPLOY-STOP**: permet aturar el servei.

- **DEPLOY-RESTART**: permet aturar i tornar a iniciar el servei.

- **DEPLOY-DESCRIPTORS**: permet desplegar canvis en els descriptors (noves variables d'entorn, canvis en la configuració i altres)
sense fer la construcció i desplegament de la imatge.

- **DEPLOY-ALL**: permet fer un desplegament complet davant canvis en l'aplicació, orquestradors i/o descriptors.

- **DEPLOY-TAG**: permet redesplegar un determinat tag de la imatge de l'aplicació que s'hagi desplegat amb èxit a producció (v.x.y.z-PR)
concebuda per a poder fer un *rollback* a una versió anterior.

Per altra banda, cal comentar que es generaran les pipelines internes dedicades a certes tasques comunes i que seran executades
per la pipeline principal. Aquestes pipelines no seran visibles per l'usuari i són les següents:

- **DEPLOYER**: s'encarrega del desplegament de l'aplicació als diferents entorns de rebuda. Serà invocada per la pipeline principal per al
desplegament de cada component a cada un dels entorns.

- **CLEANER**: s'encarrega de l'esborrat d'espais de treball. Serà invocada per la pipeline principal en finalitzar.

### Tecnologies compatibles

Veure: [Matriu de tecnologies](/sic30-serveis/ci/#matriu-de-tecnologies-de-construcció)

<div class="message information">
El SIC actualment utilitza la <a href="https://www.docker.com/">tecnologia Docker</a> per a disposar d'un entorn aïllat i immutable de construcció que, a més pugui ser utilitzat i testejat pels propis proveïdors.
Addicionalment, es contempla l'ús d'entorns propis de construcció proporcionats pels proveïdors (DockerFile) que opcionalment podran estendre del catàleg d'imatges corporatiu.<br/>
Veure: <a href="https://canigo.ctti.gencat.cat/sic30-serveis/cataleg-imatges/">Catàleg d'imatges corporatiu</a>.
</div>

<br/><br/><br/>
Si voleu més informació podeu consultar la secció de [**Guies**](/sic30-guies/). <br/>
Si teniu qualsevol dubte o problema podeu revisar les [**Preguntes Freqüents**] (/sic/faq) o utilitzar els canals de [**Suport**] (/sic/suport).