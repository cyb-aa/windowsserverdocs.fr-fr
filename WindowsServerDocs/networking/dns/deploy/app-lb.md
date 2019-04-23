---
title: Utiliser une stratégie DNS pour l’équilibrage de charge des applications
description: Cette rubrique fait partie de DNS stratégie scénario Guide pour Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9c313ac-bb86-4e48-b9b9-de5004393e06
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1bb3e6695a7ec8fc7d950873403df023b4def3d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881610"
---
# <a name="use-dns-policy-for-application-load-balancing"></a>Utiliser une stratégie DNS pour l’équilibrage de charge des applications

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour savoir comment configurer une stratégie DNS pour effectuer l’équilibrage de charge d’application.

Les versions précédentes de Windows Server DNS fournissaient uniquement à l’aide des réponses de tourniquet (Round Robin) ; d’équilibrage de charge mais avec DNS dans Windows Server 2016, vous pouvez configurer une stratégie DNS pour l’équilibrage de charge d’application.

Lorsque vous avez déployé plusieurs instances d’une application, vous pouvez utiliser une stratégie DNS pour équilibrer la charge du trafic entre les différentes instances d’application, attribution ainsi dynamique de la charge du trafic de l’application.

## <a name="example-of-application-load-balancing"></a>Exemple d’Application d’équilibrage de charge

Voici un exemple de comment vous pouvez utiliser une stratégie DNS pour l’équilibrage de charge d’application.

Cet exemple utilise la société fictive une - Services cadeau de Contoso - qui fournit des services en ligne gifing, et qui a un site Web nommé **contosogiftservices.com**.

Le site Web contosogiftservices.com est hébergé dans plusieurs centres de données qui ont chacune des adresses IP différentes.

Amérique du Nord, qui est le principal marché pour les Services de cadeau de Contoso, le site Web est hébergé dans trois centres de données : Chicago, IL, Dallas, Texas et Seattle, WA.

Le serveur Web de Seattle a la meilleure configuration matérielle et peut gérer deux fois plus de charge en tant que les deux sites. Services cadeau de Contoso souhaite que le trafic d’application indiqué dans la manière suivante.

- Étant donné que le serveur Web de Seattle inclut plus de ressources, la moitié des clients de l’application sont dirigée vers ce serveur
- Un quart de clients de l’application sont dirigées vers le centre de données de Dallas, Texas
- Un quart de clients de l’application sont dirigées vers le centre de données de Chicago, IL

L’illustration suivante représente ce scénario.

![DNS Application équilibrage de charge avec une stratégie DNS](../../media/Dns-App-Lb/dns-app-lb.jpg)


### <a name="how-application-load-balancing-works"></a>Comment Application charge équilibrage Works

Après avoir configuré le serveur DNS avec une stratégie DNS pour l’application charge équilibrage à l’aide de cet exemple de scénario, le serveur DNS répond à 50 % du temps avec l’adresse du serveur Web de Seattle, 25 % du temps avec l’adresse du serveur Web de Dallas et 25 % du temps avec l’adresse du serveur Web de Chicago.

Par conséquent, pour chaque quatre requêtes que reçoit le serveur DNS, il répond avec deux réponses pour Seattle et un pour chacun des Dallas et Chicago.

Un problème potentiel avec équilibrage de charge avec une stratégie DNS est la mise en cache des enregistrements DNS par le client DNS et le programme de résolution/LDNS, qui peuvent interférer avec, car le client ou le programme de résolution ne pas envoyer une requête au serveur DNS d’équilibrage de charge.

Vous pouvez atténuer l’effet de ce comportement à l’aide d’un temps\-à\-Live \(TTL\) valeur pour les enregistrements DNS qui doit être équilibrée.

### <a name="how-to-configure-application-load-balancing"></a>Comment configurer l’équilibrage de charge de l’Application

Les sections suivantes vous montrent comment configurer une stratégie DNS pour l’équilibrage de charge d’application.

#### <a name="create-the-zone-scopes"></a>Créer des étendues de la Zone

Vous devez d’abord créer les étendues de la zone contosogiftservices.com pour les centres de données où elles sont hébergées.

Une étendue de la zone est une instance unique de la zone. Une zone DNS peut avoir plusieurs étendues de zone, avec chaque étendue de la zone contenant son propre ensemble d’enregistrements DNS. Le même enregistrement peut être présent dans plusieurs étendues, avec différentes adresses IP ou les mêmes adresses IP.

>[!NOTE]
>Par défaut, une étendue de la zone existe sur les zones DNS. Cette étendue de la zone a le même nom que la zone, et des opérations DNS héritées travailler sur cette étendue.

Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer des étendues de zone.
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DallasZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "ChicagoZoneScope"

Pour plus d’informations, consultez [DnsServerZoneScope-ajouter](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

####<a name="bkmk_records"></a>Ajoutez des enregistrements dans les étendues de Zone

Maintenant, vous devez ajouter les enregistrements représentant l’hôte du serveur web dans les étendues de zone.

Dans **SeattleZoneScope**, vous pouvez ajouter les enregistrements www.contosogiftservices.com avec l’adresse IP 192.0.0.1, qui se trouve dans le centre de données de Seattle.

Dans **ChicagoZoneScope**, vous pouvez ajouter le même enregistrement \(www.contosogiftservices.com\) avec adresse IP 182.0.0.1 dans le centre de données de Chicago.

De même dans **DallasZoneScope**, vous pouvez ajouter un enregistrement \(www.contosogiftservices.com\) avec adresse IP 162.0.0.1 dans le centre de données de Chicago.

Vous pouvez utiliser les commandes Windows PowerShell suivantes pour ajouter des enregistrements pour les étendues de zone.
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "182.0.0.1" -ZoneScope "ChicagoZoneScope"
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "162.0.0.1" -ZoneScope "DallasZoneScope"
    

Pour plus d’informations, consultez [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

####<a name="bkmk_policies"></a>Créer les stratégies DNS

Une fois que vous avez créé les partitions (étendues de zone) et que vous avez ajouté des enregistrements, vous devez créer des stratégies DNS qui distribuent les requêtes entrantes entre ces étendues afin que 50 % des requêtes pour contosogiftservices.com sont a répondu à avec l’adresse IP pour le Web serveur dans le centre de données de Seattle et le reste sont distribuées équitablement entre les centres de données de Chicago et Dallas.

Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer une stratégie DNS qui équilibre le trafic des applications sur ces trois centres de données.

>[!NOTE]
>Dans l’exemple de commande ci-dessous, l’expression – ZonePortée « SeattleZoneScope, 2 ; ChicagoZoneScope, 1 ; DallasZoneScope, 1 » configure le serveur DNS avec un tableau qui inclut la combinaison de paramètres \<ZonePortée\>,\<poids\>.
    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1;DallasZoneScope,1" -ZoneName "contosogiftservices.com"
    

Pour plus d’informations, consultez [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  

Vous avez maintenant créé une stratégie DNS qui fournit l’application équilibrage de charge sur les serveurs Web dans trois centres de données différents.

Vous pouvez créer des milliers de stratégies DNS, en fonction de votre trafic en matière de gestion, et toutes les nouvelles stratégies sont appliquées dynamiquement, sans avoir à redémarrer le serveur DNS : sur les requêtes entrantes.
