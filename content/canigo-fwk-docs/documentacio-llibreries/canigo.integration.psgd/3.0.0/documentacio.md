+++
date        = "2021-11-01"
title       = "Documentació"
description = "Documentació canigo.integration.psgd 3.0.0"
sections    = "canigo-fwk-docs"
weight      = 3
+++

## Propòsit

L’objectiu d’aquest connector és proporcionar un punt d’accés cap a la Plataforma de Serveis de Gestió Documental (PSGD) també coneguda com a ARESTA.

## Funcionalitats

### Beans

Conté el package *cat.gencat.ctti.canigo.arch.integration.psgd.beans* on s'ofereixen les entitats per representar la informació on s’allotgarà la resposta dels serveis de PSGD

### Service

S'ofereix el servei *cat.gencat.ctti.canigo.arch.integration.psgd.PsgdService* per a la gestió de la sessió cap a PSGD.

S'ofereix el servei *cat.gencat.ctti.canigo.arch.integration.psgd.PsgdConnector* per a gestió de les operacions cap a PSGD.

### Exception

S'ofereix la exception *cat.gencat.ctti.canigo.arch.integration.psgd.exceptions.PsgdException* per identificar els erros produits en el mòdul.
