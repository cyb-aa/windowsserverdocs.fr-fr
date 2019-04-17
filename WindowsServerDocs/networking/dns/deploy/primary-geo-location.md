---
title: Utiliser une stratégie DNS pour la géolocalisation en fonction de gestion du trafic avec des serveurs principaux
description: Cette rubrique fait partie de la DNS stratégie scénario Guide pour Windows Server2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: ef9828f8-c0ad-431d-ae52-e2065532e68f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bd72e13cd3d2d7f3ca886afcdcf97e824ef227f5
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers"></a>Utiliser une stratégie DNS pour la géolocalisation en fonction de gestion du trafic avec des serveurs principaux

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour savoir comment configurer une stratégie DNS pour permettre aux serveurs DNS principaux de répondre aux requêtes de client DNS basées sur l’emplacement géographique du client et la ressource à laquelle le client tente de se connecter, en fournissant au client avec l’adresse IP de la ressource la plus proche.  
  
>[!IMPORTANT]  
>Ce scénario montre comment déployer une stratégie DNS pour la gestion de géolocalisation en fonction du trafic lorsque vous utilisez des serveurs DNS principaux uniquement. Vous pouvez également effectuer la gestion de géolocalisation en fonction du trafic lorsque vous avez des serveurs DNS principaux et secondaires. Si vous avez un déploiement principaux / secondaires, tout d’abord effectuer les étapes décrites dans cette rubrique, puis effectuez les étapes fournies dans la rubrique [utiliser une stratégie DNS pour la géolocalisation le trafic de gestion avec des déploiements principaux / secondaires](primary-secondary-geo-location.md).

Avec les nouvelles stratégies DNS, vous pouvez créer une stratégie DNS qui permet au serveur DNS pour répondre à une requête client demande l’adresse IP d’un serveur Web. Les instances du serveur Web peuvent se trouver dans différents centres de données à différents emplacements physiques. DNS, vous pouvez évaluer le client et les emplacements de serveur Web, puis répondre à la demande du client en fournissant au client avec un serveur Web adresse IP pour un serveur Web qui se trouve physiquement plus proche pour le client.  

Vous pouvez utiliser les paramètres de stratégie DNS suivants pour contrôler les réponses du serveur DNS aux requêtes des clients DNS. 

- **Sous-réseau client**. Nom d’un sous-réseau client prédéfini. Permet de vérifier le sous-réseau à partir de laquelle la requête a été envoyée.
- **Protocole de transport**. Protocole utilisé dans la requête de transport. Entrées possibles sont **UDP** et **TCP**.
- **Protocole Internet**. Protocole réseau utilisé dans la requête. Entrées possibles sont **IPv4** et **IPv6**.
- **Adresse IP de l’Interface du serveur**. Adresse IP de l’interface réseau du serveur DNS qui ont reçu la requête DNS.
- **Nom de domaine complet**. Le domaine nom complet (FQDN) de l’enregistrement dans la requête, avec la possibilité d’utiliser un caractère générique.
- **Type de requête**. Type d’enregistrement interrogé (A, SRV, TXT, etc.).
- **Heure de la journée**. Heure de que réception de la requête. 
  
Vous pouvez combiner les critères suivants avec un opérateur logique (et/ou) pour élaborer les expressions de stratégie. Lorsque ces expressions correspondent, les stratégies sont tenues d’effectuer une des actions suivantes.
 
- **Ignorer**. Le serveur DNS rejette la requête.          
- **Refuser**. Le serveur DNS répond à cette requête avec une réponse de l’échec.          
- **Autoriser**. Le serveur DNS répond avec réponse le trafic géré.          
  
##  <a name="bkmk_example"></a>GÉOLOCALISATION en fonction d’exemple de trafic de gestion

Voici un exemple de comment vous pouvez utiliser une stratégie DNS pour obtenir la redirection du trafic sur la base de l’emplacement physique du client qui exécute une requête DNS.   
  
Cet exemple utilise deux sociétés fictives - Contoso Cloud Services, qui offre web et le domaine qui héberge les solutions; services nourriture de Woodgrove Bank, qui fournit des services de livraison nourriture dans plusieurs villes du monde et qui dispose d’un site Web nommé woodgrove.com.  
  
Services de Cloud de Contoso a deux centres de données, un aux États-Unis et un autre en Europe. Le centre de données européen héberge un classement portail pour woodgrove.com de cuisine.   
  
Pour vous assurer que les clients woodgrove.com une expérience réactive à partir de son site Web, Woodgrove Bank souhaite clients européennes dirigées vers le centre de données européen et American dirigées vers le centre de données aux États-Unis. Clients situés ailleurs dans le monde peuvent être chargés d’une des centres de données.   
  
