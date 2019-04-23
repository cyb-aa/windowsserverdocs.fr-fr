---
title: Utiliser une stratégie DNS pour la gestion du trafic basée sur la géolocalisation avec des serveurs principaux
description: Cette rubrique fait partie de DNS stratégie scénario Guide pour Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: ef9828f8-c0ad-431d-ae52-e2065532e68f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 110014adf1e23be574f23efc01e8a4d69397e547
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831980"
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers"></a>Utiliser une stratégie DNS pour la gestion du trafic basée sur la géolocalisation avec des serveurs principaux

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour savoir comment configurer une stratégie DNS pour autoriser des serveurs DNS principaux répondre aux requêtes du client DNS basés sur l’emplacement géographique du client et la ressource à laquelle le client tente de se connecter, en fournissant le client avec l’ad IP droit de la ressource le plus proche.  
  
>[!IMPORTANT]  
>Ce scénario montre comment déployer une stratégie DNS pour la gestion du trafic en fonction de localisation géographique lorsque vous utilisez des serveurs DNS principales uniquement. Vous pouvez également effectuer la gestion du trafic en fonction de localisation géographique lorsque vous avez des serveurs DNS principales et secondaire. Si vous avez un déploiement de principaux / secondaires, commencer par suivre les étapes décrites dans cette rubrique, puis effectuez les étapes qui sont fournies dans la rubrique [utiliser une stratégie DNS pour l’emplacement géographique en fonction de la gestion du trafic avec des déploiements principaux / secondaires](primary-secondary-geo-location.md).

Avec les nouvelles stratégies DNS, vous pouvez créer une stratégie DNS qui permet au serveur DNS répondre à une requête client demande l’adresse IP d’un serveur Web. Les instances du serveur Web peuvent se trouver dans différents centres de données à des emplacements physiques différents. DNS, vous pouvez évaluer le client et les emplacements de serveur Web, puis répondre à la demande du client en fournissant le client avec un serveur Web adresse IP pour un serveur Web qui se trouve physiquement plus proche au client.  

Vous pouvez utiliser les paramètres de stratégie DNS suivants pour contrôler les réponses de serveur DNS aux requêtes des clients DNS. 

- **Sous-réseau du client**. Nom du sous-réseau client prédéfini. Permet de vérifier le sous-réseau à partir de laquelle la requête a été envoyée.
- **Protocole de transport**. Protocole utilisé dans la requête de transport. Les entrées possibles sont **UDP** et **TCP**.
- **Protocole Internet**. Protocole réseau utilisé dans la requête. Les entrées possibles sont **IPv4** et **IPv6**.
- **Adresse IP de l’Interface du serveur**. Adresse IP de l’interface réseau du serveur DNS qui a reçu la requête DNS.
- **FQDN**. Le domaine nom complet (FQDN) de l’enregistrement dans la requête, avec la possibilité d’utiliser un caractère générique.
- **Type de requête**. Type d’enregistrement en cours d’interrogation (A, SRV, TXT, etc.).
- **Heure de la journée**. Heure de que la requête est reçue. 
  
Vous pouvez combiner les critères suivants avec un opérateur logique (et/ou) pour formuler des expressions de stratégie. Lorsque ces expressions correspondent, les stratégies sont tenues d’effectuer une des actions suivantes.
 
- **Ignorer**. Le serveur DNS rejette la requête.          
- **Refuser**. Le serveur DNS répond à cette requête avec une réponse d’échec.          
- **Autoriser**. Le serveur DNS répond avec le trafic géré réponse.          
  
##  <a name="bkmk_example"></a>Emplacement géographique en fonction d’exemple de gestion de trafic

Voici un exemple de comment vous pouvez utiliser une stratégie DNS pour atteindre la redirection du trafic en fonction de l’emplacement physique du client qui effectue une requête DNS.   
  
Cet exemple utilise deux entreprises fictives - Contoso Cloud Services, qui offre web et domaine qui héberge des solutions ; et Woodgrove Food Services, qui fournit des services de distribution de produits alimentaires dans plusieurs villes du monde, et qui a un site Web nommé woodgrove.com.  
  
Services de Cloud de Contoso possède deux centres de données, un dans le fuseau horaire et l’autre en Europe. Le centre de données européen héberge un aliment portail pour woodgrove.com de classement.   
  
Pour vous assurer que les clients woodgrove.com une expérience réactive à partir de son site Web, Woodgrove souhaite que les clients européens dirigés vers le centre de données européen et American dirigé vers le centre de données des États-Unis. Les clients situés ailleurs dans le monde entier peuvent être dirigés vers un des centres de données.   
  
L’illustration suivante représente ce scénario.  
  
