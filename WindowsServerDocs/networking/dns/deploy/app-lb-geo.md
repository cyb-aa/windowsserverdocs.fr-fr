---
title: Utiliser une stratégie DNS pour l’équilibrage de charge des applications avec connaissance de la géolocalisation
description: Cette rubrique fait partie de DNS stratégie scénario Guide pour Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: b6e679c6-4398-496c-88bc-115099f3a819
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9f76163e6b064ac3225ab4d755afd548e1cb720b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446411"
---
# <a name="use-dns-policy-for-application-load-balancing-with-geo-location-awareness"></a>Utiliser une stratégie DNS pour l’équilibrage de charge des applications avec connaissance de la géolocalisation

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour savoir comment configurer une stratégie DNS pour équilibrer la charge une application avec reconnaissance de l’emplacement géographique.

La rubrique précédente dans ce guide, [utiliser une stratégie DNS pour l’équilibrage de charge Application](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb), utilise un exemple d’une société fictive - Services cadeau de Contoso - qui fournit des dons en ligne des services et qui a un site Web nommé contosogiftservices.com. Équilibre la charge de Services cadeau de Contoso de leur application Web en ligne entre les serveurs dans des centres de données nord-américain et situés dans Seattle, WA, Chicago, IL et Dallas, Texas.

>[!NOTE]
>Il est recommandé de vous familiariser avec la rubrique [utiliser une stratégie DNS pour l’équilibrage de charge Application](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb) avant d’exécuter les instructions de ce scénario.

Cette rubrique utilise l’infrastructure réseau et la même entreprise fictive comme base pour un nouveau déploiement d’exemple qui inclut une reconnaissance de l’emplacement géographique.

Dans cet exemple, Contoso cadeau Services étend avec succès leur présence dans le monde entier.

Comme pour l’Amérique du Nord, la société bénéficie désormais de serveurs web hébergés dans des centres de données européen.

Les administrateurs de Contoso cadeau Services DNS souhaitez configurer l’application équilibrage de charge pour les centres de données européens d’une manière similaire à l’implémentation de la stratégie DNS aux États-Unis, avec le trafic des applications distribué entre les serveurs Web qui sont trouvent dans Dublin, Irlande, Amsterdam, pays-bas et ailleurs.

Les administrateurs DNS également toutes les requêtes à partir d’autres emplacements dans le monde distribuées équitablement entre tous leurs centres de données.

Dans les sections suivantes, vous pouvez apprendre à atteindre des objectifs similaires à celles des administrateurs de DNS de Contoso sur votre propre réseau.

## <a name="how-to-configure-application-load-balancing-with-geo-location-awareness"></a>Comment configurer l’Application équilibrage de charge avec géolocalisation

Les sections suivantes vous montrent comment configurer une stratégie DNS pour l’application équilibrage de charge avec géolocalisation.

>[!IMPORTANT]
>Les sections suivantes incluent des exemples de commandes Windows PowerShell qui contiennent des exemples de valeurs de paramètres. Veillez à remplacer les exemples de valeurs dans ces commandes avec les valeurs appropriées pour votre déploiement avant d’exécuter ces commandes.

### <a name="bkmk_clientsubnets"></a>Créer les sous-réseaux du Client DNS

Vous devez tout d’abord identifier les sous-réseaux ou un espace d’adressage IP des régions Amérique du Nord et Europe.

Vous pouvez obtenir ces informations à partir de mappages de géo-IP. En fonction de ces distributions géo-IP, vous devez créer les sous-réseaux du Client DNS.

Un sous-réseau de Client DNS est un regroupement logique des sous-réseaux IPv4 ou IPv6 à partir de laquelle les requêtes sont envoyées à un serveur DNS.

Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer des sous-réseaux de Client DNS. 

    
    Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet 192.0.0.0/24,182.0.0.0/24
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet 141.1.0.0/24,151.1.0.0/24
    
Pour plus d’informations, consultez [Add-DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).

### <a name="bkmk_zscopes2"></a>Créer des étendues de la Zone

