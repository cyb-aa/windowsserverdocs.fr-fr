---
title: Utiliser une stratégie DNS pour les réponses DNS intelligentes basées sur l’heure du jour
description: Cette rubrique fait partie de DNS stratégie scénario Guide pour Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 161446ff-a072-4cc4-b339-00a04857ff3a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 33fd9447a79346127714a5e5e73977611eba483c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829470"
---
# <a name="use-dns-policy-for-intelligent-dns-responses-based-on-the-time-of-day"></a>Utiliser une stratégie DNS pour les réponses DNS intelligentes basées sur l’heure du jour

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour savoir comment distribuer le trafic d’application entre différentes instances distribuées d’une application à l’aide de stratégies DNS qui sont basés sur l’heure du jour.  
  
Ce scénario est utile dans les situations où vous souhaitez diriger le trafic dans un fuseau horaire vers les serveurs d’applications de remplacement, tels que les serveurs Web, qui sont trouvent dans un autre fuseau horaire. Cela vous permet de répartir le trafic entre les instances de l’application pendant le pic périodes lorsque vos serveurs principaux sont surchargées avec le trafic.   
  
### <a name="bkmk_example1"></a>Exemple de réponses DNS intelligentes basées sur l’heure du jour  
Voici un exemple de comment vous pouvez utiliser une stratégie DNS pour équilibrer le trafic d’application basée sur l’heure de la journée.  
  
Cet exemple utilise une société fictive, Contoso cadeau Services, qui offre des solutions de donation de tels en ligne dans le monde entier via son site Web, contosogiftservices.com.   
  
Le site Web de contosogiftservices.com est hébergé dans deux centres de données, celui de Seattle (Amérique du Nord) et l’autre à Dublin (Europe). Les serveurs DNS sont configurés pour l’envoi de géolocalisation prenant en charge les réponses à l’aide d’une stratégie DNS. Avec une hausse du nombre récent dans l’entreprise, contosogiftservices.com a un nombre plus élevé de visiteurs tous les jours, et certains clients ont signalé des problèmes de disponibilité de service.  
  
Contoso cadeau Services effectue une analyse de site et découvre que tous les soirs entre 18 h 00 et 21 h 00 heure locale, il existe une forte hausse dans le trafic vers les serveurs Web. Les serveurs Web ne peut pas mettre à l’échelle pour gérer l’augmentation du trafic sur ces heures de pointe, ce qui entraîne un déni de service aux clients. La surcharge de trafic même pic heure se produit dans les centres de données européennes et américaines. À d’autres moments de la journée, les serveurs gèrent les volumes de trafic sont bien en deçà de leur capacité maximale.  
  
Pour vous assurer que les clients contosogiftservices.com une expérience réactive à partir du site Web, Services cadeau de Contoso souhaite rediriger une partie du trafic Dublin vers les serveurs d’applications Seattle entre 18 h 00 et 21 h 00 à Dublin ; et ils souhaitent rediriger du trafic de Seattle vers les serveurs d’applications Dublin entre 18 h 00 et 21 h 00 à Seattle.  
  
L’illustration suivante représente ce scénario.  
  
![Heure de l’exemple de stratégie de DNS jours](../../media/DNS-Policy-Tod1/dns_policy_tod1.jpg)  
  
### <a name="bkmk_works1"></a>Comment Intelligent réponses DNS basées sur l’heure du jour Works  
  
Lorsque le serveur DNS est configuré avec un délai de jour de stratégie DNS, entre 18 h 00 et 21 h 00 à chaque emplacement géographique, le serveur DNS effectue les opérations suivantes.  
  
- Réponses les quatre premières requêtes qu’il reçoit par l’adresse IP du serveur Web du centre de données local.  
- Répond à la requête cinquième qu’il reçoit par l’adresse IP du serveur Web du centre de données à distance.   
  
Ce comportement en fonction de stratégies décharge vingt pour cent de la charge du trafic du serveur Web local au serveur Web distant, ce qui simplifie la surcharge sur le serveur d’applications locales et amélioration des performances de site pour les clients.  
  
Pendant les heures creuses, les serveurs DNS effectuer la gestion normale emplacements géographiques en fonction du trafic. En outre, les clients DNS qui envoient des requêtes à partir d’emplacements autres que l’Amérique du Nord ou Europe, la charge du serveur DNS équilibre le trafic entre les centres de données de Seattle et Dublin.  
  
