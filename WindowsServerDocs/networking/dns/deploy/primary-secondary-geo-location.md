---
title: Utiliser une stratégie DNS pour la gestion du trafic basée sur la géolocalisation avec des déploiements principaux/secondaires
description: Cette rubrique fait partie de DNS stratégie scénario Guide pour Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: a9ee7a56-f062-474f-a61c-9387ff260929
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cf66a306c7f023852cec93d6458e74a99c46c831
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812103"
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-secondary-deployments"></a>Utiliser une stratégie DNS pour la gestion du trafic basée sur la géolocalisation avec des déploiements principaux/secondaires

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour apprendre à créer une stratégie DNS pour la gestion du trafic en fonction de localisation géographique lorsque votre déploiement DNS inclut les serveurs DNS principaux et secondaires.  

Le scénario précédent, [utiliser une stratégie DNS pour l’emplacement géographique en fonction de la gestion du trafic avec des serveurs principaux](primary-geo-location.md), fourni des instructions pour configurer une stratégie DNS pour la gestion du trafic en fonction de localisation géographique sur un serveur DNS principal. Dans l’infrastructure Internet, toutefois, les serveurs DNS sont largement déployées dans un modèle principal-secondaire, où la copie accessible en écriture d’une zone est stockée sur select et sécurisées des serveurs principaux et des copies en lecture seule de la zone sont conservées sur plusieurs serveurs secondaires.   
  
Les serveurs secondaires utilisent les protocoles de transfert de zone transfert faisant autorité (AXFR) et transfert de Zone incrémentiel (IXFR) pour demander et recevoir des mises à jour de la zone qui incluent de nouvelles modifications aux zones sur les serveurs DNS principales.   
  