![Emplacement géographique en fonction d’exemple de gestion de trafic](../../media/DNS-Policy-Geo1/dns_policy_geo1.png)  
  
##  <a name="bkmk_works"></a>Comment les noms DNS des processus de résolution fonctionne  
  
Pendant le processus de résolution de nom, l’utilisateur tente de se connecter à www.woodgrove.com. Cela entraîne une demande de résolution de nom DNS est envoyée au serveur DNS qui est configuré dans les propriétés de connexion réseau sur l’ordinateur de l’utilisateur. En règle générale, ceci est le serveur DNS fourni par l’ISP local agissant comme un programme de résolution de mise en cache et est désigné comme le LDNS.   
  
Si le nom DNS n’est pas présent dans le cache local de LDNS, le serveur LDNS transfère la requête au serveur DNS faisant autorité pour woodgrove.com. Le serveur DNS faisant autorité répond avec l’enregistrement demandé (www.woodgrove.com) sur le serveur LDNS, qui à son tour met en cache l’enregistrement avant de les envoyer à l’ordinateur de l’utilisateur.  
  
Étant donné que les Services de Cloud de Contoso utilise des stratégies de serveur DNS, le serveur DNS faisant autorité qui héberge contoso.com est configuré pour retourner l’emplacement géographique en fonction des réponses le trafic géré. Cela entraîne la direction de Clients européens par le centre de données européen et la direction de Clients American au centre de données des États-Unis, comme illustré dans l’illustration.  
  
Dans ce scénario, le serveur DNS faisant autorité voit généralement la demande de résolution de nom en provenance du serveur LDNS, très rarement, à partir de l’ordinateur de l’utilisateur. Pour cette raison, l’adresse IP source dans la demande de résolution de nom tel que vu par le serveur DNS faisant autorité est celle du serveur LDNS et non pas de l’ordinateur de l’utilisateur. Toutefois, à l’aide de l’adresse IP du serveur LDNS lorsque vous configurez l’emplacement géographique en fonction requête réponses fournit une estimation de répartition de charge de l’emplacement géographique de l’utilisateur, étant donné que l’utilisateur interroge le serveur DNS de son fournisseur de services Internet local.  
  
>[!NOTE]  
>Les stratégies DNS utilisent l’adresse IP de l’expéditeur dans le paquet UDP/TCP qui contient la requête DNS. Si la requête atteint le serveur principal via plusieurs tronçons de programme de résolution/LDNS, la stratégie considère uniquement l’adresse IP du programme de résolution de la dernière à partir duquel le serveur DNS reçoit la requête.  
  
##  <a name="bkmk_config"></a>Comment configurer une stratégie DNS pour l’emplacement géographique en fonction des réponses de requêtes  
Pour configurer une stratégie DNS pour les réponses aux requêtes en fonction de localisation géographique, vous devez effectuer les étapes suivantes.  
  
