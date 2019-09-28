---
title: Utiliser une stratégie DNS pour les réponses DNS intelligentes basées sur l’heure du jour
description: Cette rubrique fait partie du Guide de scénario de stratégie DNS pour Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: 161446ff-a072-4cc4-b339-00a04857ff3a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e497b0d73c816f0295588aa77a21c49d376c0dcf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406183"
---
# <a name="use-dns-policy-for-intelligent-dns-responses-based-on-the-time-of-day"></a>Utiliser une stratégie DNS pour les réponses DNS intelligentes basées sur l’heure du jour

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour savoir comment distribuer le trafic d’application sur différentes instances géographiquement distribuées d’une application à l’aide de stratégies DNS basées sur l’heure de la journée.  
  
Ce scénario est utile dans les situations où vous souhaitez diriger le trafic dans un fuseau horaire vers d’autres serveurs d’applications, tels que des serveurs Web, qui se trouvent dans un autre fuseau horaire. Cela vous permet d’équilibrer la charge du trafic entre les instances d’application pendant les périodes de pointe lorsque vos serveurs principaux sont surchargés avec le trafic.   
  
### <a name="bkmk_example1"></a>Exemple de réponses DNS intelligentes en fonction de l’heure de la journée  
Vous trouverez ci-dessous un exemple de la façon dont vous pouvez utiliser la stratégie DNS pour équilibrer le trafic d’application en fonction de l’heure de la journée.  
  
Cet exemple utilise une société fictive, contoso Gift services, qui fournit des solutions de don en ligne dans le monde entier via son site Web, contosogiftservices.com.   
  
Le site Web contosogiftservices.com est hébergé dans deux centres de centres, l’un à Seattle (Amérique du Nord) et l’autre à Dublin (Europe). Les serveurs DNS sont configurés pour envoyer des réponses sensibles à la géolocalisation à l’aide de la stratégie DNS. Avec une récente augmentation des activités, contosogiftservices.com a un plus grand nombre de visiteurs chaque jour et certains clients ont signalé des problèmes de disponibilité des services.  
  
Contoso Gift services effectue une analyse de site et découvre que chaque soir entre 18 h 00 et 9 h 00 heure locale, il y a une augmentation du trafic vers les serveurs Web. Les serveurs Web ne peuvent pas être mis à l’échelle pour gérer l’augmentation du trafic à ces heures de pointe, ce qui entraîne un déni de service pour les clients. La même surcharge de trafic des heures de pointe se produit à la fois dans les centres de. européens et américains. À d’autres moments de la journée, les serveurs gèrent les volumes de trafic qui sont bien inférieurs à leur capacité maximale.  
  
Pour vous assurer que les clients contosogiftservices.com bénéficient d’une expérience réactive à partir du site Web, contoso Gift services souhaite rediriger le trafic de Dublin vers les serveurs d’applications de Seattle entre 18 h 00 et 9 h 00 en Dublin ; et ils souhaitent rediriger le trafic de Seattle vers les serveurs d’applications de Dublin entre 18 h 00 et 21 h 00 à Seattle.  
  
L’illustration suivante représente ce scénario.  
  
![Exemple de stratégie DNS d’heure de la journée](../../media/DNS-Policy-Tod1/dns_policy_tod1.jpg)  
  
### <a name="bkmk_works1"></a>Comment fonctionnent les réponses DNS intelligentes en fonction de l’heure de la journée  
  
Lorsque le serveur DNS est configuré avec la stratégie DNS heure de la journée, entre 6 h 00 et 9 h 00 à chaque emplacement géographique, le serveur DNS effectue les opérations suivantes.  
  
- Répond aux quatre premières requêtes qu’il reçoit avec l’adresse IP du serveur Web dans le centre de. local.  
- Répond à la cinquième requête qu’il reçoit avec l’adresse IP du serveur Web dans le centre de l’accès à distance.   
  
Ce comportement basé sur la stratégie décharge vingt pour cent de la charge du trafic du serveur Web local sur le serveur Web distant, ce qui facilite la surcharge sur le serveur d’applications local et l’amélioration des performances du site pour les clients.  
  
Pendant les heures creuses, les serveurs DNS effectuent une gestion normale du trafic basé sur les emplacements géographiques. En outre, les clients DNS qui envoient des requêtes à partir d’emplacements autres que Amérique du Nord ou en Europe, le serveur DNS équilibre la charge du trafic entre les centres de. Seattle et Dublin.  
  
