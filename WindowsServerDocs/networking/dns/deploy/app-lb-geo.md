---
title: Utiliser une stratégie DNS pour l’équilibrage de charge des applications avec connaissance de la géolocalisation
description: Cette rubrique fait partie du Guide de scénario de stratégie DNS pour Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: b6e679c6-4398-496c-88bc-115099f3a819
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ea3f959612de0f2bc56a887ba73aba47f1d3f141
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406221"
---
# <a name="use-dns-policy-for-application-load-balancing-with-geo-location-awareness"></a>Utiliser une stratégie DNS pour l’équilibrage de charge des applications avec connaissance de la géolocalisation

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour apprendre à configurer la stratégie DNS afin d’équilibrer la charge d’une application avec une prise en charge de la géolocalisation.

La rubrique précédente de ce guide, [utiliser une stratégie DNS pour l’équilibrage de charge d’application](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb), utilise un exemple de services de cadeaux d’entreprise fictifs, qui fournit des services de cadeaux en ligne, et qui dispose d’un site Web nommé contosogiftservices.com. Contoso Gift services équilibre la charge de l’application Web en ligne entre les serveurs des centres de l’Amérique du Nord situés à Seattle, WA, Chicago, IL et Dallas, Texas.

>[!NOTE]
>Il est recommandé de vous familiariser avec la rubrique [utiliser la stratégie DNS pour l’équilibrage de charge d’application](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb) avant d’exécuter les instructions de ce scénario.

Cette rubrique utilise la même entreprise fictive et l’infrastructure réseau comme base pour un nouvel exemple de déploiement qui comprend la prise en compte de la géolocalisation.

Dans cet exemple, contoso Gift services est en pleine expansion dans le monde entier.

À l’instar de Amérique du Nord, la société a maintenant des serveurs Web hébergés dans des centres de centres européens.

Contoso Gift services les administrateurs DNS veulent configurer l’équilibrage de charge d’application pour les centres de distribution européens de la même façon que l’implémentation de la stratégie DNS dans le États-Unis, avec le trafic d’application distribué entre les serveurs Web qui se trouvent dans Dublin, Irlande, Amsterdam, Pays-Bas et ailleurs.

Les administrateurs DNS veulent également que toutes les requêtes d’autres emplacements dans le monde soient distribuées de manière égale entre tous leurs centres de centres.

Dans les sections suivantes, vous pouvez apprendre à atteindre des objectifs similaires à ceux des administrateurs DNS contoso sur votre propre réseau.

## <a name="how-to-configure-application-load-balancing-with-geo-location-awareness"></a>Comment configurer l’équilibrage de charge d’application avec la sensibilisation à la géolocalisation

Les sections suivantes vous montrent comment configurer la stratégie DNS pour l’équilibrage de charge d’application avec la sensibilisation à la géolocalisation.

>[!IMPORTANT]
>Les sections suivantes incluent des exemples de commandes Windows PowerShell qui contiennent des exemples de valeurs pour de nombreux paramètres. Veillez à remplacer les valeurs d’exemple dans ces commandes par des valeurs appropriées pour votre déploiement avant d’exécuter ces commandes.

### <a name="bkmk_clientsubnets"></a>Créer les sous-réseaux du client DNS

Vous devez d’abord identifier les sous-réseaux ou l’espace d’adressage IP des régions Amérique du Nord et Europe.

Vous pouvez obtenir ces informations à partir de cartes géo-IP. Sur la base de ces distributions géo-IP, vous devez créer les sous-réseaux du client DNS.

Un sous-réseau client DNS est un regroupement logique de sous-réseaux IPv4 ou IPv6 à partir desquels les requêtes sont envoyées à un serveur DNS.

Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer des sous-réseaux clients DNS. 

    
    Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet 192.0.0.0/24,182.0.0.0/24
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet 141.1.0.0/24,151.1.0.0/24
    
Pour plus d’informations, consultez [Add-DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).

### <a name="bkmk_zscopes2"></a>Créer les étendues de zone

