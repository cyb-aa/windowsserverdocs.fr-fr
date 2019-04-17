---
title: Utiliser une stratégie DNS pour les réponses DNS intelligentes basées sur l’heure du jour
description: Cette rubrique fait partie de la DNS stratégie scénario Guide pour Windows Server2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 161446ff-a072-4cc4-b339-00a04857ff3a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 46821daff4a0046bf78d7f56dc7c5deabcc437e4
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-intelligent-dns-responses-based-on-the-time-of-day"></a>Utiliser une stratégie DNS pour les réponses DNS intelligentes basées sur l’heure du jour

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour savoir comment distribuer le trafic des applications sur différentes instances dispersés géographiquement d’une application à l’aide de stratégies DNS qui sont basées sur l’heure du jour.  
  
Ce scénario est utile dans les situations où vous souhaitez diriger le trafic dans un fuseau horaire pour les serveurs d’applications de remplacement, tels que les serveurs Web, qui sont trouvent dans un autre fuseau horaire. Cela vous permet d’équilibrer le trafic entre les instances de l’application au cours de la pointe périodes lorsque vos serveurs principaux sont surchargées avec le trafic.   
  
### <a name="bkmk_example1"></a>Exemple de réponses DNS intelligentes basées sur l’heure du jour  
Voici un exemple de comment vous pouvez utiliser une stratégie DNS pour équilibrer le trafic d’application basée sur l’heure de la journée.  
  
Cet exemple utilise une société fictive, Contoso cadeau Services, qui offre des solutions donner en ligne dans le monde entier par le biais de son site Web, contosogiftservices.com.   
  
Le site Web contosogiftservices.com est hébergé dans deux centres de données, un à Seattle (Amérique du Nord) et un autre dans Dublin (Europe). Les serveurs DNS sont configurés pour l’envoi de réponses prenant en charge à l’aide d’une stratégie DNS de géolocalisation. Avec un pic dans la récente dans l’entreprise, contosogiftservices.com a un plus grand nombre de visiteurs tous les jours, et certains clients ont signalé des problèmes de disponibilité de service.  
  
Contoso cadeau Services effectue une analyse de site et découvre que tous les soirs entre 18 h 00 et 21 h 00, heure locale, est un pic dans le trafic vers les serveurs Web. Les serveurs Web ne peut pas mettre à l’échelle pour gérer l’augmentation du trafic à ces heures de pointe, entraînant un déni de service pour les clients. La surcharge de trafic les heures de pointe même se produit dans les deux centres de données européens et américains. Aux autres moments de la journée, les serveurs de gérer les volumes de trafic qui sont largement leur capacité maximale.  
  
Pour vous assurer que les clients contosogiftservices.com une expérience réactive depuis le site Web, Services de cadeau Contoso souhaite rediriger certains types de trafic Dublin vers les serveurs d’applications Seattle entre 18 h 00 et 21 h 00 à Dublin; et qu’ils souhaitent rediriger certains types de trafic Seattle vers les serveurs d’applications Dublin entre 18 h 00 et 21 h 00 à Seattle.  
  
L’illustration suivante décrit ce scénario.  
  
![Heure de l’exemple d’une stratégie DNS jour](../../media/DNS-Policy-Tod1/dns_policy_tod1.jpg)  
  
### <a name="bkmk_works1"></a>Comment Intelligent réponses DNS basées sur l’heure du jour fonctionne  
  
Lorsque le serveur DNS est configuré avec l’heure de la stratégie DNS jours, entre 18 h 00 et 21 h 00 à chaque emplacement géographique, le serveur DNS effectue les opérations suivantes.  
  
- Les quatre premières requêtes qu’il reçoit avec l’adresse IP du serveur Web dans le centre de données local des réponses.  
- La cinquième requête qu’il reçoit avec l’adresse IP du serveur Web dans le centre de données à distance des réponses.   
  
Ce comportement sur la stratégie décharge 20 pour cent de la charge du trafic du serveur Web local vers le serveur Web à distance, d’accélération de la charge sur le serveur d’application locale et améliorer les performances du site pour les clients.  
  
Pendant les heures creuses, les serveurs DNS effectuent gestion normal géo-nombre de sites en fonction du trafic. En outre, les clients DNS qui envoient des requêtes à partir des emplacements autres que l’Amérique du Nord ou en Europe, la charge du serveur DNS équilibre le trafic sur les centres de données Seattle et Dublin.  
  
