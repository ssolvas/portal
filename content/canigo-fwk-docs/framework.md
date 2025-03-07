+++
date        = "2020-12-17"
title       = "Framework Canigó"
description = "Descripció del Framework de desenvolupament corporatiu JEE de la Generalitat de Catalunya"
sections    = "canigo-fwk-docs"
taxonomies  = []
toc       = true
weight        = 1
+++


## Descripció del Framework

Canigó es defineix com un **espai de treball tecnològic comú per al desenvolupament i execució d'aplicacions en l'àmbit dels sistemes
corporatius i departamentals de la Generalitat de Catalunya** i els seus objectius són:

- Oferir una arquitectura comuna de construcció d'aplicacions JEE.
- Proporcionar un entorn de treball, documentació, suport i manteniment dels seus components.
- Simplificar la complexitat inherent a JEE, oferint un marc de referència de treball.
- Oferir una solució alineada amb els estàndards i solucions més utilitzades per la comunitat OpenSource.
- Oferir una solució oberta que permeti afegir i intercanviar qualsevol peça amb un cost reduït.
- Oferir una solució d'interconnectivitat amb els serveis corporatius.
- Oferir patrons de desenvolupament àmpliament acceptats.

L'arquitectura de Canigó es basa en l'**arquitectura MVC (Model-Vista-Controlador)**, on existeix un procés d'abstracció que permet
dividir una aplicació en components lògics amb responsabilitats diferenciades i que poden ser desenvolupats fins i tot per diferents rols dins l'equip.
Tanmateix, a més de l'estructura lògica dels components segons el patró MVC, Canigó defineix la seva ubicació segons una
arquitectura en 3 capes i 4 mòduls transversals.

<br/>
Capes:

- **Capa de Presentació**: els patrons de desenvolupament actuals fomenten la separació de la capa de presentació en dues parts diferenciades:
   - **Servidor**: la "vista" serà una resposta format JSON, mitjançant serveis (microserveis) HTTP/REST
   - **Client**: client lleuger estàtic que consumirà el serveis del punt anterior.

- **Capa de Lògica de Negoci**
- **Capa de Dades/Integració**

<br/>
Mòduls transversals:

- **Nucli del Framework**
- **Mòdul de Seguretat**
- **Mòdul de Suport**
- **Mòdul d'Operació**


![Arquitectura Canigó](/related/canigo/arquitectura.png)

Tal com es mostra en el gràfic, Canigó està estructurat modularment donant així l'opció de només utilitzar els mòduls
que es requereixis. Tots aquests mòduls es troben definits mitjançant interfícies, aïllant-los així de la implementació concreta escollida.
Canigó es basa en la utilització de interfícies i en la integració existent entre diferents APIs (JPA,...) i paquets oberts (Spring, Hibernate, ...)
oferint també extensions als paquets oberts, afegint un ampli ventall de components reutilitzables.

## Components Base

Canigó3 és un framework de codi obert per a la plataforma Java. Està basat en Spring, la primera versió del qual va ser creada per Rod Johnson
a l'octubre de 2002, i està pensat per a donar suport a:

- la configuració d'aplicacions
- facilitar el muntatge d'un sistema per parts fàcilment intercanviables i visibles entre elles
- la integració de serveis
- serveis de seguretat
- connexió a base de dades, transaccionalitat i altres
- la realització de proves, a través de Spring, que permet realitzar tests unitaris desacoblant els objectes del seu
context fent més senzill realitzar proves dels components per separat.
- l'accés a dades externes, mitjançant Spring, gestionant els recursos, proporcionant APIs d'ajuda i suportant la majoria
de les tecnologies d'accés a dades com: JDBC, JPA (Hibernate com a proveïdor) i MongoDB.

Es pot considerar que la base principal de l'arquitectura de Canigó és un conjunt totalment integrat, i a la vegada modular,
de les millors pràctiques tecnològiques existents actualment en aquest entorn, formada pels següents components:

- **Spring Framework** com a contenidor centralitzat d'objectes i serveis, totalment configurable mitjançant fitxers XML.
La injecció de dependències permet la configuració d'objectes fora del codi de l'aplicació (i de manera no intrusiva),
redueix el codi de l'aplicació dedicat a configurar i localitzar recursos, facilita millors pràctiques com programar contra
interfícies enlloc de contra classes, permetent el desacoblament de serveis i el canvi ràpid d'una implementació concreta per
una altra. Permet també la gestió de transaccions sense la utilització de APIs específiques mitjançant l'ús de Aspect Oriented Programming (AOP).
- **Spring Data JPA** proporciona un model de persistència basat en POJO's (Plain Old Java Objects) per a mapejar bases
de dades relacionals en Java.
- **Spring Data MongoDB** proporciona un model de persistència també basat en POJO's per a la integració amb la base de
dades orientada a documents MongoDB. La capa d'accés a dades es composa de repositoris.
- **AOP (Aspect Oriented Programming)** per a la intercepció d’esdeveniments a l'aplicació sense necessitat de modificar el codi.

