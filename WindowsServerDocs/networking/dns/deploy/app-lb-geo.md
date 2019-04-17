---
title: Utiliser une stratégie DNS pour l’Application équilibrage de charge avec géolocalisation
description: Cette rubrique fait partie de la DNS stratégie scénario Guide pour Windows Server2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: b6e679c6-4398-496c-88bc-115099f3a819
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7637d927c7b22b83053e7f9100b07581c11bafc0
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-application-load-balancing-with-geo-location-awareness"></a>Utiliser une stratégie DNS pour l’Application équilibrage de charge avec géolocalisation

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour savoir comment configurer une stratégie DNS pour équilibrer la charge une application avec la géolocalisation.

Rubrique précédente dans ce guide, [utiliser une stratégie DNS pour l’équilibrage de charge Application](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb), utilise un exemple d’une société fictive - Contoso cadeau Services - qui fournit donner online services, et qui dispose d’un site Web nommé contosogiftservices.com. Équilibre la charge de Services de cadeau Contoso de leur application Web en ligne entre les serveurs dans des centres de données Amérique du Nord situé dans Seattle, Washington, Chicago, Illinois et Dallas, Texas, États-Unis.

>[!NOTE]
>Il est recommandé de vous familiariser avec la rubrique [utiliser une stratégie DNS pour l’équilibrage de charge Application](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb) avant d’exécuter les instructions de ce scénario.

Cette rubrique utilise l’infrastructure réseau et la même entreprise fictive comme base pour un exemple de déploiement nouveau qui inclut la géolocalisation.

Dans cet exemple, Contoso cadeau Services étend avec succès leur présence dans le monde entier.

Comme pour l’Amérique du Nord, la société a maintenant des serveurs web hébergés dans des centres de données européenne.

Les administrateurs de Contoso cadeau Services DNS souhaitez configurer application équilibrage de charge pour les centres de données européennes de manière similaire à l’implémentation de la stratégie DNS aux États-Unis, avec le trafic des applications distribué entre les serveurs Web qui se trouvent dans Dublin, Irlande, Amsterdam, pays-bas et un autre emplacement.

Les administrateurs DNS également toutes les requêtes à partir d’autres emplacements dans le monde répartie entre tous leurs centres de données.

Dans les sections suivantes, vous pouvez apprendre à atteindre des objectifs similaires à celles des administrateurs DNS Contoso sur votre réseau.

## <a name="how-to-configure-application-load-balancing-with-geo-location-awareness"></a>Comment configurer l’Application équilibrage de charge avec géolocalisation

Les sections suivantes vous montrent comment configurer une stratégie DNS pour l’application équilibrage de charge avec géolocalisation.

>[!IMPORTANT]
>Les sections suivantes incluent des exemples de commandes Windows PowerShell qui contiennent des exemples de valeurs pour un grand nombre de paramètres. Assurez-vous que vous remplacez les exemples de valeurs de ces commandes par les valeurs appropriées pour votre déploiement avant d’exécuter ces commandes.

###<a name="bkmk_clientsubnets"></a>Créer les sous-réseaux du Client DNS

Vous devez d’abord identifier les sous-réseaux ou l’espace d’adressage IP des régions Amérique du Nord et en Europe.

Vous pouvez obtenir ces informations à partir de cartes géo-IP. En fonction de ces distributions géo-IP, vous devez créer les sous-réseaux du Client DNS.

Un sous-réseau de Client DNS est un regroupement logique des sous-réseaux IPv4 ou IPv6 à partir de laquelle les requêtes sont envoyées à un serveur DNS.

Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer des sous-réseaux de Client DNS. 

    
    Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet 192.0.0.0/24,182.0.0.0/24
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet 141.1.0.0/24,151.1.0.0/24
    
