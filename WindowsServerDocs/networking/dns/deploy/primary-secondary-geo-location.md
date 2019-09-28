---
title: Utiliser une stratégie DNS pour la gestion du trafic basée sur la géolocalisation avec des déploiements principaux/secondaires
description: Cette rubrique fait partie du Guide de scénario de stratégie DNS pour Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: a9ee7a56-f062-474f-a61c-9387ff260929
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6a7836160fc7363ec3d7b2fb11e194db82970f9a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406155"
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-secondary-deployments"></a>Utiliser une stratégie DNS pour la gestion du trafic basée sur la géolocalisation avec des déploiements principaux/secondaires

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour apprendre à créer une stratégie DNS pour la gestion du trafic basée sur la géolocalisation lorsque votre déploiement DNS comprend des serveurs DNS principal et secondaire.  

Dans le scénario précédent, [Utilisez la stratégie DNS pour la gestion du trafic basée](primary-geo-location.md)sur la géolocalisation avec les serveurs principaux. vous trouverez des instructions pour la configuration de la stratégie DNS pour la gestion du trafic basée sur la géolocalisation sur un serveur DNS principal. Toutefois, dans l’infrastructure Internet, les serveurs DNS sont largement déployés dans un modèle principal-secondaire, où la copie accessible en écriture d’une zone est stockée sur des serveurs principaux sélectionnés et sécurisés, et les copies en lecture seule de la zone sont conservées sur plusieurs serveurs secondaires.   
  
Les serveurs secondaires utilisent les protocoles de transfert de zone (AXFR) et le transfert de zone incrémentiel (IXFR) pour demander et recevoir des mises à jour de zone qui incluent les nouvelles modifications apportées aux zones sur les serveurs DNS principaux.   
  