Une fois que les sous-réseaux du client sont en place, vous devez partitionner la zone contosogiftservices.com dans des étendues de zone différents, chacun correspondant à un centre de données.

Une étendue de la zone est une instance unique de la zone. Une zone DNS peut avoir plusieurs étendues de zone, avec chaque étendue de la zone contenant son propre ensemble d’enregistrements DNS. Le même enregistrement peut être présent dans plusieurs étendues, avec différentes adresses IP ou les mêmes adresses IP.

>[!NOTE]
>Par défaut, une étendue de la zone existe sur les zones DNS. Cette étendue de la zone a le même nom que la zone, et des opérations DNS héritées travailler sur cette étendue.

Le scénario précédent sur l’équilibrage de charge d’application montre comment configurer les trois étendues de centres de données de zone en Amérique du Nord.

Avec les commandes ci-dessous, vous pouvez créer deux étendues de zone plus, un pour les centres de données Dublin et Amsterdam. 

Vous pouvez ajouter ces étendues de zone sans aucune modification pour les trois étendues de zone Amérique du Nord existants dans la même zone. En outre, après avoir créé ces étendues de zone, il est inutile de redémarrer votre serveur DNS.

Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer des étendues de zone.

    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AmsterdamZoneScope"
    

Pour plus d’informations, consultez [DnsServerZoneScope-ajouter](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

### <a name="bkmk_records2"></a>Ajoutez des enregistrements dans les étendues de Zone

Maintenant, vous devez ajouter les enregistrements représentant l’hôte du serveur web dans les étendues de zone.

Les enregistrements pour les centres de données d’Amérique ont été ajoutés dans le scénario précédent. Vous pouvez utiliser les commandes Windows PowerShell suivantes pour ajouter des enregistrements pour les étendues de centres de données européen zone.
 
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "151.1.0.1" -ZoneScope "DublinZoneScope”
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "AmsterdamZoneScope"
    

Pour plus d’informations, consultez [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

### <a name="bkmk_policies2"></a>Créer les stratégies DNS

Une fois que vous avez créé les partitions (étendues de zone) et que vous avez ajouté des enregistrements, vous devez créer des stratégies DNS qui distribuent les requêtes entrantes entre ces étendues.

Pour cet exemple, la distribution des requêtes entre les serveurs d’applications dans différents centres de données remplit les critères suivants.

1. Lorsque la requête DNS provient d’une source dans un sous-réseau client Amérique du Nord, 50 % des réponses DNS pointe vers le centre de données de Seattle, 25 % des réponses pointent vers le centre de données de Chicago et les 25 % restants des réponses pointent vers le centre de données de Dallas.
2. Lorsque la requête DNS est reçue à partir d’une source dans un sous-réseau client européen, 50 % des réponses DNS pointe vers le centre de données Dublin et 50 % des réponses DNS pointe vers le centre de données Amsterdam.
3. Lorsque la requête provient d’ailleurs dans le monde, les réponses DNS sont distribuées sur toutes les cinq centres de données.

Vous pouvez utiliser les commandes Windows PowerShell suivantes pour implémenter ces stratégies DNS.

    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaLBPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1; TexasZoneScope,1" -ZoneName "contosogiftservices.com" –ProcessingOrder 1
    
    Add-DnsServerQueryResolutionPolicy -Name "EuropeLBPolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 2
    
    Add-DnsServerQueryResolutionPolicy -Name "WorldWidePolicy" -Action ALLOW -FQDN "eq,*.contoso.com" -ZoneScope "SeattleZoneScope,1;ChicagoZoneScope,1; TexasZoneScope,1;DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3
    
    

Pour plus d’informations, consultez [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).

Vous avez maintenant créé une stratégie DNS qui fournit l’application équilibrage de charge sur les serveurs Web qui sont trouvent dans les cinq différents centres de données sur plusieurs continents.

Vous pouvez créer des milliers de stratégies DNS, en fonction de votre trafic en matière de gestion, et toutes les nouvelles stratégies sont appliquées dynamiquement, sans avoir à redémarrer le serveur DNS : sur les requêtes entrantes.