Lorsque plusieurs stratégies DNS sont configurés dans DNS, ils sont un ensemble ordonné de règles, et ils sont traités par le système DNS à partir de la priorité la plus élevée à la priorité la plus basse. DNS utilise la première stratégie qui correspond aux circonstances, notamment l’heure de la journée. Pour cette raison, des stratégies plus spécifiques doivent avoir une priorité plus élevée. Si vous créez des temps de stratégies de la journée et que vous leur donnez la priorité élevée dans la liste des stratégies, DNS traite et utilise ces stratégies tout d’abord si elles correspondent aux paramètres de la requête de client DNS et des critères définis dans la stratégie. Si elles ne correspondent pas, le DNS descend dans la liste des stratégies pour traiter les stratégies par défaut jusqu'à ce qu’il trouve une correspondance.  
  
Pour plus d’informations sur les types de stratégie et de critères, consultez [vue d’ensemble des stratégies DNS](../../dns/deploy/DNS-Policies-Overview.md).  
  
### <a name="bkmk_how1"></a>Comment configurer une stratégie DNS pour les réponses DNS intelligentes basées sur l’heure du jour  
Pour configurer une stratégie DNS pour les réponses aux requêtes temporel jour application d’équilibrage de charge, vous devez effectuer les étapes suivantes.  
  
