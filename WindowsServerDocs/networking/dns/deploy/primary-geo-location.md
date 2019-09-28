---
title: Utiliser une stratégie DNS pour la gestion du trafic basée sur la géolocalisation avec des serveurs principaux
description: Cette rubrique fait partie du Guide de scénario de stratégie DNS pour Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: ef9828f8-c0ad-431d-ae52-e2065532e68f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9c313b88e2502a99baf5962a1f2eb224d67a38dc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406175"
---
# <a name="use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers"></a>Utiliser une stratégie DNS pour la gestion du trafic basée sur la géolocalisation avec des serveurs principaux

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour apprendre à configurer la stratégie DNS afin d’autoriser les serveurs DNS principaux à répondre aux requêtes des clients DNS en fonction de l’emplacement géographique du client et de la ressource à laquelle le client tente de se connecter, en fournissant au client l’adresse IP ad. l’approche de la ressource la plus proche.  
  
>[!IMPORTANT]  
>Ce scénario montre comment déployer une stratégie DNS pour la gestion du trafic basée sur la géolocalisation lorsque vous utilisez uniquement des serveurs DNS principaux. Vous pouvez également effectuer une gestion du trafic basée sur la géolocalisation lorsque vous avez à la fois des serveurs DNS principaux et secondaires. Si vous avez un déploiement principal secondaire, commencez par effectuer les étapes de cette rubrique, puis effectuez les étapes fournies dans la rubrique [utiliser la stratégie DNS pour la gestion du trafic basée sur la géolocalisation avec des déploiements principaux secondaires](primary-secondary-geo-location.md).

Avec les nouvelles stratégies DNS, vous pouvez créer une stratégie DNS qui permet au serveur DNS de répondre à une requête client demandant l’adresse IP d’un serveur Web. Les instances du serveur Web peuvent se trouver dans des centres de centres différents à des emplacements physiques différents. DNS peut évaluer les emplacements du client et du serveur Web, puis répondre à la demande du client en fournissant au client une adresse IP de serveur Web pour un serveur Web situé physiquement plus près du client.  

Vous pouvez utiliser les paramètres de stratégie DNS suivants pour contrôler les réponses du serveur DNS aux requêtes des clients DNS. 

- **Sous-réseau client**. Nom d’un sous-réseau client prédéfini. Utilisé pour vérifier le sous-réseau à partir duquel la requête a été envoyée.
- **Protocole de transport**. Protocole de transport utilisé dans la requête. Les entrées possibles sont **UDP** et **TCP**.
- **Protocole Internet**. Protocole réseau utilisé dans la requête. Les entrées possibles sont **IPv4** et **IPv6**.
- **Adresse IP de l’interface serveur**. Adresse IP de l’interface réseau du serveur DNS qui a reçu la demande DNS.
- **NOM DE DOMAINE COMPLET**. Nom de domaine complet (FQDN) de l’enregistrement dans la requête, avec la possibilité d’utiliser un caractère générique.
- **Type de requête**. Type d’enregistrement interrogé (A, SRV, TXT, etc.).
- **Heure de la journée**. Heure à laquelle la requête est reçue. 
  
Vous pouvez combiner les critères suivants avec un opérateur logique (AND/OR) pour formuler des expressions de stratégie. Lorsque ces expressions correspondent, les stratégies sont supposées effectuer l’une des actions suivantes.
 
- **Ignorer**. Le serveur DNS supprime silencieusement la requête.          
- **Refuser**. Le serveur DNS répond à cette requête avec une réponse d’échec.          
- **Autoriser**. Le serveur DNS répond à la réponse gérée par le trafic.          
  
##  <a name="bkmk_example"></a>Exemple de gestion du trafic basé sur l’emplacement géographique

Vous trouverez ci-dessous un exemple de la façon dont vous pouvez utiliser la stratégie DNS pour obtenir une redirection du trafic sur la base de l’emplacement physique du client qui exécute une requête DNS.   
  
Cet exemple utilise deux sociétés fictives : services Cloud contoso, qui fournissent des solutions d’hébergement Web et de domaine. et les services de la Woodgrove Bank, qui fournissent des services de distribution de nourriture dans plusieurs villes du monde entier, et qui ont un site Web nommé woodgrove.com.  
  
Contoso cloud services a deux centres de centres, l’un aux États-Unis et l’autre en Europe. Le centre de centres européen héberge un portail d’ordonnancement des aliments pour woodgrove.com.   
  
Pour vous assurer que les clients woodgrove.com bénéficient d’une expérience réactive à partir de leur site Web, la Woodgrove Bank souhaite des clients européens dirigés vers le centre de donnée européen et les clients américains dirigés vers le centre de centres des États-Unis. Les clients situés ailleurs dans le monde peuvent être dirigés vers l’un de ces centres de donnée.   
  