Pour plus d’informations, voir [Add-DnsServerClientSubnet](https://technet.microsoft.com/library/mt126261.aspx).

###<a name="bkmk_zscopes2"></a>Créer les étendues de Zone

Une fois que les sous-réseaux du client sont en place, vous devez partitionner contosogiftservices.com de la zone en étendues zone différents, chacun d’eux pour un centre de données.

Une étendue de la zone est une instance unique de la zone. Une zone DNS peut avoir plusieurs étendues de zone, avec chaque étendue de la zone qui contient son propre ensemble d’enregistrements DNS. L’enregistrement même peut être présent dans plusieurs étendues, avec des adresses IP différentes ou les mêmes adresses IP.

>[!NOTE]
>Par défaut, une étendue de la zone existe sur les zones DNS. Cette étendue de la zone a le même nom que la zone, et les opérations de DNS héritées fonctionnent sur cette étendue.

Le scénario précédent sur l’équilibrage de charge application montre comment configurer les trois étendues de centres de données de zone en Amérique du Nord.

Avec les commandes ci-dessous, vous pouvez créer deux étendues zone plus, une pour les centres de données Dublin et Amsterdam. 

Vous pouvez ajouter ces étendues zone sans aucune modification pour les trois étendues de zone Amérique du Nord existants dans la même zone. En outre, une fois que vous créez ces étendues de zone, vous n’avez pas besoin de redémarrer votre serveur DNS.

Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer des étendues de zone.

    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AmsterdamZoneScope"
    

Pour plus d’informations, voir [Add-DnsServerZoneScope](https://technet.microsoft.com/library/mt126267.aspx)

###<a name="bkmk_records2"></a>Ajouter des enregistrements pour les étendues de Zone

Maintenant, vous devez ajouter les enregistrements représentant l’hôte du serveur web dans les étendues de zone.

Les enregistrements pour les centres de données Amérique ont été ajoutées dans le scénario précédent. Vous pouvez utiliser les commandes Windows PowerShell suivantes pour ajouter des enregistrements pour les étendues de centres de données européenne zone.
 
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "151.1.0.1" -ZoneScope "DublinZoneScope”
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "AmsterdamZoneScope"
    

Pour plus d’informations, voir [Add-DnsServerResourceRecord](https://technet.microsoft.com/library/jj649925.aspx).

###<a name="bkmk_policies2"></a>Créer les stratégies DNS

Une fois que vous avez créé les partitions (étendues zone) et que vous avez ajouté des enregistrements, vous devez créer des stratégies DNS qui distribuer les requêtes entrantes sur ces étendues.

Pour cet exemple, la distribution de requête sur les serveurs d’applications dans différents centres de données respecte les critères suivants.

1. Lorsque la requête DNS est reçue à partir d’une source dans un sous-réseau client Amérique du Nord, 50 % des réponses DNS pointent vers le centre de données de Seattle, 25 % de réponses pointez sur le centre de données de Chicago, et les 25 % restants de réponses pointer vers le centre de données Dallas.
2. Lorsque la requête DNS est reçue à partir d’une source dans un sous-réseau client européen, 50 % des réponses DNS pointent vers le centre de données Dublin et 50 % des réponses DNS pointent vers le centre de données Amsterdam.
3. Lorsque la requête provient d’ailleurs dans le monde entier, les réponses DNS sont réparties sur tous les cinq centres de données.

Vous pouvez utiliser les commandes Windows PowerShell suivantes pour implémenter ces stratégies DNS.

    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaLBPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1; TexasZoneScope,1" -ZoneName "contosogiftservices.com" –ProcessingOrder 1
    
    Add-DnsServerQueryResolutionPolicy -Name "EuropeLBPolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 2
    
    Add-DnsServerQueryResolutionPolicy -Name "WorldWidePolicy" -Action ALLOW -FQDN "eq,*.contoso.com" -ZoneScope "SeattleZoneScope,1;ChicagoZoneScope,1; TexasZoneScope,1;DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3
    
    

Pour plus d’informations, voir [Add-DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx).

Vous avez maintenant créé une stratégie DNS qui fournit l’application équilibrage de charge sur les serveurs Web qui sont trouvent dans les cinq différents centres de données sur plusieurs continents.

Vous pouvez créer des milliers de stratégies DNS en fonction de votre trafic en matière de gestion, et toutes les nouvelles stratégies sont appliquées dynamiquement, sans redémarrer le serveur DNS - dans les requêtes entrantes.
