---
title: Utiliser une stratégie DNS pour la géolocalisation gestion basée sur le trafic avec déploiements principaux / secondaires
description: Cette rubrique fait partie de la DNS stratégie scénario Guide pour Windows Server2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: a9ee7a56-f062-474f-a61c-9387ff260929
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4c78a0198e29fb59f30fd8ad776c7f200312d014
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-secondary-deployments"></a>Utiliser une stratégie DNS pour la géolocalisation gestion basée sur le trafic avec déploiements principaux / secondaires

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour savoir comment créer une stratégie DNS pour la gestion de géolocalisation en fonction du trafic lorsque votre déploiement DNS comprend des serveurs DNS principaux et secondaires.  

Le scénario précédent, [utiliser une stratégie DNS pour la géolocalisation le trafic de gestion avec des serveurs principaux](primary-geo-location.md), fourni des instructions pour configurer une stratégie DNS pour la gestion du trafic géolocalisation basée sur un serveur DNS principal. Dans l’infrastructure Internet, toutefois, les serveurs DNS sont largement déployées dans un modèle principaux / secondaires, où la copie accessible en écriture d’une zone stockée sur des serveurs principaux Sélectionnez et sécurisés, et des copies en lecture seule de la zone sont conservées sur plusieurs serveurs secondaires.   
  
Les serveurs secondaires utilisent les protocoles de transfert de zone faisant autorité transfert (AXFR) et transfert de Zone incrémentiel (IXFR) pour demander et recevoir des mises à jour de la zone qui incluent de nouvelles modifications aux zones sur les serveurs DNS principales.   
  