> [!NOTE]
> Pour plus d’informations sur AXFR, consultez la Internet Engineering Task Force (IETF) [demande de commentaires 5936](https://tools.ietf.org/rfc/rfc5936.txt). Pour plus d’informations sur IXFR, consultez la Internet Engineering Task Force (IETF) [demande de commentaires 1995](https://tools.ietf.org/html/rfc1995).  
  
## <a name="primary-secondary-geo-location-based-traffic-management-example"></a>Emplacement géographique principal-secondaire en fonction d’exemple de gestion de trafic  
Voici un exemple de comment vous pouvez utiliser une stratégie DNS dans un déploiement de principaux / secondaires pour atteindre la redirection du trafic en fonction de l’emplacement physique du client qui effectue une requête DNS.  
  
Cet exemple utilise deux entreprises fictives - Contoso Cloud Services, qui offre web et domaine qui héberge des solutions ; et Woodgrove Food Services, qui fournit des services de distribution de produits alimentaires dans plusieurs villes du monde, et qui a un site Web nommé woodgrove.com.  
  
Pour vous assurer que les clients woodgrove.com une expérience réactive à partir de son site Web, Woodgrove souhaite que les clients européens dirigés vers le centre de données européen et American dirigé vers le centre de données des États-Unis. Les clients situés ailleurs dans le monde entier peuvent être dirigés vers un des centres de données.  
  
Services de Cloud de Contoso possède deux centres de données, un dans le fuseau horaire et l’autre en Europe, sur lequel Contoso héberge son les portail pour woodgrove.com de commande.  
  
Le déploiement de DNS de Contoso inclut deux serveurs secondaires : **SecondaryServer1**, avec l’adresse IP 10.0.0.2 ; et **SecondaryServer2**, avec l’adresse IP 10.0.0.3. Ces serveurs secondaires agissent comme serveurs de noms dans deux régions différentes, avec SecondaryServer1 situé en Europe et SecondaryServer2 situés aux États-Unis
  
Il existe une copie de la zone accessible en écriture principale sur **PrimaryServer** (adresse IP 10.0.0.1), où la zone est modifiée. Avec les transferts de zone standard vers les serveurs secondaires, les serveurs secondaires sont toujours à jour avec les nouvelles modifications à la zone sur le PrimaryServer.
  
L’illustration suivante représente ce scénario.
  
![Emplacement géographique principal-secondaire en fonction d’exemple de gestion de trafic](../../media/Dns-Policy_PS1/dns_policy_primarysecondary1.jpg)  
   
## <a name="how-the-dns-primary-secondary-system-works"></a>Comment fonctionne le système principal-secondaire DNS

Lorsque vous déployez gestion emplacement géographique en fonction du trafic dans un déploiement de DNS principal-secondaire, il est important de comprendre comment normale à une zone principale-secondaire transferts se produisent avant de découvrir des transferts de zone étendue au niveau. Les sections suivantes fournissent des informations sur la zone et les transferts de zone étendue au niveau.  
  
- [Transferts de zone dans un déploiement de principaux / secondaires DNS](#zone-transfers-in-a-dns-primary-secondary-deployment)  
- [Transferts de niveau zone étendue dans un déploiement de principaux / secondaires DNS](#zone-scope-level-transfers-in-a-dns-primary-secondary-deployment)  
  
### <a name="zone-transfers-in-a-dns-primary-secondary-deployment"></a>Transferts de zone dans un déploiement de principaux / secondaires DNS

Vous pouvez créer un déploiement de principaux / secondaires DNS et synchroniser des zones en procédant comme suit.  
1. Lorsque vous installez DNS, la zone principale est créée sur le serveur DNS principal.  
2. Sur le serveur secondaire, créer les zones et spécifier les serveurs principaux.   
3. Sur les serveurs principaux, vous pouvez ajouter les serveurs secondaires approuvé bases de données secondaires sur la zone principale.   
4. Les zones secondaires effectuer une demande de transfert de zone complet (AXFR) et recevoir la copie de la zone.   
5. Si nécessaire, les serveurs principaux envoient des notifications pour les serveurs secondaires sur les mises à jour de la zone.  
6. Serveurs secondaires effectuer une demande de transfert de zone incrémentiel (IXFR). Pour cette raison, les serveurs secondaires sont synchronisées avec le serveur principal.   
  
### <a name="zone-scope-level-transfers-in-a-dns-primary-secondary-deployment"></a>Transferts de niveau zone étendue dans un déploiement de principaux / secondaires DNS

Le scénario de gestion du trafic requiert des étapes supplémentaires pour partitionner les zones étendues de zone différente. Pour cette raison, les étapes supplémentaires sont nécessaires pour transférer les données dans des étendues de la zone pour les serveurs secondaires et pour transférer des stratégies et les sous-réseaux du Client DNS vers les serveurs secondaires.   
  
Après avoir configuré votre infrastructure DNS avec les serveurs principaux et secondaires, les transferts de zone étendue au niveau sont exécutées automatiquement par le système DNS, en effectuant les tâches suivantes.  
  
Pour garantir le transfert de niveau Zone étendue, serveurs DNS utilisent les mécanismes d’Extension pour DNS (EDNS0) OPT RR. Toutes les demandes de transfert (AXFR ou IXFR) des zones avec des étendues proviennent avec un RR EDNS0 OPT, dont l’ID d’option a la valeur « 65433 » par défaut. Pour plus d’informations sur EDNSO, consultez l’IETF [demande de commentaires 6891](https://tools.ietf.org/html/rfc6891).  
  
La valeur de l’enregistrement de ressource OPT est le nom d’étendue de zone pour laquelle la demande est envoyée. Lorsqu’un serveur DNS principal reçoit ce paquet à partir d’un serveur secondaire approuvé, il interprète la demande en provenance de cette étendue de la zone.   
  
Si le serveur principal a l’étendue de la zone il répond avec les données de transfert (XFR) à partir de cette étendue. La réponse contient un enregistrement de ressource OPT avec le même ID de l’option « 65433 » et une valeur définie sur la même étendue de la zone. Les serveurs secondaires reçoivent cette réponse, récupèrent les informations de l’étendue de la réponse et mettre à jour de cette étendue particulière de la zone.  
  
Après ce processus, le serveur principal gère une liste de bases de données secondaires approuvées qui a envoyé une telle zone étendue demande pour les notifications.   
  
Pour toute mise à jour supplémentaire dans une étendue de la zone, une notification de IXFR est envoyée aux serveurs secondaires, avec le même enregistrement de ressource OPT. L’étendue de la zone reçoit cette notification effectue la demande IXFR contenant ce RR OPT et le même processus comme décrit ci-dessus suit.  
  
## <a name="how-to-configure-dns-policy-for-primary-secondary-geo-location-based-traffic-management"></a>Comment configurer une stratégie DNS pour gestion du trafic en fonction des emplacement géographique principal-secondaire

Avant de commencer, assurez-vous que vous avez effectué toutes les étapes décrites dans la rubrique [utiliser une stratégie DNS pour l’emplacement géographique en fonction de la gestion du trafic avec des serveurs principaux](../../dns/deploy/Scenario--Use-DNS-Policy-for-Geo-Location-Based-Traffic-Management-with-Primary-Servers.md), et votre serveur DNS principal est configuré avec les zones, étendues de la zone, le Client DNS Des sous-réseaux et une stratégie DNS.  
  
> [!NOTE]
> Les instructions de cette rubrique pour copier les sous-réseaux du Client DNS, les étendues de zone et les stratégies DNS à partir de serveurs principaux DNS vers des serveurs DNS secondaires sont pour votre configuration initiale de DNS et la validation. À l’avenir vous souhaiterez peut-être modifier les sous-réseaux du Client DNS, les étendues de zone et les paramètres de stratégies sur le serveur principal. Dans ce cas, vous pouvez créer des scripts d’automatisation afin de conserver les serveurs secondaires synchronisés avec le serveur principal.  
  
Pour configurer une stratégie DNS pour les réponses aux requêtes principaux / secondaires emplacement géographique en fonction, vous devez effectuer les étapes suivantes.  
  
- [Créer les Zones de la base de données secondaire](#create-the-secondary-zones)  
- [Configurer les paramètres de transfert de Zone sur la Zone principale](#configure-the-zone-transfer-settings-on-the-primary-zone)  
- [Copie les sous-réseaux du Client DNS](#copy-the-dns-client-subnets)  
- [Créer des étendues de la Zone sur le serveur secondaire](#create-the-zone-scopes-on-the-secondary-server)  
- [Configurer une stratégie DNS](#configure-dns-policy)  
  
Les sections suivantes fournissent des instructions de configuration détaillées.  
  
> [!IMPORTANT]
> Les sections suivantes incluent des exemples de commandes Windows PowerShell qui contiennent des exemples de valeurs de paramètres. Veillez à remplacer les exemples de valeurs dans ces commandes avec les valeurs appropriées pour votre déploiement avant d’exécuter ces commandes.  
> 
> L’appartenance au **DnsAdmins**, ou équivalent, est requis pour effectuer les procédures suivantes.  
  
### <a name="create-the-secondary-zones"></a>Créer les Zones de la base de données secondaire

Vous pouvez créer la copie secondaire de la zone que vous souhaitez répliquer vers SecondaryServer1 et SecondaryServer2 (en supposant que les applets de commande sont en cours d’exécution à distance à partir d’un client de gestion unique).   
  
Par exemple, vous pouvez créer la copie secondaire de www.woodgrove.com sur SecondaryServer1 et SecondarySesrver2.  
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer des zones secondaires.  
  
    
    Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer1  
      
    Add-DnsServerSecondaryZone -Name "woodgrove.com" -ZoneFile "woodgrove.com.dns" -MasterServers 10.0.0.1 -ComputerName SecondaryServer2  
      

Pour plus d’informations, consultez [Add-DnsServerSecondaryZone](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserversecondaryzone?view=win10-ps).  
  
### <a name="configure-the-zone-transfer-settings-on-the-primary-zone"></a>Configurer les paramètres de transfert de Zone sur la Zone principale

Vous devez configurer les paramètres de zone principale afin que :

1. Transferts de zone à partir du serveur principal vers les serveurs secondaires spécifiés sont autorisés.  
2. Notifications de mise à jour de zone sont envoyées par le serveur principal pour les serveurs secondaires.  
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour configurer les paramètres de transfert de zone sur la zone principale.
  
> [!NOTE]
> Dans la commande suivante, le paramètre **-notifier** Spécifie que le serveur principal envoie des notifications concernant les mises à jour à la liste de sélection de bases de données secondaires.  
  
    
    Set-DnsServerPrimaryZone -Name "woodgrove.com" -Notify Notify -SecondaryServers "10.0.0.2,10.0.0.3" -SecureSecondaries TransferToSecureServers -ComputerName PrimaryServer  
     
  
Pour plus d’informations, consultez [ensemble afin d’illustrer](https://docs.microsoft.com/powershell/module/dnsserver/set-dnsserverprimaryzone?view=win10-ps).  
  
  
### <a name="copy-the-dns-client-subnets"></a>Copie les sous-réseaux du Client DNS

Vous devez copier les sous-réseaux du Client DNS à partir du serveur principal pour les serveurs secondaires.
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour copier les sous-réseaux vers les serveurs secondaires.
  
    
    Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer1  
      
    Get-DnsServerClientSubnet -ComputerName PrimaryServer | Add-DnsServerClientSubnet -ComputerName SecondaryServer2  
      

Pour plus d’informations, consultez [Add-DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).  
  
### <a name="create-the-zone-scopes-on-the-secondary-server"></a>Créer des étendues de la Zone sur le serveur secondaire

Vous devez créer les étendues de la zone sur les serveurs secondaires. Dans DNS, les étendues de zone également démarrer demande cela est le serveur principal. Avec toute modification sur les étendues de la zone sur le serveur principal, une notification qui contient les informations d’étendue de zone est envoyée aux serveurs secondaires. Les serveurs secondaires peuvent ensuite à jour leurs étendues de zone avec modification incrémentielle.  
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer des étendues de la zone sur les serveurs secondaires.  
  
    
    Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer1 -ErrorAction Ignore  
      
    Get-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName PrimaryServer|Add-DnsServerZoneScope -ZoneName "woodgrove.com" -ComputerName SecondaryServer2 -ErrorAction Ignore  
  

> [!NOTE]
> Dans ces exemples de commandes, le **- ErrorAction ignorer** paramètre n’est inclus, car une étendue de la zone par défaut existe sur chaque zone. L’étendue de la zone par défaut ne peut pas être créé ou supprimé. Traitement en pipeline entraîne une tentative de création de cette étendue et l’opération échoue. Vous pouvez également créer les étendues de la zone non définie par défaut sur deux zones secondaires.  
  
Pour plus d’informations, consultez [Add-DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).  
  
### <a name="configure-dns-policy"></a>Configurer une stratégie DNS

Une fois que vous avez créé les sous-réseaux, les partitions (étendues de zone) et que vous avez ajouté des enregistrements, vous devez créer des stratégies qui connectent les sous-réseaux et les partitions, afin que lorsqu’une requête provient d’une source dans un des sous-réseaux de client DNS, la réponse de requête est retournée à partir de l’étendue correcte de la zone. Aucune stratégie n’est requis pour le mappage de l’étendue de la zone par défaut.  
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer une stratégie DNS qui lie les sous-réseaux du Client DNS et les étendues de zone.   
    
    $policy = Get-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName PrimaryServer  
      
    $policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer1  
      
    $policy | Add-DnsServerQueryResolutionPolicy -ZoneName "woodgrove.com" -ComputerName SecondaryServer2  
      

Pour plus d’informations, consultez [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Désormais, les serveurs DNS secondaires sont configurés avec les stratégies DNS requises pour rediriger le trafic en fonction de l’emplacement géographique.  
  
Lorsque le serveur DNS reçoit des requêtes de résolution de nom, le serveur DNS évalue les champs dans la requête DNS sur les stratégies DNS configurés. Si l’adresse IP source dans la demande de résolution de nom correspond à aucune des stratégies, l’étendue de la zone associée est utilisée pour répondre à la requête et l’utilisateur est dirigé vers la ressource qui est géographiquement le plus proche les.   
  
Vous pouvez créer des milliers de stratégies DNS, en fonction de votre trafic en matière de gestion, et toutes les nouvelles stratégies sont appliquées dynamiquement, sans avoir à redémarrer le serveur DNS : sur les requêtes entrantes.
