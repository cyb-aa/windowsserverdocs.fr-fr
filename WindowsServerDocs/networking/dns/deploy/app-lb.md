---
title: Utiliser une stratégie DNS pour l’équilibrage de charge des applications
description: Cette rubrique fait partie du Guide de scénario de stratégie DNS pour Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9c313ac-bb86-4e48-b9b9-de5004393e06
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 356c61c2cc5b60f43a69f17966c97f3c69d05cda
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356040"
---
# <a name="use-dns-policy-for-application-load-balancing"></a>Utiliser une stratégie DNS pour l’équilibrage de charge des applications

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour apprendre à configurer une stratégie DNS pour effectuer l’équilibrage de charge de l’application.

Les versions précédentes du DNS Windows Server offraient uniquement l’équilibrage de charge à l’aide de réponses de tourniquet (Round Robin). Toutefois, avec DNS dans Windows Server 2016, vous pouvez configurer une stratégie DNS pour l’équilibrage de charge d’application.

Lorsque vous avez déployé plusieurs instances d’une application, vous pouvez utiliser la stratégie DNS pour équilibrer la charge du trafic entre les différentes instances d’application, ce qui permet d’allouer dynamiquement la charge du trafic pour l’application.

## <a name="example-of-application-load-balancing"></a>Exemple d’équilibrage de charge d’application

Vous trouverez ci-dessous un exemple de la façon dont vous pouvez utiliser la stratégie DNS pour l’équilibrage de charge d’application.

Cet exemple utilise un service de cadeaux société-contoso fictif, qui fournit des services gifing en ligne et un site Web nommé **contosogiftservices.com**.

Le site Web contosogiftservices.com est hébergé dans plusieurs centres de centres ayant chacun des adresses IP différentes.

Dans Amérique du Nord, qui est le marché principal des services de cadeaux contoso, le site Web est hébergé dans trois centres de connaissances : Chicago, IL, Dallas, Texas et Seattle, WA.

Le serveur Web de Seattle dispose de la meilleure configuration matérielle et peut gérer deux fois plus de charge que les deux autres sites. Les services de cadeaux contoso veulent que le trafic d’application soit dirigé de la manière suivante.

- Étant donné que le serveur Web de Seattle comprend davantage de ressources, la moitié des clients de l’application sont dirigées vers ce serveur.
- Un quart des clients de l’application sont dirigés vers le centre de donnée Dallas, Texas.
- Un quart des clients de l’application sont dirigés vers Chicago, IL, centre de donnée

L’illustration suivante représente ce scénario.

![Équilibrage de charge d’application DNS avec stratégie DNS](../../media/Dns-App-Lb/dns-app-lb.jpg)


### <a name="how-application-load-balancing-works"></a>Fonctionnement de l’équilibrage de charge d’application

Une fois que vous avez configuré le serveur DNS avec la stratégie DNS pour l’équilibrage de la charge des applications à l’aide de cet exemple de scénario, le serveur DNS répond à 50% du temps avec l’adresse du serveur Web de Seattle, 25% du temps avec l’adresse du serveur Web Dallas et 25% du temps avec adresse du serveur Web de Chicago.

Ainsi, pour les quatre requêtes que le serveur DNS reçoit, il répond avec deux réponses pour Seattle et une pour Dallas et Chicago.

Un problème possible avec l’équilibrage de charge avec la stratégie DNS est la mise en cache des enregistrements DNS par le client DNS et le programme de résolution/LDNS, qui peuvent interférer avec l’équilibrage de charge, car le client ou le programme de résolution n’envoient pas de requête au serveur DNS.

Vous pouvez atténuer l’effet de ce comportement en utilisant une valeur @ no__t-0to @ no__t-1Live \(TTL @ no__t-3 à faible temps pour les enregistrements DNS dont la charge doit être équilibrée.

### <a name="how-to-configure-application-load-balancing"></a>Comment configurer l’équilibrage de charge d’application

Les sections suivantes vous montrent comment configurer la stratégie DNS pour l’équilibrage de charge de l’application.

#### <a name="create-the-zone-scopes"></a>Créer les étendues de zone

Vous devez d’abord créer les étendues de la zone contosogiftservices.com pour les centres de centres où ils sont hébergés.

Une étendue de zone est une instance unique de la zone. Une zone DNS peut avoir plusieurs étendues de zone, chaque étendue contenant son propre ensemble d’enregistrements DNS. Le même enregistrement peut être présent dans plusieurs étendues, avec des adresses IP différentes ou les mêmes adresses IP.

>[!NOTE]
>Par défaut, il existe une étendue de zone sur les zones DNS. Cette étendue de zone porte le même nom que la zone, et les opérations DNS héritées fonctionnent sur cette étendue.

Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer des étendues de zone.
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DallasZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "ChicagoZoneScope"

Pour plus d’informations, consultez [Add-DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

#### <a name="bkmk_records"></a>Ajouter des enregistrements aux étendues de zone

À présent, vous devez ajouter les enregistrements qui représentent l’hôte du serveur Web dans les étendues de la zone.

Dans **SeattleZoneScope**, vous pouvez ajouter l’enregistrement www.contosogiftservices.com avec l’adresse IP 192.0.0.1, qui se trouve dans le centre de contenu Seattle.

Dans **ChicagoZoneScope**, vous pouvez ajouter le même enregistrement @no__t -1www. contosogiftservices. com @ no__t-2 avec l’adresse IP 182.0.0.1 dans le centre de centres Chicago.

De même, dans **DallasZoneScope**, vous pouvez ajouter un enregistrement @no__t -1www. contosogiftservices. com @ no__t-2 avec l’adresse IP 162.0.0.1 dans le centre de centres Chicago.

Vous pouvez utiliser les commandes Windows PowerShell suivantes pour ajouter des enregistrements aux étendues de zone.
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "182.0.0.1" -ZoneScope "ChicagoZoneScope"
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "162.0.0.1" -ZoneScope "DallasZoneScope"
    

Pour plus d’informations, consultez [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

#### <a name="bkmk_policies"></a>Créer les stratégies DNS

Une fois que vous avez créé les partitions (étendues de zone) et que vous avez ajouté des enregistrements, vous devez créer des stratégies DNS qui distribuent les requêtes entrantes sur ces étendues afin que 50% des requêtes pour contosogiftservices.com soient réparties avec l’adresse IP du Web. le serveur du centre de la Seattle et le reste sont répartis uniformément entre les centres de distribution de Chicago et de Dallas.

Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer une stratégie DNS qui équilibre le trafic d’application sur ces trois centres de informations.

>[!NOTE]
>Dans l’exemple de commande ci-dessous, l’expression – ZoneScope "SeattleZoneScope, 2 ; ChicagoZoneScope, 1 ; DallasZoneScope, 1 "configure le serveur DNS avec un tableau qui comprend la combinaison de paramètres \<ZoneScope @ no__t-1, \<weight @ no__t-3.
    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1;DallasZoneScope,1" -ZoneName "contosogiftservices.com"
    

Pour plus d’informations, consultez [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  

Vous venez de créer avec succès une stratégie DNS qui permet d’équilibrer la charge des applications entre les serveurs Web dans trois centres de donnes différents.

Vous pouvez créer des milliers de stratégies DNS en fonction de vos besoins en matière de gestion du trafic, et toutes les nouvelles stratégies sont appliquées de manière dynamique, sans redémarrer le serveur DNS-sur les requêtes entrantes.