>[!NOTE]
>Pour plus d’informations sur les AXFR, voir la Internet Engineering Task Force (IETF) [demande de commentaires 5936](https://tools.ietf.org/rfc/rfc5936.txt). Pour plus d’informations sur les IXFR, consultez la Internet Engineering Task Force (IETF) [demande de commentaires 1995](https://tools.ietf.org/html/rfc1995).  
  
## <a name="bkmk_example"></a>GÉOLOCALISATION principaux / secondaires en fonction d’exemple de trafic de gestion  
Voici un exemple de comment vous pouvez utiliser une stratégie DNS dans un déploiement principaux / secondaires pour atteindre la redirection du trafic sur la base de l’emplacement physique du client qui exécute une requête DNS.  
  
Cet exemple utilise deux sociétés fictives - Contoso Cloud Services, qui offre web et le domaine qui héberge les solutions; services nourriture de Woodgrove Bank, qui fournit des services de livraison nourriture dans plusieurs villes du monde et qui dispose d’un site Web nommé woodgrove.com.  
  
Pour vous assurer que les clients woodgrove.com une expérience réactive à partir de son site Web, Woodgrove Bank souhaite clients européennes dirigées vers le centre de données européen et American dirigées vers le centre de données aux États-Unis. Clients situés ailleurs dans le monde peuvent être chargés d’une des centres de données.  
  
Services de Cloud de Contoso a deux centres de données, un aux États-Unis et un autre en Europe, sur lequel Contoso héberge sa nourriture classement portail pour woodgrove.com.  
  
Le déploiement de Contoso DNS inclut deux serveurs secondaires: **SecondaryServer1**, avec l’adresse IP 10.0.0.2; et **SecondaryServer2**, avec l’adresse IP 10.0.0.3. Ces serveurs secondaires sont agissant en tant que serveurs de noms dans les deux des différentes régions, avec SecondaryServer1 situé en Europe et SecondaryServer2 situés aux États-Unis.
  
Il existe une copie de zone accessible en écriture principale sur **PrimaryServer** (adresse IP 10.0.0.1), où la zone des modifications. Avec les transferts de zone standard vers les serveurs secondaires, les serveurs secondaires sont toujours à jour avec les nouvelles modifications à la zone sur le PrimaryServer.
  
L’illustration suivante décrit ce scénario.
  
![GÉOLOCALISATION principaux / secondaires en fonction d’exemple de trafic de gestion](../../media/Dns-Policy_PS1/dns_policy_primarysecondary1.jpg)  
   
## <a name="bkmk_works"></a>Comment fonctionne le système principal-secondaire DNS

Lorsque vous déployez gestion géolocalisation en fonction du trafic dans un déploiement de DNS principaux / secondaires, il est important de comprendre comment normale à une zone principale secondaire transferts de se produisent avant d’en savoir plus sur les transferts de niveau zone étendue. Les sections suivantes fournissent des informations sur la zone et les transferts de niveau zone étendue.  
  
- [Transferts de zone dans un déploiement de principaux / secondaires DNS](#bkmk_zone)  
- [Transferts de niveau zone étendue dans un déploiement de principaux / secondaires DNS](#bkmk_scope)  
  
### <a name="bkmk_zone"></a>Transferts de zone dans un déploiement de principaux / secondaires DNS

Vous pouvez créer un déploiement de principaux / secondaires DNS et synchroniser les zones avec les étapes suivantes.  
1. Lorsque vous installez DNS, la zone principale est créée sur le serveur DNS principal.  
2. Sur le serveur secondaire, créez les zones et spécifier les serveurs principaux.   
3. Sur les serveurs principaux, vous pouvez ajouter les serveurs secondaires en tant que serveurs secondaires approuvés sur la zone principale.   
4. Les zones secondaires effectuer une demande de transfert de zone complet (AXFR) et recevoir la copie de la zone.   
5. Si nécessaire, les serveurs principaux envoient des notifications sur les serveurs secondaires sur les mises à jour de zone.  
6. Serveurs secondaires effectuer une demande de transfert de zone incrémentiel (IXFR). Pour cette raison, les serveurs secondaires sont synchronisés avec le serveur principal.   
  
### <a name="bkmk_scope"></a>Transferts de niveau zone étendue dans un déploiement de principaux / secondaires DNS

Le scénario de gestion du trafic nécessite des étapes supplémentaires pour les zones de partition en portées autre zone. Pour cette raison, les étapes supplémentaires sont requises pour transférer les données contenues dans les étendues de zone vers les serveurs secondaires et pour transférer les stratégies et les sous-réseaux du Client DNS pour les serveurs secondaires.   
  
Après avoir configuré votre infrastructure DNS avec les serveurs principaux et secondaires, des transferts de niveau zone étendue sont effectuées automatiquement par le serveur DNS à l’aide de processus suivants.  
  
Pour garantir le transfert de niveau Zone étendue, les serveurs DNS utilisent les mécanismes d’Extension pour DNS (EDNS0) OPT RR. Toutes les demandes de transfert (AXFR ou IXFR) des zones avec des étendues proviennent avec un RR EDNS0 OPT, dont l’ID option est définie sur «65433» par défaut. Pour plus d’informations sur EDNSO, consultez l’IETF [demande de commentaires 6891](https://tools.ietf.org/html/rfc6891).  
  
La valeur de l’enregistrement de ressource OPT est le nom de l’étendue zone pour laquelle la demande est envoyée. Lorsqu’un serveur DNS principal reçoit ce paquet à partir d’un serveur secondaire approuvé, il interprète la demande comme provenant de cette étendue de la zone.   
  
Si le serveur principal a cette étendue de la zone il répond avec les données de transfert (XFR) à partir de cette étendue. La réponse contient un enregistrement de ressource OPT avec le même ID de l’option «65433» et une valeur définie pour la même étendue de la zone. Les serveurs secondaires reçoivent cette réponse, récupèrent les informations de l’étendue de la réponse et mettre à jour cette étendue particulière de la zone.  
  
Une fois ce processus, le serveur principal conserve une liste des serveurs secondaires approuvés qui ont envoyé une telle zone étendue demande pour les notifications.   
  
Pour toute mise à jour supplémentaire dans une étendue de la zone, une notification IXFR est envoyée vers les serveurs secondaires, avec le même RR OPT. L’étendue de la zone notifications qui effectue la demande IXFR contenant ce RR OPT et le même processus comme décrit ci-dessus suit.  
  
## <a name="bkmk_config"></a>Comment configurer une stratégie DNS pour la gestion le trafic en fonction de géolocalisation principaux / secondaires

Avant de commencer, vérifiez que vous avez effectué toutes les étapes décrites dans la rubrique [utiliser une stratégie DNS pour la géolocalisation le trafic de gestion avec des serveurs principaux](../../dns/deploy/Scenario--Use-DNS-Policy-for-Geo-Location-Based-Traffic-Management-with-Primary-Servers.md), et votre serveur DNS principal est configuré avec des zones, étendues de zone, sous-réseaux du Client DNS et une stratégie DNS.  
  
>[!NOTE]
> Les instructions de cette rubrique pour copier les sous-réseaux du Client DNS, des étendues de zone et des stratégies DNS à partir de serveurs principaux DNS vers des serveurs secondaires DNS sont pour votre configuration initiale de DNS et la validation. À l’avenir, vous devrez modifier les sous-réseaux du Client DNS, les étendues de zone et les paramètres de stratégies sur le serveur principal. Dans ce cas, vous pouvez créer des scripts d’automatisation pour conserver les serveurs secondaires synchronisés avec le serveur principal.  
  
Pour configurer une stratégie DNS pour les réponses principaux / secondaires géo-emplacement en fonction de requêtes, vous devez effectuer les étapes suivantes.  
  
- [Créer les Zones secondaires](#bkmk_secondary)  
- [Configurer les paramètres de transfert de Zone sur la Zone principale](#bkmk_zonexfer)  
- [Copiez les sous-réseaux du Client DNS](#bkmk_client)  
- [Créer les étendues de Zone sur le serveur secondaire](#bkmk_zonescopes)  
- [Configurer une stratégie DNS](#bkmk_dnspolicy)  
  
Les sections suivantes fournissent des instructions de configuration détaillées.  
  
>[!IMPORTANT]
>Les sections suivantes incluent des exemples de commandes Windows PowerShell qui contiennent des exemples de valeurs pour un grand nombre de paramètres. Assurez-vous que vous remplacez les exemples de valeurs de ces commandes par les valeurs appropriées pour votre déploiement avant d’exécuter ces commandes.  
><br>L’appartenance au groupe **DnsAdmins**, ou équivalent, est requis pour effectuer les procédures suivantes.  
  
### <a name="bkmk_secondary"></a>Créer les Zones secondaires

Vous pouvez créer la copie secondaire de la zone que vous souhaitez répliquer vers SecondaryServer1 et SecondaryServer2 (en supposant que les applets de commande sont en cours d’exécution à distance à partir d’un client de gestion unique).   
  
Par exemple, vous pouvez créer la copie secondaire de www.woodgrove.com sur SecondaryServer1 et SecondarySesrver2.  
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer les zones secondaires.  
  
    
    Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer1  
      
    Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer2  
      

Pour plus d’informations, voir [Add-DnsServerSecondaryZone](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserversecondaryzone?view=win10-ps).  
  
### <a name="bkmk_zonexfer"></a>Configurer les paramètres de transfert de Zone sur la Zone principale

Vous devez configurer les paramètres de zone principale afin que:

1. Les transferts de zone depuis le serveur principal pour les serveurs secondaires spécifiés sont autorisés.  
2. Notifications de mise à jour de zone sont envoyées par le serveur principal pour les serveurs secondaires.  
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour configurer les paramètres de transfert de zone sur la zone principale.
  
>[!NOTE]
>Dans la commande suivante, le paramètre **-avertir** Spécifie que le serveur principal envoie des notifications concernant les mises à jour à la liste Sélectionnez-le des serveurs secondaires.  
  
    
    Set-DnsServerPrimaryZone -Name "woodgrove.com" -Notify Notify -SecondaryServers "10.0.0.2,10.0.0.3" -SecureSecondaries TransferToSecureServers -ComputerName PrimaryServer  
     
  
Pour plus d’informations, voir [ensemble afin d’illustrer](https://https://docs.microsoft.com/powershell/module/dnsserver/set-dnsserverprimaryzone?view=win10-ps).  
  
  
### <a name="bkmk_client"></a>Copiez les sous-réseaux du Client DNS

Vous devez copier les sous-réseaux du Client DNS à partir du serveur principal pour les serveurs secondaires.
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour copier les sous-réseaux sur les serveurs secondaires.
  
    
    Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer1  
      
    Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer2  
      

Pour plus d’informations, voir [Add-DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).  
  
### <a name="bkmk_zonescopes"></a>Créer les étendues de Zone sur le serveur secondaire

Vous devez créer les étendues de zone sur les serveurs secondaires. Dans DNS, les étendues de zone commençaient également à demander cela est à partir du serveur principal. Toute modification sur les étendues de zone sur le serveur principal, une notification contenant les informations de l’étendue de zone est envoyée aux serveurs secondaires. Les serveurs secondaires peuvent ensuite mettre à jour leurs étendues zone changement.  
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer les étendues de zone sur les serveurs secondaires.  
  
    
    Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer1 -ErrorAction Ignore  
      
    Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer2 -ErrorAction Ignore  
  

>[!NOTE]
>Dans ces exemples de commandes, la **- ErrorAction ignorer** paramètre est inclus, car il existe une étendue de zone par défaut sur chaque zone. L’étendue de la zone par défaut ne peut pas être créé ou supprimé. Traitement en pipeline entraîne une tentative de création de cette étendue et il échoue. Vous pouvez également créer les étendues de zone par défaut sur les deux zones secondaires.  
  
Pour plus d’informations, voir [Add-DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).  
  
### <a name="bkmk_dnspolicy"></a>Configurer une stratégie DNS

Après avoir créé les sous-réseaux, les partitions (étendues zone) et que vous avez ajouté des enregistrements, vous devez créer des stratégies qui se connectent les sous-réseaux et les partitions, afin que lorsqu’une requête provient d’une source dans un des sous-réseaux de client DNS, la réponse de la requête est retournée à partir de la portée appropriée de la zone. Aucune stratégie n’est requis pour le mappage de l’étendue de la zone par défaut.  
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer une stratégie DNS qui renvoie les sous-réseaux du Client DNS et les étendues de zone.   
    
    $policy = Get-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName PrimaryServer  
      
    $policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer1  
      
    $policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer2  
      

Pour plus d’informations, voir [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Désormais, les serveurs DNS secondaires sont configurés avec les stratégies DNS requis pour rediriger le trafic en fonction de géolocalisation.  
  
Lorsque le serveur DNS reçoit des requêtes de résolution de nom, le serveur DNS évalue les champs dans la requête DNS contre les stratégies DNS configurés. Si l’adresse IP source dans la demande de résolution de nom correspond à aucune des stratégies, l’étendue de la zone associée est utilisée pour répondre à la requête, et l’utilisateur est dirigé vers la ressource est géographiquement le plus proche pour.   
  
Vous pouvez créer des milliers de stratégies DNS en fonction de votre trafic en matière de gestion, et toutes les nouvelles stratégies sont appliquées dynamiquement, sans redémarrer le serveur DNS - dans les requêtes entrantes.
