---
title: Utiliser une stratégie DNS pour l’équilibrage de la charge de l’Application
description: Cette rubrique fait partie de la DNS stratégie scénario Guide pour Windows Server2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9c313ac-bb86-4e48-b9b9-de5004393e06
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d156c258b971c45bf1c4c20739440bd5cc9e239f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-application-load-balancing"></a>Utiliser une stratégie DNS pour l’équilibrage de la charge de l’Application

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour savoir comment configurer une stratégie DNS pour effectuer l’équilibrage de charge application.

Les versions précédentes de Windows Server DNS fournissaient uniquement à l’aide du tourniquet réponses; d’équilibrage de charge mais avec DNS dans Windows Server 2016, vous pouvez configurer une stratégie DNS pour l’équilibrage de charge application.

Lorsque vous avez déployé plusieurs instances d’une application, vous pouvez utiliser une stratégie DNS pour équilibrer la charge du trafic entre les instances d’application différents, attribution ainsi dynamique de la charge du trafic de l’application.

## <a name="example-of-application-load-balancing"></a>Exemple de l’équilibrage de la charge de l’Application

Voici un exemple de comment vous pouvez utiliser une stratégie DNS pour l’équilibrage de charge application.

Cet exemple utilise la société fictive une - Contoso cadeau Services - qui fournit des services en ligne gifing, et qui a un site Web nommé **contosogiftservices.com**.

Le site Web contosogiftservices.com est hébergé dans plusieurs centres de données qui ont des adresses IP différentes.

En Amérique du Nord, qui est le principal marché pour les Services de cadeaux Contoso, le site Web est hébergé dans trois centres de données: Chicago, Illinois, Dallas, Texas et Seattle, Washington.

Le serveur Web de Seattle possède la meilleure configuration matérielle et peut gérer deux fois plus de charge en tant que les deux sites. Services de cadeau Contoso souhaite trafic application indiqué dans la manière suivante.

- Étant donné que le serveur Web Seattle comprend plus de ressources, la moitié des clients de l’application sont dirigée vers ce serveur
- Un quart des clients de l’application sont dirigées vers le centre de données Dallas, Texas, États-Unis
- Un quart des clients de l’application sont dirigées vers le centre de données de Chicago, Illinois,

L’illustration suivante décrit ce scénario.

![DNS Application équilibrage de charge avec une stratégie DNS](../../media/Dns-App-Lb/dns-app-lb.jpg)


### <a name="how-application-load-balancing-works"></a>Comment Application équilibrage des travaux

Après avoir configuré le serveur DNS avec une stratégie DNS pour l’application charge équilibrage à l’aide de cet exemple de scénario, le serveur DNS répond à 50 % du temps avec l’adresse du serveur Web de Seattle, 25 % du temps avec l’adresse du serveur Web Dallas et 25 % du temps avec l’adresse du serveur Web de Chicago.

Par conséquent pour toutes les quatre requêtes que reçoit le serveur DNS, il répond avec deux réponses pour Seattle et un pour chaque Dallas et Chicago.

Un problème potentiel avec équilibrage de charge avec une stratégie DNS est la mise en cache des enregistrements DNS par le client DNS et la résolution/LDNS, ce qui peut interférer avec équilibrage car le client ou le programme de résolution de ne pas envoyer une requête au serveur DNS.

Vous pouvez réduire l’effet de ce comportement à l’aide d’une valeur \(TTL\) Live-terme celle-ci faible pour les enregistrements DNS qui doivent être à charge équilibrée.

### <a name="how-to-configure-application-load-balancing"></a>Comment configurer l’équilibrage de la charge de l’Application

Les sections suivantes vous montrent comment configurer une stratégie DNS pour l’équilibrage de charge application.

#### <a name="create-the-zone-scopes"></a>Créer les étendues de Zone

Vous devez d’abord créer l’étendue de la zone contosogiftservices.com pour les centres de données où elles sont hébergées.

Une étendue de la zone est une instance unique de la zone. Une zone DNS peut avoir plusieurs étendues de zone, avec chaque étendue de la zone qui contient son propre ensemble d’enregistrements DNS. L’enregistrement même peut être présent dans plusieurs étendues, avec des adresses IP différentes ou les mêmes adresses IP.

>[!NOTE]
>Par défaut, une étendue de la zone existe sur les zones DNS. Cette étendue de la zone a le même nom que la zone, et les opérations de DNS héritées fonctionnent sur cette étendue.

Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer des étendues de zone.
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DallasZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "ChicagoZoneScope"

Pour plus d’informations, voir [Add-DnsServerZoneScope](https://technet.microsoft.com/library/mt126267.aspx)

####<a name="bkmk_records"></a>Ajouter des enregistrements pour les étendues de Zone

Maintenant, vous devez ajouter les enregistrements représentant l’hôte du serveur web dans les étendues de zone.

Dans **SeattleZoneScope**, vous pouvez ajouter l’enregistrement www.contosogiftservices.com avec l’adresse IP 192.0.0.1, qui se trouve dans le centre de données de Seattle.

Dans **ChicagoZoneScope**, vous pouvez ajouter le même \(www.contosogiftservices.com\) Enregistrer avec l’adresse IP 182.0.0.1 dans le centre de données de Chicago.

De même dans **DallasZoneScope**, vous pouvez ajouter un \(www.contosogiftservices.com\) enregistrement avec l’adresse IP 162.0.0.1 dans le centre de données de Chicago.

Vous pouvez utiliser les commandes Windows PowerShell suivantes pour ajouter des enregistrements pour les étendues de zone.
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "182.0.0.1" -ZoneScope "ChicagoZoneScope"
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "162.0.0.1" -ZoneScope "DallasZoneScope"
    

Pour plus d’informations, voir [Add-DnsServerResourceRecord](https://technet.microsoft.com/library/jj649925.aspx).

####<a name="bkmk_policies"></a>Créer les stratégies DNS

Une fois que vous avez créé les partitions (étendues zone) et que vous avez ajouté des enregistrements, vous devez créer des stratégies DNS qui distribuer les requêtes entrantes sur ces étendues afin que 50 % de requêtes pour contosogiftservices.com sont répondu avec l’adresse IP du serveur Web du centre de données de Seattle et le reste est répartie entre les centres de données de Chicago et Dallas.

Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer une stratégie DNS qui équilibre le trafic des applications sur ces trois centres de données.

>[!NOTE]
>Dans l’exemple de commande ci-dessous, l’expression – ZonePortée «SeattleZoneScope, 2; ChicagoZoneScope, 1; Configure le serveur DNS 1» DallasZoneScope, avec un tableau qui contient la combinaison de paramètre \ < ZoneScope\, > \ < weight\ >.
    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW – -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1;DallasZoneScope,1" -ZoneName "contosogiftservices.com"
    

Pour plus d’informations, voir [Add-DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx).  

Vous avez maintenant créé une stratégie DNS qui fournit l’application équilibrage de charge sur les serveurs Web dans trois différents centres de données.

Vous pouvez créer des milliers de stratégies DNS en fonction de votre trafic en matière de gestion, et toutes les nouvelles stratégies sont appliquées dynamiquement, sans redémarrer le serveur DNS - dans les requêtes entrantes.