- [Créer les sous-réseaux du Client DNS](#bkmk_subnets)  
- [Créer des étendues de la Zone](#bkmk_zscopes)  
- [Ajoutez des enregistrements dans les étendues de Zone](#bkmk_records)  
- [Créer les stratégies DNS](#bkmk_policies)  
  
>[!NOTE]
>Vous devez effectuer ces étapes sur le serveur DNS faisant autorité pour la zone que vous souhaitez configurer. L’appartenance au **DnsAdmins**, ou équivalent, est requis pour effectuer les procédures suivantes.  
  
Les sections suivantes fournissent des instructions de configuration détaillées.  
  
>[!IMPORTANT]
>Les sections suivantes incluent des exemples de commandes Windows PowerShell qui contiennent des exemples de valeurs de paramètres. Veillez à remplacer les exemples de valeurs dans ces commandes avec les valeurs appropriées pour votre déploiement avant d’exécuter ces commandes.  
  
#### <a name="bkmk_subnets"></a>Créer les sous-réseaux du Client DNS  
La première étape consiste à identifier les sous-réseaux ou un espace d’adressage IP des régions pour lequel vous souhaitez rediriger le trafic. Par exemple, si vous souhaitez rediriger le trafic aux États-Unis et Europe, vous devez identifier les sous-réseaux ou les espaces d’adressage IP de ces régions.  
  
Vous pouvez obtenir ces informations à partir de mappages de géo-IP. En fonction de ces distributions géo-IP, vous devez créer les « sous-réseaux du Client DNS ». Un sous-réseau de Client DNS est un regroupement logique des sous-réseaux IPv4 ou IPv6 à partir de laquelle les requêtes sont envoyées à un serveur DNS.  
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer des sous-réseaux de Client DNS.  
  
```  
Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet "192.0.0.0/24, 182.0.0.0/24"  
  
Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24, 151.1.0.0/24"  
  
```  
Pour plus d’informations, consultez [Add-DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).  
  
#### <a name="bkmk_zscopes"></a>Créer des étendues de la Zone  
Une fois que les sous-réseaux du client sont configurés, vous devez partitionner la zone dont le trafic que vous souhaitez rediriger dans deux étendues de zone différente, une étendue pour chacun de ces sous-réseaux de Client DNS que vous avez configuré.  
  
Par exemple, si vous souhaitez rediriger le trafic pour le www.contosogiftservices.com nom DNS, vous devez créer deux étendues de zone différente dans la zone contosogiftservices.com, un pour les États-Unis et l’autre pour l’Europe.  
  
Une étendue de la zone est une instance unique de la zone. Une zone DNS peut avoir plusieurs étendues de zone, avec chaque étendue de la zone contenant son propre ensemble d’enregistrements DNS. Le même enregistrement peut être présent dans plusieurs étendues, avec différentes adresses IP ou les mêmes adresses IP.  
  
>[!NOTE]
>Par défaut, une étendue de la zone existe sur les zones DNS. Cette étendue de la zone a le même nom que la zone, et des opérations DNS héritées travailler sur cette étendue.  
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer des étendues de zone.  
  
```  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"  
  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"  
  
```  
Pour plus d’informations, consultez [Add-DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).  
  
#### <a name="bkmk_records"></a>Ajoutez des enregistrements dans les étendues de Zone  
Maintenant, vous devez ajouter les enregistrements représentant l’hôte du serveur web dans les étendues de deux zones.  
  
Par exemple, dans **SeattleZoneScope**, l’enregistrement **www.contosogiftservices.com** est ajouté avec l’adresse IP 192.0.0.1, qui se trouve dans un centre de données de Seattle. De même, dans **DublinZoneScope**, l’enregistrement **www.contosogiftservices.com** est ajouté avec l’adresse IP 141.1.0.3 dans le centre de données Dublin  
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour ajouter des enregistrements pour les étendues de zone.  
  
```  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope  
  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.3" -ZoneScope "DublinZoneScope"  
  
```  
Le paramètre ZonePortée n’est pas inclus lorsque vous ajoutez un enregistrement dans l’étendue par défaut. Cela équivaut à ajouter des enregistrements à une zone DNS standard.  
  
Pour plus d’informations, consultez [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).  
  
#### <a name="bkmk_policies"></a>Créer les stratégies DNS  
Une fois que vous avez créé les sous-réseaux, les partitions (étendues de zone) et que vous avez ajouté des enregistrements, vous devez créer des stratégies qui connectent les sous-réseaux et les partitions, afin que lorsqu’une requête provient d’une source dans un des sous-réseaux de client DNS, la réponse de requête est retournée à partir de l’étendue correcte de la zone. Aucune stratégie n’est requis pour le mappage de l’étendue de la zone par défaut.  
  
Après avoir configuré ces stratégies DNS, le comportement du serveur DNS est la suivante :  
  
1. Les clients DNS européen recevoir l’adresse IP du serveur Web du centre de données Dublin dans leur réponse à la requête DNS.  
2. Les clients DNS American recevoir l’adresse IP du serveur Web du centre de données Seattle dans leur réponse à la requête DNS.  
3. Entre 18 h 00 et 21 h 00 à Dublin, 20 % des requêtes à partir de clients européens recevoir l’adresse IP du serveur Web du centre de données Seattle dans leur réponse à la requête DNS.  
4. Entre 18 h 00 et 21 h 00 à Seattle, 20 % des requêtes à partir des clients American recevoir l’adresse IP du serveur Web du centre de données Dublin dans leur réponse à la requête DNS.  
5. La moitié des requêtes du reste du monde recevoir l’adresse IP du centre de données de Seattle et l’autre moitié recevoir l’adresse IP du centre de données Dublin.  
  
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer une stratégie DNS qui lie les sous-réseaux du Client DNS et les étendues de zone.  
  
>[!NOTE]
>Dans cet exemple, le serveur DNS étant dans le fuseau horaire GMT, l’heure de pointe des périodes de temps doivent être exprimées dans l’heure GMT équivalente.  
  
```  
Add-DnsServerQueryResolutionPolicy -Name "America6To9Policy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,4;DublinZoneScope,1" -TimeOfDay "EQ,01:00-04:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 1  
  
Add-DnsServerQueryResolutionPolicy -Name "Europe6To9Policy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "SeattleZoneScope,1;DublinZoneScope,4" -TimeOfDay "EQ,17:00-20:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 2  
  
Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3  
  
Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 4  
  
Add-DnsServerQueryResolutionPolicy -Name "RestOfWorldPolicy" -Action ALLOW --ZoneScope "DublinZoneScope,1;SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 5  
  
```  
Pour plus d’informations, consultez [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Désormais, le serveur DNS est configuré avec les stratégies DNS requises pour rediriger le trafic en fonction de l’emplacement géographique et l’heure de la journée.  
  
Lorsque le serveur DNS reçoit des requêtes de résolution de nom, le serveur DNS évalue les champs dans la requête DNS sur les stratégies DNS configurés. Si l’adresse IP source dans la demande de résolution de nom correspond à aucune des stratégies, l’étendue de la zone associée est utilisée pour répondre à la requête et l’utilisateur est dirigé vers la ressource qui est géographiquement le plus proche les.  
  
Vous pouvez créer des milliers de stratégies DNS, en fonction de votre trafic en matière de gestion, et toutes les nouvelles stratégies sont appliquées dynamiquement, sans avoir à redémarrer le serveur DNS : sur les requêtes entrantes.