L’illustration suivante décrit ce scénario.  
  
![GÉOLOCALISATION en fonction d’exemple de trafic de gestion](../../media/DNS-Policy-Geo1/dns_policy_geo1.png)  
  
##  <a name="bkmk_works"></a>Comment le DNS processus de résolution de nom fonctionne  
  
Pendant le processus de résolution de nom, l’utilisateur tente de se connecter à www.woodgrove.com. Cela entraîne une demande de résolution de nom DNS est envoyée au serveur DNS qui est configuré dans les propriétés de connexion réseau sur l’ordinateur de l’utilisateur. En règle générale, cela est le serveur DNS fourni par le fournisseur de services Internet local agissant comme un résolveur mise en cache et est désigné comme le LDNS.   
  
Si le nom DNS n’est pas présent dans le cache local de LDNS, le serveur LDNS transfère la requête au serveur DNS faisant autorité pour woodgrove.com. Le serveur DNS faisant autorité répond avec l’enregistrement demandé (www.woodgrove.com) pour le serveur LDNS, qui à son tour met en cache de l’enregistrement localement avant de les envoyer à l’ordinateur de l’utilisateur.  
  
Services Cloud de Contoso utilise des stratégies du serveur DNS, le serveur DNS faisant autorité que les hôtes contoso.com est configuré pour renvoyer la géolocalisation en fonction des réponses de trafic géré. Cela entraîne la direction de Clients européen vers le centre de données européen et la direction de Clients American au centre de données aux États-Unis, comme illustré dans l’illustration.  
  
Dans ce scénario, le serveur DNS faisant autorité voit généralement la demande de résolution de nom en provenance du serveur LDNS, très rarement, à partir de l’ordinateur de l’utilisateur. Pour cette raison, l’adresse IP source dans la demande de résolution de nom comme illustré par le serveur DNS faisant autorité est celui du serveur LDNS et non d’ordinateur de l’utilisateur. Cependant, à l’aide de l’adresse IP du serveur LDNS lorsque vous configurez la géolocalisation basé requête réponses fournit une estimation juste de l’emplacement géographique de l’utilisateur, car l’utilisateur est interrogeant le serveur DNS de son fournisseur de services Internet local.  
  
>[!NOTE]  
>Stratégies DNS utilisent l’adresse IP de l’expéditeur dans les paquets UDP/TCP qui contient la requête DNS. Si la requête atteint le serveur principal par le biais de plusieurs sauts résolveur/LDNS, la stratégie considère seulement l’adresse IP de l’utilitaire de résolution dernière à partir de laquelle le serveur DNS reçoit la requête.  
  
##  <a name="bkmk_config"></a>Comment configurer une stratégie DNS pour les réponses aux requêtes en fonction de géolocalisation  
Pour configurer une stratégie DNS pour les réponses aux requêtes de géolocalisation en fonction, vous devez effectuer les étapes suivantes.  
  