> [!NOTE]
> Pour plus d’informations sur AXFR, consultez la [demande de commentaires 5936 de](https://tools.ietf.org/rfc/rfc5936.txt)l’IETF (Internet Engineering Task Force). Pour plus d’informations sur IXFR, consultez la [demande de commentaires 1995 de](https://tools.ietf.org/html/rfc1995)l’IETF (Internet Engineering Task Force).  
  
## <a name="primary-secondary-geo-location-based-traffic-management-example"></a>Exemple de gestion du trafic basé sur la géolocalisation principale secondaire  
Vous trouverez ci-dessous un exemple de la façon dont vous pouvez utiliser la stratégie DNS dans un déploiement secondaire principal pour obtenir une redirection du trafic sur la base de l’emplacement physique du client qui exécute une requête DNS.  
  
Cet exemple utilise deux sociétés fictives : services Cloud contoso, qui fournissent des solutions d’hébergement Web et de domaine. et les services de la Woodgrove Bank, qui fournissent des services de distribution de nourriture dans plusieurs villes du monde entier, et qui ont un site Web nommé woodgrove.com.  
  
Pour vous assurer que les clients woodgrove.com bénéficient d’une expérience réactive à partir de leur site Web, la Woodgrove Bank souhaite des clients européens dirigés vers le centre de donnée européen et les clients américains dirigés vers le centre de centres des États-Unis. Les clients situés ailleurs dans le monde peuvent être dirigés vers l’un de ces centres de donnée.  
  
Contoso cloud services a deux centres de donnes, l’un aux États-Unis et l’autre en Europe, sur lequel contoso héberge son portail d’organisation des aliments pour woodgrove.com.  
  
Le déploiement de contoso DNS comprend deux serveurs secondaires : **SecondaryServer1**, avec l’adresse IP 10.0.0.2 ; et **SecondaryServer2**, avec l’adresse IP 10.0.0.3. Ces serveurs secondaires font office de serveurs de noms dans les deux régions, avec SecondaryServer1 situé en Europe et SecondaryServer2 situés aux États-Unis.
  
Il existe une copie de zone principale accessible en écriture sur **PrimaryServer** (adresse IP 10.0.0.1), où les modifications de zone sont effectuées. Avec les transferts de zone réguliers vers les serveurs secondaires, les serveurs secondaires sont toujours à jour avec toutes les nouvelles modifications apportées à la zone sur le PrimaryServer.
  
L’illustration suivante représente ce scénario.
  
![Exemple de gestion du trafic basé sur la géolocalisation principale secondaire](../../media/Dns-Policy_PS1/dns_policy_primarysecondary1.jpg)  
   
## <a name="how-the-dns-primary-secondary-system-works"></a>Fonctionnement du système secondaire DNS principal

Lorsque vous déployez la gestion du trafic basée sur la géolocalisation dans un déploiement DNS principal primaire, il est important de comprendre comment les transferts de zone secondaire principaux normaux se produisent avant d’en savoir plus sur les transferts au niveau de l’étendue de zone. Les sections suivantes fournissent des informations sur les transferts au niveau de l’étendue zone et zone.  
  
- [Transferts de zone dans un déploiement DNS principal/secondaire](#zone-transfers-in-a-dns-primary-secondary-deployment)  
- [Transferts au niveau de l’étendue de zone dans un déploiement DNS principal/secondaire](#zone-scope-level-transfers-in-a-dns-primary-secondary-deployment)  
  
### <a name="zone-transfers-in-a-dns-primary-secondary-deployment"></a>Transferts de zone dans un déploiement DNS principal/secondaire

Vous pouvez créer un déploiement DNS principal secondaire et synchroniser des zones en procédant comme suit.  
1. Lorsque vous installez DNS, la zone principale est créée sur le serveur DNS principal.  
2. Sur le serveur secondaire, créez les zones et spécifiez les serveurs principaux.   
3. Sur les serveurs principaux, vous pouvez ajouter les serveurs secondaires en tant que secondaires approuvés sur la zone principale.   
4. Les zones secondaires effectuent une demande de transfert de zone complète (AXFR) et reçoivent la copie de la zone.   
5. Si nécessaire, les serveurs principaux envoient des notifications aux serveurs secondaires à propos des mises à jour de zone.  
6. Les serveurs secondaires effectuent une demande de transfert de zone incrémentiel (IXFR). Pour cette raison, les serveurs secondaires restent synchronisés avec le serveur principal.   
  
### <a name="zone-scope-level-transfers-in-a-dns-primary-secondary-deployment"></a>Transferts au niveau de l’étendue de zone dans un déploiement DNS principal/secondaire

Le scénario de gestion du trafic nécessite des étapes supplémentaires pour partitionner les zones dans différentes étendues de zone. Pour cette raison, des étapes supplémentaires sont nécessaires pour transférer les données à l’intérieur des étendues de zone vers les serveurs secondaires, et pour transférer des stratégies et des sous-réseaux clients DNS vers les serveurs secondaires.   
  
Une fois que vous avez configuré votre infrastructure DNS avec des serveurs principaux et secondaires, les transferts au niveau de l’étendue de zone sont effectués automatiquement par DNS, à l’aide des processus suivants.  
  
Pour garantir le transfert au niveau de l’étendue de la zone, les serveurs DNS utilisent les mécanismes d’extension pour le RR DNS (EDNS0) OPT. Toutes les demandes de transfert de zone (AXFR ou IXFR) à partir des zones avec des étendues proviennent d’un enregistrement de ressource de sécurité EDNS0, dont l’ID d’option est défini sur « 65433 » par défaut. Pour plus d’informations sur EDNSO, consultez l’IETF [Request for comments 6891](https://tools.ietf.org/html/rfc6891).  
  
La valeur du RR OPT est le nom de l’étendue de zone pour laquelle la demande est envoyée. Lorsqu’un serveur DNS principal reçoit ce paquet d’un serveur secondaire approuvé, il interprète la demande comme étant disponible pour cette étendue de zone.   
  
Si le serveur principal a cette étendue de zone, il répond avec les données de transfert (XFR) de cette étendue. La réponse contient un enregistrement de ressource OPT avec le même ID d’option « 65433 » et la valeur définie sur la même étendue de zone. Les serveurs secondaires reçoivent cette réponse, récupèrent les informations d’étendue à partir de la réponse et mettent à jour cette étendue particulière de la zone.  
  
Après ce processus, le serveur principal gère une liste des secondaires de confiance qui ont envoyé une demande d’étendue de zone de ce type pour les notifications.   
  
Pour toute mise à jour supplémentaire dans une étendue de zone, une notification IXFR est envoyée aux serveurs secondaires, avec le même RR OPT. L’étendue de zone recevant cette notification fait de la demande IXFR contenant ce RR OPT et le même processus que celui décrit ci-dessus.  
  
## <a name="how-to-configure-dns-policy-for-primary-secondary-geo-location-based-traffic-management"></a>Comment configurer la stratégie DNS pour la gestion du trafic basé sur l’emplacement géographique secondaire

Avant de commencer, assurez-vous que vous avez effectué toutes les étapes de la rubrique [utiliser une stratégie DNS pour la gestion du trafic basée sur la géolocalisation avec des serveurs principaux](../../dns/deploy/Scenario--Use-DNS-Policy-for-Geo-Location-Based-Traffic-Management-with-Primary-Servers.md)et que votre serveur DNS principal est configuré avec des zones, des étendues de zone, des sous-réseaux clients DNS et DNS. renvoi.  
  
> [!NOTE]
> Les instructions de cette rubrique pour copier les sous-réseaux du client DNS, les étendues de zone et les stratégies DNS des serveurs principaux DNS vers les serveurs DNS secondaires sont destinées à votre configuration DNS initiale et à la validation. À l’avenir, vous souhaiterez peut-être modifier les paramètres des sous-réseaux du client DNS, des étendues de zone et des stratégies sur le serveur principal. Dans ce cas, vous pouvez créer des scripts d’automatisation pour maintenir la synchronisation des serveurs secondaires avec le serveur principal.  
  
Pour configurer la stratégie DNS pour les réponses de requête de géolocalisation principale secondaire, vous devez effectuer les étapes suivantes.  
  
- [Créer les zones secondaires](#create-the-secondary-zones)  
- [Configurer les paramètres de transfert de zone sur la zone principale](#configure-the-zone-transfer-settings-on-the-primary-zone)  
- [Copier les sous-réseaux du client DNS](#copy-the-dns-client-subnets)  
- [Créer les étendues de zone sur le serveur secondaire](#create-the-zone-scopes-on-the-secondary-server)  
- [Configurer la stratégie DNS](#configure-dns-policy)  
  
Les sections suivantes fournissent des instructions de configuration détaillées.  
  
> [!IMPORTANT]
> Les sections suivantes incluent des exemples de commandes Windows PowerShell qui contiennent des exemples de valeurs pour de nombreux paramètres. Veillez à remplacer les valeurs d’exemple dans ces commandes par des valeurs appropriées pour votre déploiement avant d’exécuter ces commandes.  
> 
> L’appartenance à **DnsAdmins**, ou équivalent, est nécessaire pour effectuer les procédures suivantes.  
  
### <a name="create-the-secondary-zones"></a>Créer les zones secondaires

Vous pouvez créer la copie secondaire de la zone que vous souhaitez répliquer sur SecondaryServer1 et SecondaryServer2 (en supposant que les applets de commande sont exécutées à distance à partir d’un client de gestion unique).   
  
Par exemple, vous pouvez créer la copie secondaire de www.woodgrove.com sur SecondaryServer1 et SecondarySesrver2.  
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer les zones secondaires.  
  
    
    Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer1  
      
    Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer2  
      

Pour plus d’informations, consultez [Add-DnsServerSecondaryZone](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserversecondaryzone?view=win10-ps).  
  
### <a name="configure-the-zone-transfer-settings-on-the-primary-zone"></a>Configurer les paramètres de transfert de zone sur la zone principale

Vous devez configurer les paramètres de la zone principale de manière à ce que :

1. Les transferts de zone à partir du serveur principal vers les serveurs secondaires spécifiés sont autorisés.  
2. Les notifications de mise à jour de zone sont envoyées par le serveur principal aux serveurs secondaires.  
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour configurer les paramètres de transfert de zone sur la zone principale.
  
> [!NOTE]
> Dans l’exemple de commande suivant, le paramètre **-Notify** spécifie que le serveur principal enverra des notifications sur les mises à jour de la liste de sélection des secondaires.  
  
    
    Set-DnsServerPrimaryZone -Name "woodgrove.com" -Notify Notify -SecondaryServers "10.0.0.2,10.0.0.3" -SecureSecondaries TransferToSecureServers -ComputerName PrimaryServer  
     
  
Pour plus d’informations, consultez [Set-DnsServerPrimaryZone](https://docs.microsoft.com/powershell/module/dnsserver/set-dnsserverprimaryzone?view=win10-ps).  
  
  
### <a name="copy-the-dns-client-subnets"></a>Copier les sous-réseaux du client DNS

Vous devez copier les sous-réseaux du client DNS du serveur principal vers les serveurs secondaires.
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour copier les sous-réseaux sur les serveurs secondaires.
  
    
    Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer1  
      
    Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer2  
      

Pour plus d’informations, consultez [Add-DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).  
  
### <a name="create-the-zone-scopes-on-the-secondary-server"></a>Créer les étendues de zone sur le serveur secondaire

Vous devez créer les étendues de zone sur les serveurs secondaires. Dans DNS, les étendues de zone commencent également à demander des XFRs à partir du serveur principal. En cas de modification des étendues de zone sur le serveur principal, une notification contenant les informations d’étendue de zone est envoyée aux serveurs secondaires. Les serveurs secondaires peuvent ensuite mettre à jour leurs étendues de zone avec des modifications incrémentielles.  
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer les étendues de zone sur les serveurs secondaires.  
  
    
    Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer1 -ErrorAction Ignore  
      
    Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer2 -ErrorAction Ignore  
  

> [!NOTE]
> Dans ces exemples de commandes, le paramètre **-ErrorAction ignore** est inclus, car il existe une étendue de zone par défaut dans chaque zone. Impossible de créer ou de supprimer l’étendue de zone par défaut. Le traitement en pipeline entraîne une tentative de création de cette étendue et échoue. Vous pouvez également créer les étendues de zone non définie par défaut sur deux zones secondaires.  
  
Pour plus d’informations, consultez [Add-DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).  
  
### <a name="configure-dns-policy"></a>Configurer la stratégie DNS

Une fois que vous avez créé les sous-réseaux, les partitions (étendues de zone) et que vous avez ajouté des enregistrements, vous devez créer des stratégies qui connectent les sous-réseaux et les partitions, de sorte que lorsqu’une requête provient d’une source dans l’un des sous-réseaux du client DNS, la réponse à la requête est retournée à partir de étendue correcte de la zone. Aucune stratégie n’est requise pour le mappage de l’étendue de zone par défaut.  
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer une stratégie DNS qui lie les sous-réseaux du client DNS et les étendues de zone.   
    
    $policy = Get-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName PrimaryServer  
      
    $policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer1  
      
    $policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer2  
      

Pour plus d’informations, consultez [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Désormais, les serveurs DNS secondaires sont configurés avec les stratégies DNS requises pour rediriger le trafic en fonction de la géolocalisation.  
  
Lorsque le serveur DNS reçoit des requêtes de résolution de noms, le serveur DNS évalue les champs de la requête DNS par rapport aux stratégies DNS configurées. Si l’adresse IP source de la demande de résolution de noms correspond à l’une des stratégies, l’étendue de la zone associée est utilisée pour répondre à la requête, et l’utilisateur est dirigé vers la ressource qui est géographiquement la plus proche.   
  
Vous pouvez créer des milliers de stratégies DNS en fonction de vos besoins en matière de gestion du trafic, et toutes les nouvelles stratégies sont appliquées de manière dynamique, sans redémarrer le serveur DNS-sur les requêtes entrantes.