1. [Créer les sous-réseaux du Client DNS](#bkmk_subnets)  
2. [Créer les étendues de la Zone](#bkmk_scopes)  
3. [Ajoutez des enregistrements dans les étendues de Zone](#bkmk_records)  
4. [Créer les stratégies](#bkmk_policies)  
  
>[!NOTE]  
>Vous devez effectuer ces étapes sur le serveur DNS faisant autorité pour la zone que vous souhaitez configurer. L’appartenance au **DnsAdmins**, ou équivalent, est requis pour effectuer les procédures suivantes.  
  
Les sections suivantes fournissent des instructions de configuration détaillées.  
  
>[!IMPORTANT]  
>Les sections suivantes incluent des exemples de commandes Windows PowerShell qui contiennent des exemples de valeurs de paramètres. Veillez à remplacer les exemples de valeurs dans ces commandes avec les valeurs appropriées pour votre déploiement avant d’exécuter ces commandes.  
  
### <a name="bkmk_subnets"></a>Créer les sous-réseaux du Client DNS  
  
La première étape consiste à identifier les sous-réseaux ou un espace d’adressage IP des régions pour lequel vous souhaitez rediriger le trafic. Par exemple, si vous souhaitez rediriger le trafic aux États-Unis et Europe, vous devez identifier les sous-réseaux ou les espaces d’adressage IP de ces régions.  
  
Vous pouvez obtenir ces informations à partir de mappages de géo-IP. En fonction de ces distributions géo-IP, vous devez créer les « sous-réseaux du Client DNS ». Un sous-réseau de Client DNS est un regroupement logique des sous-réseaux IPv4 ou IPv6 à partir de laquelle les requêtes sont envoyées à un serveur DNS.  
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer des sous-réseaux de Client DNS.  
  
      
    Add-DnsServerClientSubnet -Name "USSubnet" -IPv4Subnet "192.0.0.0/24"  
      
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24"  
      
  
Pour plus d’informations, consultez [Add-DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).  
  
### <a name="bkmk_scopes"></a>Créer des étendues de Zone  
Une fois que les sous-réseaux du client sont configurés, vous devez partitionner la zone dont le trafic que vous souhaitez rediriger dans deux étendues de zone différente, une étendue pour chacun de ces sous-réseaux de Client DNS que vous avez configuré.   
  
Par exemple, si vous souhaitez rediriger le trafic pour le www.woodgrove.com nom DNS, vous devez créer deux étendues de zone différente dans la zone woodgrove.com, un pour les États-Unis et l’autre pour l’Europe.  
  
Une étendue de la zone est une instance unique de la zone. Une zone DNS peut avoir plusieurs étendues de zone, avec chaque étendue de la zone contenant son propre ensemble d’enregistrements DNS. Le même enregistrement peut être présent dans plusieurs étendues, avec différentes adresses IP ou les mêmes adresses IP.  
  
>[!NOTE]  
>Par défaut, une étendue de la zone existe sur les zones DNS. Cette étendue de la zone a le même nom que la zone et des opérations DNS héritées travailler sur cette étendue.   
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer des étendues de zone.  
  
      
    Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "USZoneScope"  
      
    Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "EuropeZoneScope"  

Pour plus d’informations, consultez [Add-DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).  
  
### <a name="bkmk_records"></a>Ajoutez des enregistrements dans les étendues de Zone  
Maintenant, vous devez ajouter les enregistrements représentant l’hôte du serveur web dans les étendues de deux zones.   
  
Par exemple, **USZoneScope** et **EuropeZoneScope**. Dans USZoneScope, vous pouvez ajouter la www.woodgrove.com enregistrement avec l’adresse IP 192.0.0.1, qui se trouve dans un centre de données des États-Unis ; et dans EuropeZoneScope, vous pouvez ajouter le même enregistrement (www.woodgrove.com) avec l’adresse IP 141.1.0.1 dans le centre de données européen.   
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour ajouter des enregistrements pour les étendues de zone.  
  
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "USZoneScope  
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "EuropeZoneScope"  
  
  
  
Dans cet exemple, vous devez également utiliser les commandes Windows PowerShell suivantes pour ajouter des enregistrements dans l’étendue de la zone par défaut pour vous assurer que le reste du monde permettre toujours accéder à la woodgrove.com web à partir d’une des deux centres de données.  
    
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1"   
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1"
      
 
Le **ZonePortée** paramètre n’est pas inclus lorsque vous ajoutez un enregistrement dans l’étendue par défaut. Cela équivaut à ajouter des enregistrements à une zone DNS standard.  
  
Pour plus d’informations, consultez [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).  
  
### <a name="bkmk_policies"></a>Créer les stratégies  
Une fois que vous avez créé les sous-réseaux, les partitions (étendues de zone) et que vous avez ajouté des enregistrements, vous devez créer des stratégies qui connectent les sous-réseaux et les partitions, afin que lorsqu’une requête provient d’une source dans un des sous-réseaux de client DNS, la réponse de requête est retournée à partir de l’étendue correcte de la zone. Aucune stratégie n’est requis pour le mappage de l’étendue de la zone par défaut.   
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer une stratégie DNS qui lie les sous-réseaux du Client DNS et les étendues de zone.   
  
      
    Add-DnsServerQueryResolutionPolicy -Name "USPolicy" -Action ALLOW -ClientSubnet "eq,USSubnet" -ZoneScope "USZoneScope,1" -ZoneName "woodgrove.com"  
      
    Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "EuropeZoneScope,1" -ZoneName "woodgrove.com"  
     
Pour plus d’informations, consultez [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Désormais, le serveur DNS est configuré avec les stratégies DNS requises pour rediriger le trafic en fonction de l’emplacement géographique.  
  
Lorsque le serveur DNS reçoit des requêtes de résolution de nom, le serveur DNS évalue les champs dans la requête DNS sur les stratégies DNS configurés. Si l’adresse IP source dans la demande de résolution de nom correspond à aucune des stratégies, l’étendue de la zone associée est utilisée pour répondre à la requête et l’utilisateur est dirigé vers la ressource qui est géographiquement le plus proche les.   
  
Vous pouvez créer des milliers de stratégies DNS, en fonction de votre trafic en matière de gestion, et toutes les nouvelles stratégies sont appliquées dynamiquement, sans avoir à redémarrer le serveur DNS : sur les requêtes entrantes.  
  
  