1. [Créer les sous-réseaux du Client DNS](#bkmk_subnets)  
2. [Créer les étendues de la Zone](#bkmk_scopes)  
3. [Ajouter des enregistrements pour les étendues de Zone](#bkmk_records)  
4. [Créer les stratégies](#bkmk_policies)  
  
>[!NOTE]  
>Vous devez effectuer ces étapes sur le serveur DNS faisant autorité pour la zone que vous souhaitez configurer. L’appartenance au groupe **DnsAdmins**, ou équivalent, est requis pour effectuer les procédures suivantes.  
  
Les sections suivantes fournissent des instructions de configuration détaillées.  
  
>[!IMPORTANT]  
>Les sections suivantes incluent des exemples de commandes Windows PowerShell qui contiennent des exemples de valeurs pour un grand nombre de paramètres. Assurez-vous que vous remplacez les exemples de valeurs de ces commandes par les valeurs appropriées pour votre déploiement avant d’exécuter ces commandes.  
  
### <a name="bkmk_subnets"></a>Créer les sous-réseaux du Client DNS  
  
La première étape consiste à identifier les sous-réseaux ou l’espace d’adressage IP des régions pour lequel vous souhaitez rediriger le trafic. Par exemple, si vous souhaitez rediriger le trafic pour les États-Unis et en Europe, vous devez identifier les sous-réseaux ou les espaces d’adressage IP de ces régions.  
  
Vous pouvez obtenir ces informations à partir de cartes géo-IP. En fonction de ces distributions géo-IP, vous devez créer les «sous-réseaux du Client DNS». Un sous-réseau de Client DNS est un regroupement logique des sous-réseaux IPv4 ou IPv6 à partir de laquelle les requêtes sont envoyées à un serveur DNS.  
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer des sous-réseaux de Client DNS.  
  
      
    Add-DnsServerClientSubnet -Name "USSubnet" -IPv4Subnet "192.0.0.0/24"  
      
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24"  
      
  
Pour plus d’informations, voir [Add-DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).  
  
### <a name="bkmk_scopes"></a>Créer des étendues de Zone  
Après avoir configuré les sous-réseaux du client, vous devez partitionner la zone dont le trafic que vous souhaitez rediriger les deux étendues zone différents, une étendue pour chacun de ces sous-réseaux de Client DNS que vous avez configuré.   
  
Par exemple, si vous souhaitez rediriger le trafic pour le nom DNS www.woodgrove.com, vous devez créer deux étendues autre zone dans la zone woodgrove.com, un pour les États-Unis et un pour l’Europe.  
  
Une étendue de la zone est une instance unique de la zone. Une zone DNS peut avoir plusieurs étendues de zone, avec chaque étendue de la zone qui contient son propre ensemble d’enregistrements DNS. L’enregistrement même peut être présent dans plusieurs étendues, avec des adresses IP différentes ou les mêmes adresses IP.  
  
>[!NOTE]  
>Par défaut, une étendue de la zone existe sur les zones DNS. Cette étendue de la zone a le même nom que la zone et les opérations de DNS héritées fonctionnent sur cette étendue.   
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer des étendues de zone.  
  
      
    Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "USZoneScope"  
      
    Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "EuropeZoneScope"  

Pour plus d’informations, voir [Add-DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).  
  
### <a name="bkmk_records"></a>Ajouter des enregistrements pour les étendues de Zone  
Maintenant, vous devez ajouter les enregistrements représentant l’hôte du serveur web dans les étendues de deux zone.   
  
Par exemple, **USZoneScope** et **EuropeZoneScope**. Dans USZoneScope, vous pouvez ajouter l’enregistrement www.woodgrove.com avec l’adresse IP 192.0.0.1, qui se trouve dans un centre de données aux États-Unis; et dans EuropeZoneScope, vous pouvez ajouter le même enregistrement (www.woodgrove.com) avec l’adresse IP 141.1.0.1dans le centre de données européenne.   
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour ajouter des enregistrements pour les étendues de zone.  
  
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "USZoneScope  
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "EuropeZoneScope"  
  
  
  
Dans cet exemple, vous devez également utiliser les commandes Windows PowerShell suivantes pour ajouter des enregistrements dans l’étendue de la zone par défaut pour vous assurer que le reste du monde peuvent toujours de serveur web de l’accès le woodgrove.com à partir d’une des deux centres de données.  
    
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1"   
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1"
      
 
Le **ZonePortée** paramètre n’est pas inclus lorsque vous ajoutez un enregistrement dans l’étendue par défaut. Il s’agit de la même que l’ajout d’enregistrements à une zone DNS standard.  
  
Pour plus d’informations, voir [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).  
  
### <a name="bkmk_policies"></a>Créer les stratégies  
Après avoir créé les sous-réseaux, les partitions (étendues zone) et que vous avez ajouté des enregistrements, vous devez créer des stratégies qui se connectent les sous-réseaux et les partitions, afin que lorsqu’une requête provient d’une source dans un des sous-réseaux de client DNS, la réponse de la requête est retournée à partir de la portée appropriée de la zone. Aucune stratégie n’est requis pour le mappage de l’étendue de la zone par défaut.   
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer une stratégie DNS qui renvoie les sous-réseaux du Client DNS et les étendues de zone.   
  
      
    Add-DnsServerQueryResolutionPolicy -Name "USPolicy" -Action ALLOW -ClientSubnet "eq,USSubnet" -ZoneScope "USZoneScope,1" -ZoneName "woodgrove.com"  
      
    Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "EuropeZoneScope,1" -ZoneName "woodgrove.com"  
     
Pour plus d’informations, voir [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Désormais, le serveur DNS est configuré avec les stratégies DNS requis pour rediriger le trafic en fonction de géolocalisation.  
  
Lorsque le serveur DNS reçoit des requêtes de résolution de nom, le serveur DNS évalue les champs dans la requête DNS contre les stratégies DNS configurés. Si l’adresse IP source dans la demande de résolution de nom correspond à aucune des stratégies, l’étendue de la zone associée est utilisée pour répondre à la requête, et l’utilisateur est dirigé vers la ressource est géographiquement le plus proche pour.   
  
Vous pouvez créer des milliers de stratégies DNS en fonction de votre trafic en matière de gestion, et toutes les nouvelles stratégies sont appliquées dynamiquement, sans redémarrer le serveur DNS - dans les requêtes entrantes.  
  
  