Lorsque plusieurs stratégies DNS sont configurés dans DNS, ils sont un ensemble ordonné de règles, et qu’ils sont traités par le système DNS à partir de la priorité la plus élevée à la priorité la plus basse. DNS utilise la première stratégie qui correspond à ces conditions, notamment l’heure de la journée. Pour cette raison, les stratégies plus spécifiques doivent avoir une priorité plus élevée. Si vous créez des stratégies de la journée et leur donnez la priorité élevée dans la liste des stratégies, DNS traite et utilise ces stratégies tout d’abord si elles correspondent aux paramètres de la requête de client DNS et les critères définis dans la stratégie. Si elles ne correspondent pas, le DNS déplace vers le bas de la liste des stratégies pour traiter les stratégies par défaut, jusqu'à ce qu’il trouve une correspondance.  
  
Pour plus d’informations sur les types de stratégies et des critères, consultez [vue d’ensemble des stratégies DNS](../../dns/deploy/DNS-Policies-Overview.md).  
  
### <a name="bkmk_how1"></a>Comment configurer une stratégie DNS pour les réponses DNS intelligentes basées sur l’heure du jour  
Pour configurer une stratégie DNS pour les réponses heure du jour application l’équilibrage de charge en fonction de requêtes, vous devez effectuer les étapes suivantes.  
  