Une fois les sous-réseaux clients en place, vous devez partitionner les contosogiftservices.com de zone dans différentes étendues de zone, chacune pour un centre de centres.

Une étendue de zone est une instance unique de la zone. Une zone DNS peut avoir plusieurs étendues de zone, chaque étendue contenant son propre ensemble d’enregistrements DNS. Le même enregistrement peut être présent dans plusieurs étendues, avec des adresses IP différentes ou les mêmes adresses IP.

>[!NOTE]
>Par défaut, il existe une étendue de zone sur les zones DNS. Cette étendue de zone porte le même nom que la zone, et les opérations DNS héritées fonctionnent sur cette étendue.

Le scénario précédent sur l’équilibrage de charge d’application montre comment configurer trois étendues de zone pour les centres de informations dans Amérique du Nord.

Avec les commandes ci-dessous, vous pouvez créer deux étendues de zone, une pour les centres de informations Dublin et Amsterdam. 

Vous pouvez ajouter ces étendues de zone sans modifier les trois étendues de zone de Amérique du Nord existantes dans la même zone. En outre, après avoir créé ces étendues de zone, vous n’avez pas besoin de redémarrer votre serveur DNS.

Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer des étendues de zone.

    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AmsterdamZoneScope"
    

Pour plus d’informations, consultez [Add-DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

### <a name="bkmk_records2"></a>Ajouter des enregistrements aux étendues de zone

À présent, vous devez ajouter les enregistrements qui représentent l’hôte du serveur Web dans les étendues de la zone.

Les enregistrements pour les centres de données d’États-Unis d’Amérique ont été ajoutés dans le scénario précédent. Vous pouvez utiliser les commandes Windows PowerShell suivantes pour ajouter des enregistrements aux étendues de zone pour les centres de données européens.
 
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "151.1.0.1" -ZoneScope "DublinZoneScope”
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "AmsterdamZoneScope"
    

Pour plus d’informations, consultez [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

### <a name="bkmk_policies2"></a>Créer les stratégies DNS

Une fois que vous avez créé les partitions (étendues de zone) et que vous avez ajouté des enregistrements, vous devez créer des stratégies DNS qui distribuent les requêtes entrantes sur ces étendues.

Pour cet exemple, la distribution des requêtes entre les serveurs d’applications dans différents centres de distribution répond aux critères suivants.

1. Lorsque la requête DNS est reçue à partir d’une source dans un sous-réseau client nord-américain, 50% des réponses DNS pointent vers le centre de données de Seattle, 25% des réponses pointent vers le centre de données de Chicago, et les 25% de réponses restantes pointent vers le centre de données de Dallas.
2. Lorsque la requête DNS est reçue à partir d’une source dans un sous-réseau client européen, 50% des réponses DNS pointent vers le centre de code Dublin, et 50% des réponses DNS pointent vers le centre de code Amsterdam.
3. Lorsque la requête provient de n’importe où dans le monde, les réponses DNS sont distribuées entre les cinq centres de distribution.

Vous pouvez utiliser les commandes Windows PowerShell suivantes pour implémenter ces stratégies DNS.

    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaLBPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1; TexasZoneScope,1" -ZoneName "contosogiftservices.com" –ProcessingOrder 1
    
    Add-DnsServerQueryResolutionPolicy -Name "EuropeLBPolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 2
    
    Add-DnsServerQueryResolutionPolicy -Name "WorldWidePolicy" -Action ALLOW -FQDN "eq,*.contoso.com" -ZoneScope "SeattleZoneScope,1;ChicagoZoneScope,1; TexasZoneScope,1;DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3
    
    

Pour plus d’informations, consultez [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).

Vous venez de créer avec succès une stratégie DNS qui permet d’équilibrer la charge des applications entre les serveurs Web qui se trouvent dans cinq centres de donnes différents sur plusieurs continents.

Vous pouvez créer des milliers de stratégies DNS en fonction de vos besoins en matière de gestion du trafic, et toutes les nouvelles stratégies sont appliquées de manière dynamique, sans redémarrer le serveur DNS-sur les requêtes entrantes.