L’illustration suivante représente ce scénario.  
  
![Exemple de gestion du trafic basé sur l’emplacement géographique](../../media/DNS-Policy-Geo1/dns_policy_geo1.png)  
  
##  <a name="bkmk_works"></a>Fonctionnement du processus de résolution de noms DNS  
  
Pendant le processus de résolution de noms, l’utilisateur tente de se connecter à www.woodgrove.com. Cela entraîne la demande de résolution de nom DNS envoyée au serveur DNS qui est configuré dans les propriétés de connexion réseau sur l’ordinateur de l’utilisateur. En règle générale, il s’agit du serveur DNS fourni par le fournisseur de services Internet local agissant comme un programme de résolution de mise en cache. il est appelé LDNS.   
  
Si le nom DNS n’est pas présent dans le cache local de LDNS, le serveur LDNS transfère la requête au serveur DNS faisant autorité pour woodgrove.com. Le serveur DNS faisant autorité répond avec l’enregistrement demandé (www.woodgrove.com) au serveur LDNS, qui à son tour met en cache l’enregistrement localement avant de l’envoyer à l’ordinateur de l’utilisateur.  
  
Étant donné que contoso cloud services utilise des stratégies de serveur DNS, le serveur DNS faisant autorité qui héberge contoso.com est configuré pour retourner des réponses gérées par le trafic basé sur la géolocalisation. Cela aboutit à la direction des clients européens vers le centre de donnés européen et à la direction des clients américains vers le centre de donnés des États-Unis, comme illustré dans l’illustration.  
  
Dans ce scénario, le serveur DNS faisant autorité voit généralement la demande de résolution de noms provenant du serveur LDNS et, très rarement, de l’ordinateur de l’utilisateur. Pour cette raison, l’adresse IP source dans la demande de résolution de noms, telle qu’elle est affichée par le serveur DNS faisant autorité, est celle du serveur LDNS et non celle de l’ordinateur de l’utilisateur. Toutefois, l’utilisation de l’adresse IP du serveur LDNS lorsque vous configurez les réponses de requête basée sur la géolocalisation fournit une estimation équitable de l’emplacement géographique de l’utilisateur, car l’utilisateur interroge le serveur DNS de son fournisseur de services Internet local.  
  
>[!NOTE]  
>Les stratégies DNS utilisent l’adresse IP de l’expéditeur dans le paquet UDP/TCP qui contient la requête DNS. Si la requête atteint le serveur principal par le biais de plusieurs sauts de programme de résolution/LDNS, la stratégie considère uniquement l’adresse IP du dernier programme de résolution à partir duquel le serveur DNS reçoit la requête.  
  
##  <a name="bkmk_config"></a>Comment configurer la stratégie DNS pour les réponses de requêtes basées sur l’emplacement géographique  
Pour configurer la stratégie DNS pour les réponses de requêtes basées sur l’emplacement géographique, vous devez effectuer les étapes suivantes.  
  