## Mòduls

### Nucli del Framework

Aquest mòdul és el <i>core</i> del framework on es troben les principals característiques:

- **Configuració Multientorn**: basat en el PropertyPlaceHolder ofert per Spring. Amb la diferència que aquest mòdul
permet disposar de propietats dependents de l'entorn sense necessitat de cap configuració extra a nivell de beans o propietats.
- **Internacionalització (i18n)**: té com a objectiu facilitar el desenvolupament a l'hora d'oferir una aplicació en
múltiples llenguatges.
- **Servei de Traces**: basat en Log4j, permet definir el nivell de traces, sortides, nivell mínim de traces, format
de sortida, informació de context... etcètera.
- **Servei d'Excepcions**: permet informar que s'ha produït un error al realitzar una petició. Canigó ofereix una sèrie
d'excepcions per defecte (BaseException, CoreException, ...). També proporciona un mecanisme d'intercepció d'excepcions
per a evitar "try-catch".

### Mòdul de Seguretat

Té com a propòsit gestionar l'autenticació i autorització dels usuaris que accedeixen a les aplicacions:

- **Mòdul de Seguretat**: basat en Spring Security, permet gestionar l'autenticació i l'autorització dels usuaris de les aplicacions.
- **Backends Seguretat**: el servei de seguretat s'integra amb GICAR, SACE, LDAP, Base de dades i Inmemory.

### Mòdul de Suport

Aquest mòdul facilita l'ús d'un conjunt addicional de funcionalitats:

- **Transferència Fitxers**: permet al servidor obtenir fitxers adjunts provinents d'una petició d'un formulari HTML.
A més, aquest mòdul s'integra amb d'altres com el d'Antivirus.
- **Enviament de correu**: permet l'enviament de correus electrònics a una o diverses adreces. També permet enviar en
format de text pla o html i, en els dos casos, ofereix la possibilitat d'adjuntar un o més fitxers en mode adjunt o inline.
- **Planificador Tasques**: basat en Spring i Quartz. Ofereix la possibilitat d'executar tasques de forma diferida. En
qualsevol moment del dia, algun dia de la setmana... etcètera.
- **OLE**: permet la manipulació d'objectes OLE (Excel i Word: crear, llegir, modificar documents de Microsoft en format OLE).
Està basat en POI.
- **Merging**: permet realitzar la fusió de documents en format WordML, partint d'un document amb uns marcadors que seran
substituïts per un conjunt de valors indicats en un diccionari.
- **SFTP**: permet enviar i rebre arxius entre el servidor on s'executa l'aplicació i altres servidors de forma segura
mitjançant un intercanvi de claus. Està basat en llibreries JSCH i Commons-VFS.

### Mòdul d'Operació