Lorsque plusieurs stratégies DNS sont configurées dans DNS, il s’agit d’un ensemble ordonné de règles qui sont traitées par le DNS, de la priorité la plus élevée à la priorité la plus basse. DNS utilise la première stratégie qui correspond aux circonstances, y compris l’heure de la journée. Pour cette raison, les stratégies plus spécifiques doivent avoir une priorité plus élevée. Si vous créez des stratégies d’heure et leur attribuez une priorité élevée dans la liste des stratégies, DNS traite et utilise d’abord ces stratégies si elles correspondent aux paramètres de la requête du client DNS et aux critères définis dans la stratégie. S’ils ne correspondent pas, le DNS descend dans la liste des stratégies pour traiter les stratégies par défaut jusqu’à ce qu’il trouve une correspondance.  
  
Pour plus d’informations sur les types de stratégie et les critères, consultez [vue d’ensemble des stratégies DNS](../../dns/deploy/DNS-Policies-Overview.md).  
  
### <a name="bkmk_how1"></a>Comment configurer la stratégie DNS pour les réponses DNS intelligentes en fonction de l’heure de la journée  
Pour configurer une stratégie DNS pour les réponses de requêtes basées sur l’équilibrage de charge des applications, vous devez effectuer les étapes suivantes.  
  