1. [Créer les sous-réseaux du client DNS](#bkmk_subnets)  
2. [Créer les étendues de la zone](#bkmk_scopes)  
3. [Ajouter des enregistrements aux étendues de zone](#bkmk_records)  
4. [Créer les stratégies](#bkmk_policies)  
  
>[!NOTE]  
>Vous devez effectuer ces étapes sur le serveur DNS qui fait autorité pour la zone que vous souhaitez configurer. L’appartenance à **DnsAdmins**, ou équivalent, est nécessaire pour effectuer les procédures suivantes.  
  
Les sections suivantes fournissent des instructions de configuration détaillées.  
  
>[!IMPORTANT]  
>Les sections suivantes incluent des exemples de commandes Windows PowerShell qui contiennent des exemples de valeurs pour de nombreux paramètres. Veillez à remplacer les valeurs d’exemple dans ces commandes par des valeurs appropriées pour votre déploiement avant d’exécuter ces commandes.  
  
### <a name="bkmk_subnets"></a>Créer les sous-réseaux du client DNS  
  
La première étape consiste à identifier les sous-réseaux ou l’espace d’adressage IP des régions pour lesquelles vous souhaitez rediriger le trafic. Par exemple, si vous souhaitez rediriger le trafic pour les États-Unis et l’Europe, vous devez identifier les sous-réseaux ou les espaces d’adressage IP de ces régions.  
  
Vous pouvez obtenir ces informations à partir de cartes géo-IP. Sur la base de ces distributions géo-IP, vous devez créer les « sous-réseaux du client DNS ». Un sous-réseau client DNS est un regroupement logique de sous-réseaux IPv4 ou IPv6 à partir desquels les requêtes sont envoyées à un serveur DNS.  
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer des sous-réseaux clients DNS.  
  
      
    Add-DnsServerClientSubnet -Name "USSubnet" -IPv4Subnet "192.0.0.0/24"  
      
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24"  
      
  
Pour plus d’informations, consultez [Add-DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).  
  
### <a name="bkmk_scopes"></a>Créer des étendues de zone  
Une fois les sous-réseaux clients configurés, vous devez partitionner la zone dont vous souhaitez rediriger le trafic vers deux étendues de zone différentes, une étendue pour chacun des sous-réseaux du client DNS que vous avez configurés.   
  
Par exemple, si vous souhaitez rediriger le trafic pour le nom DNS www.woodgrove.com, vous devez créer deux étendues de zone différentes dans la zone woodgrove.com, une pour les États-Unis et une pour l’Europe.  
  
Une étendue de zone est une instance unique de la zone. Une zone DNS peut avoir plusieurs étendues de zone, chaque étendue contenant son propre ensemble d’enregistrements DNS. Le même enregistrement peut être présent dans plusieurs étendues, avec des adresses IP différentes ou les mêmes adresses IP.  
  
>[!NOTE]  
>Par défaut, il existe une étendue de zone sur les zones DNS. Cette étendue de zone porte le même nom que la zone et les opérations DNS héritées fonctionnent sur cette étendue.   
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer des étendues de zone.  
  
      
    Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "USZoneScope"  
      
    Add-DnsServerZoneScope -ZoneName "woodgrove.com" -Name "EuropeZoneScope"  

Pour plus d’informations, consultez [Add-DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).  
  
### <a name="bkmk_records"></a>Ajouter des enregistrements aux étendues de zone  
À présent, vous devez ajouter les enregistrements qui représentent l’hôte du serveur Web dans les deux étendues de zone.   
  
Par exemple, **USZoneScope** et **EuropeZoneScope**. Dans USZoneScope, vous pouvez ajouter l’enregistrement www.woodgrove.com avec l’adresse IP 192.0.0.1, qui se trouve dans un centre de centres des États-Unis. et dans EuropeZoneScope, vous pouvez ajouter le même enregistrement (www.woodgrove.com) à l’adresse IP 141.1.0.1 dans le centre de centres européen.   
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour ajouter des enregistrements aux étendues de zone.  
  
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "USZoneScope  
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "EuropeZoneScope"  
  
  
  
Dans cet exemple, vous devez également utiliser les commandes Windows PowerShell suivantes pour ajouter des enregistrements dans l’étendue de zone par défaut afin de vous assurer que le reste du monde peut toujours accéder au serveur Web woodgrove.com à partir de l’un des deux centres de données.  
    
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "192.0.0.1"   
      
    Add-DnsServerResourceRecord -ZoneName "woodgrove.com" -A -Name "www" -IPv4Address "141.1.0.1"
      
 
Le paramètre **ZoneScope** n’est pas inclus lorsque vous ajoutez un enregistrement dans l’étendue par défaut. Cela revient à ajouter des enregistrements à une zone DNS standard.  
  
Pour plus d’informations, consultez [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).  
  
### <a name="bkmk_policies"></a>Créer les stratégies  
Une fois que vous avez créé les sous-réseaux, les partitions (étendues de zone) et que vous avez ajouté des enregistrements, vous devez créer des stratégies qui connectent les sous-réseaux et les partitions, de sorte que lorsqu’une requête provient d’une source dans l’un des sous-réseaux du client DNS, la réponse à la requête est retournée à partir de étendue correcte de la zone. Aucune stratégie n’est requise pour le mappage de l’étendue de zone par défaut.   
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer une stratégie DNS qui lie les sous-réseaux du client DNS et les étendues de zone.   
  
      
    Add-DnsServerQueryResolutionPolicy -Name "USPolicy" -Action ALLOW -ClientSubnet "eq,USSubnet" -ZoneScope "USZoneScope,1" -ZoneName "woodgrove.com"  
      
    Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "EuropeZoneScope,1" -ZoneName "woodgrove.com"  
     
Pour plus d’informations, consultez [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
À présent, le serveur DNS est configuré avec les stratégies DNS requises pour rediriger le trafic en fonction de la géolocalisation.  
  
Lorsque le serveur DNS reçoit des requêtes de résolution de noms, le serveur DNS évalue les champs de la requête DNS par rapport aux stratégies DNS configurées. Si l’adresse IP source de la demande de résolution de noms correspond à l’une des stratégies, l’étendue de la zone associée est utilisée pour répondre à la requête, et l’utilisateur est dirigé vers la ressource qui est géographiquement la plus proche.   
  
Vous pouvez créer des milliers de stratégies DNS en fonction de vos besoins en matière de gestion du trafic, et toutes les nouvelles stratégies sont appliquées de manière dynamique, sans redémarrer le serveur DNS-sur les requêtes entrantes.  
  
  