- **Instrumentació**: permet a l'aplicació generar dades d'instrumentació (nombre de peticions, nombre d'errors, ...) de la seva
execució i posteriorment ser explotades amb eines de monitorització.
- **Monitorització**: permet a l'aplicació mostrar una pantalla on es mostren dades guardades pel mòdul d'instrumentació.
- **Logging**: permet a l'aplicació mostrar una pantalla on canviar el nivell dels logs, monitoritzar el diferents fitxers
de logs i descarregar-los.

### Mòdul d'Integració

La finalitat d'aquests mòduls és la de facilitar l'accés a diferents serveis que ofereix la Generalitat:

- **PICA**: proporciona una interfície Java per a accedir a la Plataforma d'Integració i Col·laboració Administrativa. Aquest
mòdul permet realitzar comunicacions síncrones i asíncrones.
- **PSIS**: permet la validació de signatures i certificats mitjançant el servei de PSIS ofert per Catcert.
- **SARCAT**: permet consumir els diferents serveis que ofereix Sarcat, a través de la Pica o via Webservices o SFTP per a peticions planificades.
- **Notificacions Telemàtiques**: és un connector funcional cap a la PICA que simplifica d’ús de Notificacions Telemàtiques de la Generalitat.
- **Antivirus**: permet l'escaneig d'arxius mitjançant la Plataforma d'Antivirus Corporatiu de la Generalitat.
- **Webservices**: Canigó 3 no disposa d'un mòdul de Webservices. S'ha preparat una guia on s'explica com realitzar l'exportació
de serveis Java mitjançant Webservices, la importació de webservices externs i generació de les classes Java d'invocació. Proposa l'ús d'Spring WS, Jaxb i OXM.
- **Cues**: Canigó 3 no disposa d'un mòdul de Gestió de Cues. S'ha preparat una guia on s'explica com produir i consumir
missatges d'una cua. Proposa l’ús d'Spring JMS (Java Message Service).

### Mòdul de Persistència

Aquest mòdul permet persistir i recuperar dades entre l'aplicació i el motor de base de dades:

- **JPA**: Java Persistence API busca la manera d'unificar les utilitats que proporcionen un mapeig objecte-relacional. La
implementació de JPA per defecte a Canigó 3 és Hibernate. Spring Data JPA permet implementar fàcilment repositoris per a l'accés a dades amb JPA.
- **MongoDB**: facilitat d'integració amb MongoDB gràcies a Spring Data MongoDB.

## Serveis

Per a la documentació de serveis REST, Canigó proporciona la utilitat [Swagger](https://swagger.io/), disponible al context "/" de l'aplicació.
Swagger proporciona una manera de descriure una API de forma estructurada i en un format llegible, a més proporciona una interfície disponible
des del navegador que permet interactuar i provar l'API directament des del navegador. L'especificació inclou la següent informació:

- Serveis que estan disponibles
- Operacions que estan disponibles als serveis
- Paràmetres disponibles als serveis
- Autoritzacions
- Informació addicional, com contractes o llicenciament

![Swagger](/related/canigo/documentacio/plugin-canigo/img11.jpg)

## Creació de projecte base

Des de CS Canigó es proporciona un [Pugin per l'Eclipse](/canigo-fwk-docs/entorn-de-desenvolupament/plugin-eclipse/) que utilitza un arquetipus _Maven_ per a
generar projectes base amb Canigó. Aquest plugin està disponible a l'[Entorn de desenvolupament](/canigo-fwk-docs/entorn-de-desenvolupament/maquina-virtual/).
El projecte base generat és un projecte _Maven_ amb un exemple de serveis rest disponibles a `/api/equipaments` on s'utilitzen les capes
Security, Controller, Service i Repository de JPA utilitzant Hibernate i atacant a una base de dades H2 en memòria.
Una vegada generat el projecte, es poden afegir i/o treure mòduls i modificar connectors com el de JPA per, per exemple, canviar el
connector de H2 a Oracle, Mysql o PostgreSQL.

![Capes projecte base](/related/canigo/projecte_base.png)

## Desplegament

El projecte base utilitza _Maven_ per a l'empaquetat del projecte per a ser desplegat al servidor d'aplicacions en format *war*.
No obstant, **si el projecte ha de ser desplegat a alguna plataforma Cloud, el projecte base utilitza** [**Spring Boot**](https://spring.io/projects/spring-boot)
per a ser auto-executable amb un servidor embegut que, per defecte, es troba configurat amb el servidor Tomcat, tot i que es pot canviar.
D’aquesta forma, el projecte auto-executable por allotjar-se dins d'una imatge [Docker](https://www.docker.com/) i desplegar-se a algun servidor
de contenidor d'imatges Docker. El projecte base ja proporciona la configuració d'exemple en un `Dockerfile` per a generar una imatge Docker amb el
projecte auto-executable.

![Desplegament projecte base](/related/canigo/desplegament_projecte_base.png)


<br/><br/><br/>
La informació detallada sobre els diferents components del Framework Canigó, la seva arquitectura i components, es pot trobar a l'apartat de <a href="/canigo-fwk-docs/documentacio-per-versions/">documentació del portal web</a>.

## Centre de Suport Canigó pel Framework Canigó

El **Framework Canigó** es ofert pel **Centre de Suport Canigó** que s'orienta a proporcionar serveis d'acompanyament als Departaments oferint el suport i assessorament tecnològic necessaris per tal d'assolir l'èxit de les seves noves iniciatives tecnològiques així com donar suport al correcte funcionament de les aplicacions i serveis actualment desplegats.

Els objectius principals del Centre de Suport pel servei de **Framework Canigó** es poden resumir en els següents aspectes:

* Assegurar la seva evolució tant a nivell correctiu com evolutiu.
* Garantir la màxima qualitat dels nostres productes, tant pel que fa al codi com al material de suport.
* Planificar, controlar i produir de forma continuada noves versions o evolucions, seguint les necessitats de la Generalitat.