- [Créer les sous-réseaux du client DNS](#bkmk_subnets)  
- [Créer les étendues de zone](#bkmk_zscopes)  
- [Ajouter des enregistrements aux étendues de zone](#bkmk_records)  
- [Créer les stratégies DNS](#bkmk_policies)  
  
>[!NOTE]
>Vous devez effectuer ces étapes sur le serveur DNS qui fait autorité pour la zone que vous souhaitez configurer. L’appartenance à **DnsAdmins**, ou équivalent, est nécessaire pour effectuer les procédures suivantes.  
  
Les sections suivantes fournissent des instructions de configuration détaillées.  
  
>[!IMPORTANT]
>Les sections suivantes incluent des exemples de commandes Windows PowerShell qui contiennent des exemples de valeurs pour de nombreux paramètres. Veillez à remplacer les valeurs d’exemple dans ces commandes par des valeurs appropriées pour votre déploiement avant d’exécuter ces commandes.  
  
#### <a name="bkmk_subnets"></a>Créer les sous-réseaux du client DNS  
La première étape consiste à identifier les sous-réseaux ou l’espace d’adressage IP des régions pour lesquelles vous souhaitez rediriger le trafic. Par exemple, si vous souhaitez rediriger le trafic pour les États-Unis et l’Europe, vous devez identifier les sous-réseaux ou les espaces d’adressage IP de ces régions.  
  
Vous pouvez obtenir ces informations à partir de cartes géo-IP. Sur la base de ces distributions géo-IP, vous devez créer les « sous-réseaux du client DNS ». Un sous-réseau client DNS est un regroupement logique de sous-réseaux IPv4 ou IPv6 à partir desquels les requêtes sont envoyées à un serveur DNS.  
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer des sous-réseaux clients DNS.  
  
```  
Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet "192.0.0.0/24, 182.0.0.0/24"  
  
Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24, 151.1.0.0/24"  
  
```  
Pour plus d’informations, consultez [Add-DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).  
  
#### <a name="bkmk_zscopes"></a>Créer les étendues de zone  
Une fois les sous-réseaux clients configurés, vous devez partitionner la zone dont vous souhaitez rediriger le trafic vers deux étendues de zone différentes, une étendue pour chacun des sous-réseaux du client DNS que vous avez configurés.  
  
Par exemple, si vous souhaitez rediriger le trafic pour le nom DNS www.contosogiftservices.com, vous devez créer deux étendues de zone différentes dans la zone contosogiftservices.com, une pour les États-Unis et une pour l’Europe.  
  
Une étendue de zone est une instance unique de la zone. Une zone DNS peut avoir plusieurs étendues de zone, chaque étendue contenant son propre ensemble d’enregistrements DNS. Le même enregistrement peut être présent dans plusieurs étendues, avec des adresses IP différentes ou les mêmes adresses IP.  
  
>[!NOTE]
>Par défaut, il existe une étendue de zone sur les zones DNS. Cette étendue de zone porte le même nom que la zone, et les opérations DNS héritées fonctionnent sur cette étendue.  
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer des étendues de zone.  
  
```  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"  
  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"  
  
```  
Pour plus d’informations, consultez [Add-DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).  
  
#### <a name="bkmk_records"></a>Ajouter des enregistrements aux étendues de zone  
À présent, vous devez ajouter les enregistrements qui représentent l’hôte du serveur Web dans les deux étendues de zone.  
  
Par exemple, dans **SeattleZoneScope**, l’enregistrement <strong>www.contosogiftservices.com</strong> est ajouté avec l’adresse IP 192.0.0.1, qui se trouve dans un centre de contenu Seattle. De même, dans **DublinZoneScope**, l’enregistrement <strong>www.contosogiftservices.com</strong> est ajouté avec l’adresse IP 141.1.0.3 dans le centre de centres Dublin  
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour ajouter des enregistrements aux étendues de zone.  
  
```  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope  
  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.3" -ZoneScope "DublinZoneScope"  
  
```  
Le paramètre ZoneScope n’est pas inclus lorsque vous ajoutez un enregistrement dans l’étendue par défaut. Cela revient à ajouter des enregistrements à une zone DNS standard.  
  
Pour plus d’informations, consultez [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).  
  
#### <a name="bkmk_policies"></a>Créer les stratégies DNS  
Une fois que vous avez créé les sous-réseaux, les partitions (étendues de zone) et que vous avez ajouté des enregistrements, vous devez créer des stratégies qui connectent les sous-réseaux et les partitions, de sorte que lorsqu’une requête provient d’une source dans l’un des sous-réseaux du client DNS, la réponse à la requête est retournée à partir de étendue correcte de la zone. Aucune stratégie n’est requise pour le mappage de l’étendue de zone par défaut.  
  
Une fois ces stratégies DNS configurées, le comportement du serveur DNS est le suivant :  
  
1. Les clients DNS européens reçoivent l’adresse IP du serveur Web dans le centre de centres Dublin dans leur réponse à la requête DNS.  
2. Les clients américains DNS reçoivent l’adresse IP du serveur Web dans le centre de la Seattle dans leur réponse à la requête DNS.  
3. Entre 18 h 00 et 21 h en Dublin, 20% des requêtes des clients européens reçoivent l’adresse IP du serveur Web dans le centre de la Seattle dans leur réponse à la requête DNS.  
4. Entre le 18 h et le 9 h 00 à Seattle, 20% des requêtes des clients américains reçoivent l’adresse IP du serveur Web dans le centre de distribution Dublin dans leur réponse à la requête DNS.  
5. La moitié des requêtes du reste du monde reçoivent l’adresse IP du centre de la Seattle et l’autre moitié reçoit l’adresse IP du centre de centres Dublin.  
  
  
Vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer une stratégie DNS qui lie les sous-réseaux du client DNS et les étendues de zone.  
  
>[!NOTE]
>Dans cet exemple, le serveur DNS est dans le fuseau horaire GMT, donc les périodes de pointe doivent être exprimées dans l’heure GMT équivalente.  
  
```  
Add-DnsServerQueryResolutionPolicy -Name "America6To9Policy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,4;DublinZoneScope,1" -TimeOfDay "EQ,01:00-04:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 1  
  
Add-DnsServerQueryResolutionPolicy -Name "Europe6To9Policy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "SeattleZoneScope,1;DublinZoneScope,4" -TimeOfDay "EQ,17:00-20:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 2  
  
Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3  
  
Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 4  
  
Add-DnsServerQueryResolutionPolicy -Name "RestOfWorldPolicy" -Action ALLOW --ZoneScope "DublinZoneScope,1;SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 5  
  
```  
Pour plus d’informations, consultez [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
À présent, le serveur DNS est configuré avec les stratégies DNS requises pour rediriger le trafic en fonction de la géolocalisation et de l’heure de la journée.  
  
Lorsque le serveur DNS reçoit des requêtes de résolution de noms, le serveur DNS évalue les champs de la requête DNS par rapport aux stratégies DNS configurées. Si l’adresse IP source de la demande de résolution de noms correspond à l’une des stratégies, l’étendue de la zone associée est utilisée pour répondre à la requête, et l’utilisateur est dirigé vers la ressource qui est géographiquement la plus proche.  
  
Vous pouvez créer des milliers de stratégies DNS en fonction de vos besoins en matière de gestion du trafic, et toutes les nouvelles stratégies sont appliquées de manière dynamique, sans redémarrer le serveur DNS-sur les requêtes entrantes.