- [Créer les sous-réseaux du Client DNS](#bkmk_subnets)  
- [Créer les étendues de Zone](#bkmk_zscopes)  
- [Ajouter des enregistrements pour les étendues de Zone](#bkmk_records)  
- [Créer les stratégies DNS](#bkmk_policies)  
  
>[!NOTE]
>Vous devez effectuer ces étapes sur le serveur DNS faisant autorité pour la zone que vous souhaitez configurer. L’appartenance au groupe **DnsAdmins**, ou équivalent, est requis pour effectuer les procédures suivantes.  
  
Les sections suivantes fournissent des instructions de configuration détaillées.  
  
>[!IMPORTANT]
>Les sections suivantes incluent des exemples de commandes Windows PowerShell qui contiennent des exemples de valeurs pour un grand nombre de paramètres. Assurez-vous que vous remplacez les exemples de valeurs de ces commandes par les valeurs appropriées pour votre déploiement avant d’exécuter ces commandes.  
  
#### <a name="bkmk_subnets"></a>Créer les sous-réseaux du Client DNS  
La première étape consiste à identifier les sous-réseaux ou l’espace d’adressage IP des régions pour lequel vous souhaitez rediriger le trafic. Par exemple, si vous souhaitez rediriger le trafic pour les États-Unis et en Europe, vous devez identifier les sous-réseaux ou les espaces d’adressage IP de ces régions.  
  
Vous pouvez obtenir ces informations à partir de cartes géo-IP. En fonction de ces distributions géo-IP, vous devez créer les «sous-réseaux du Client DNS». Un sous-réseau de Client DNS est un regroupement logique des sous-réseaux IPv4 ou IPv6 à partir de laquelle les requêtes sont envoyées à un serveur DNS.  
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer des sous-réseaux de Client DNS.  
  
```  
Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet "192.0.0.0/24, 182.0.0.0/24"  
  
Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24, 151.1.0.0/24"  
  
```  
Pour plus d’informations, voir [Add-DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).  
  
#### <a name="bkmk_zscopes"></a>Créer les étendues de Zone  
Après avoir configuré les sous-réseaux du client, vous devez partitionner la zone dont le trafic que vous souhaitez rediriger les deux étendues zone différents, une étendue pour chacun de ces sous-réseaux de Client DNS que vous avez configuré.  
  
Par exemple, si vous souhaitez rediriger le trafic pour le nom DNS www.contosogiftservices.com, vous devez créer deux étendues autre zone dans la zone contosogiftservices.com, un pour les États-Unis et un pour l’Europe.  
  
Une étendue de la zone est une instance unique de la zone. Une zone DNS peut avoir plusieurs étendues de zone, avec chaque étendue de la zone qui contient son propre ensemble d’enregistrements DNS. L’enregistrement même peut être présent dans plusieurs étendues, avec des adresses IP différentes ou les mêmes adresses IP.  
  
>[!NOTE]
>Par défaut, une étendue de la zone existe sur les zones DNS. Cette étendue de la zone a le même nom que la zone, et les opérations de DNS héritées fonctionnent sur cette étendue.  
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer des étendues de zone.  
  
```  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"  
  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"  
  
```  
Pour plus d’informations, voir [Add-DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).  
  
#### <a name="bkmk_records"></a>Ajouter des enregistrements pour les étendues de Zone  
Maintenant, vous devez ajouter les enregistrements représentant l’hôte du serveur web dans les étendues de deux zone.  
  
Par exemple, dans **SeattleZoneScope**, l’enregistrement **www.contosogiftservices.com** est ajouté avec l’adresse IP 192.0.0.1, qui se trouve dans un centre de données de Seattle. De même, dans **DublinZoneScope**, l’enregistrement **www.contosogiftservices.com** est ajouté avec adresse IP 141.1.0.3dans le centre de données Dublin  
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour ajouter des enregistrements pour les étendues de zone.  
  
```  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope  
  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.3" -ZoneScope "DublinZoneScope"  
  
```  
Le paramètre ZonePortée n’est pas inclus lorsque vous ajoutez un enregistrement dans l’étendue par défaut. Il s’agit de la même que l’ajout d’enregistrements à une zone DNS standard.  
  
Pour plus d’informations, voir [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).  
  
#### <a name="bkmk_policies"></a>Créer les stratégies DNS  
Après avoir créé les sous-réseaux, les partitions (étendues zone) et que vous avez ajouté des enregistrements, vous devez créer des stratégies qui se connectent les sous-réseaux et les partitions, afin que lorsqu’une requête provient d’une source dans un des sous-réseaux de client DNS, la réponse de la requête est retournée à partir de la portée appropriée de la zone. Aucune stratégie n’est requis pour le mappage de l’étendue de la zone par défaut.  
  
Après avoir configuré ces stratégies DNS, le comportement de serveur DNS est le suivant:  
  
1. Les clients DNS européenne recevoir l’adresse IP du serveur Web du centre de données Dublin dans leur réponse à la requête DNS.  
2. Les clients DNS American recevoir l’adresse IP du serveur Web du centre de données Seattle dans leur réponse à la requête DNS.  
3. Entre 18 h 00 et 21 h 00 à Dublin, 20% des requêtes à partir de clients européennes recevoir l’adresse IP du serveur Web du centre de données Seattle dans leur réponse à la requête DNS.  
4. Entre 18 h 00 et 21 h 00 à Seattle, 20% des requêtes des clients American recevoir l’adresse IP du serveur Web du centre de données Dublin dans leur réponse à la requête DNS.  
5. La moitié des requêtes du reste du monde reçoivent l’adresse IP du centre de données Seattle et l’autre moitié recevoir l’adresse IP du centre de données Dublin.  
  
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer une stratégie DNS qui renvoie les sous-réseaux du Client DNS et les étendues de zone.  
  
>[!NOTE]
>Dans cet exemple, le serveur DNS est dans le fuseau horaire GMT, afin de l’heure de pointe des périodes de temps doivent être exprimées en heure GMT équivalente.  
  
```  
Add-DnsServerQueryResolutionPolicy -Name "America6To9Policy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,4;DublinZoneScope,1" -TimeOfDay "EQ,01:00-04:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 1  
  
Add-DnsServerQueryResolutionPolicy -Name "Europe6To9Policy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "SeattleZoneScope,1;DublinZoneScope,4" -TimeOfDay "EQ,17:00-20:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 2  
  
Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3  
  
Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 4  
  
Add-DnsServerQueryResolutionPolicy -Name "RestOfWorldPolicy" -Action ALLOW --ZoneScope "DublinZoneScope,1;SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 5  
  
```  
Pour plus d’informations, voir [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Désormais, le serveur DNS est configuré avec les stratégies DNS requis de rediriger le trafic basé sur la géolocalisation et l’heure de la journée.  
  
Lorsque le serveur DNS reçoit des requêtes de résolution de nom, le serveur DNS évalue les champs dans la requête DNS contre les stratégies DNS configurés. Si l’adresse IP source dans la demande de résolution de nom correspond à aucune des stratégies, l’étendue de la zone associée est utilisée pour répondre à la requête, et l’utilisateur est dirigé vers la ressource est géographiquement le plus proche pour.  
  
Vous pouvez créer des milliers de stratégies DNS en fonction de votre trafic en matière de gestion, et toutes les nouvelles stratégies sont appliquées dynamiquement, sans redémarrer le serveur DNS - dans les requêtes entrantes.


