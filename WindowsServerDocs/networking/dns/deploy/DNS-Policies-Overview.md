---
title: Vue d’ensemble des stratégies DNS
description: Cette rubrique fait partie de la DNS stratégie scénario Guide pour Windows Server2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 566bc270-81c7-48c3-a904-3cba942ad463
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 06086bbd7edc2fa489805eb5075062332e002ab4
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="dns-policies-overview"></a>Vue d’ensemble des stratégies DNS

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur la stratégie de DNS, qui est une nouveauté de Windows Server2016. Vous pouvez utiliser une stratégie DNS pour la gestion du trafic basée géolocalisation, des réponses DNS intelligentes basées sur l’heure du jour, pour gérer un seul serveur DNS configuré pour le déploiement split\-brain, application de filtres sur les requêtes DNS et bien plus encore. Les éléments suivants fournissent plus de détails sur ces fonctionnalités.

-   **Application l’équilibrage de charge.** Lorsque vous avez déployé plusieurs instances d’une application à différents emplacements, vous pouvez utiliser une stratégie DNS pour équilibrer la charge du trafic entre les instances d’application différents, attribution dynamique de la charge du trafic de l’application.

-   **Emplacement de Geo\ en fonction de gestion du trafic.** Vous pouvez utiliser une stratégie DNS pour permettre à des serveurs DNS principaux et secondaires répondre aux requêtes de client DNS basées sur l’emplacement géographique du client et la ressource à laquelle le client tente de se connecter, en fournissant le client avec l’adresse IP de la ressource la plus proche. 

-   **Fractionnement cerveau DNS.** Avec split\-brain DNS, les enregistrements DNS sont réparties en différentes étendues de Zone sur le même serveur DNS, et les clients DNS recevoir une réponse si les clients sont des clients internes ou externes. Vous pouvez configurer split\-brain DNS pour les zones intégrées à Active Directory ou pour les zones sur des serveurs DNS autonomes.

-   **Le filtrage.** Vous pouvez configurer une stratégie DNS pour créer des filtres de requête qui sont basées sur des critères que vous fournissez. Filtres de requête dans une stratégie DNS vous autorise à configurer le serveur DNS pour répondre de manière personnalisée en fonction de la requête DNS et client DNS qui envoie la requête DNS. 
-   **Légales.** Vous pouvez utiliser une stratégie DNS pour rediriger les clients DNS malveillants à une adresse IP d’inexistantes autre que celle au lieu de les dirigeant vers l’ordinateur qu’il tente d’atteindre.

-   **Heure de la journée en fonction de redirection.** Vous pouvez utiliser une stratégie DNS pour distribuer le trafic des applications sur différentes instances dispersés géographiquement d’une application à l’aide de stratégies DNS qui sont basées sur l’heure du jour.

## <a name="new-concepts"></a>Nouveaux Concepts  
Pour créer des stratégies pour prendre en charge les scénarios répertoriés ci-dessus, il est nécessaire être en mesure d’identifier les groupes d’enregistrements dans une zone, les groupes de clients sur un réseau, entre autres éléments. Ces éléments sont représentés par les nouveaux objets DNS suivants:  

- **Sous-réseau client:** un objet de sous-réseau client représente un sous-réseau IPv4 ou IPv6 à partir de laquelle les requêtes sont envoyées à un serveur DNS. Vous pouvez créer des sous-réseaux pour définir des stratégies à appliquer sur quel sous-réseau les demandes proviennent de la base d’ultérieurement. Par exemple, dans un scénario DNS split brain, la demande pour la résolution pour un nom tel que *www.microsoft.com* peut répondre avec une adresse IP interne pour les clients à partir des sous-réseaux internes et une adresse IP différente pour les clients de sous-réseaux externes.

- **Étendue de la récursivité:** étendues récursivité sont des instances uniques d’un groupe de paramètres qui contrôlent la récursivité sur un serveur DNS. Une étendue de la récursivité contient une liste des redirecteurs et indique si la récursivité est activée. Un serveur DNS peut avoir plusieurs étendues de la récursivité. Les stratégies de récurrence de serveur DNS permettent de choisir une étendue de la récursivité pour un ensemble de requêtes. Si le serveur DNS ne fait pas autorité pour certaines requêtes, les stratégies de récurrence de serveur DNS permettent vous permettent de contrôler la façon de résoudre ces requêtes. Vous pouvez spécifier les redirecteurs à utiliser et si vous souhaitez utiliser la récursivité.

- **Les étendues de la zone:** une zone DNS peut avoir plusieurs étendues de zone, avec chaque étendue de la zone contenant leur propre ensemble d’enregistrements DNS. L’enregistrement même peut être présent dans plusieurs étendues, avec des adresses IP différentes. En outre, les transferts de zones sont effectuées au niveau de l’étendue de la zone. Cela signifie que les enregistrements à partir d’une étendue de la zone dans une zone principale seront transférées à l’étendue de la même zone dans une zone secondaire.

## <a name="types-of-policy"></a>Types de stratégie

Stratégies DNS sont séparés par niveau et le type. Vous pouvez utiliser des stratégies de résolution de requête pour définir le mode de traitement des requêtes et des stratégies de transfert de Zone pour définir comment les transferts de zones se produisent. Vous pouvez appliquer chaque type de stratégie au niveau du serveur ou au niveau de zone.
  
### <a name="query-resolution-policies"></a>Stratégies de résolution de requête

Vous pouvez utiliser des stratégies de résolution de requête DNS pour spécifier la résolution entrante de requêtes sont gérées par un serveur DNS. Chaque stratégie de résolution de requête DNS contient les éléments suivants:  
  
|Champ|Description|Valeurs possibles|  
|---------|---------------|-------------------|  
|**Nom**|Nom de la stratégie|-Jusqu'à 256caractères<br />-Peut contenir n’importe quel caractère valide pour un nom de fichier|  
|**État**|État de la stratégie|-Enable (par défaut)<br />-Désactivé|  
|**Niveau**|Niveau de stratégie|-Serveur<br />-Zone|  
|**Ordre de traitement**|Une fois une requête est classée par niveau et s’applique, le serveur de recherche la première stratégie correspond aux critères d’et l’applique à interroger pour lequel la requête|-Valeur numérique<br />-Valeur unique par stratégie contenant le même niveau et l’applique à la valeur|  
|**Action**|Action à effectuer par le serveur DNS|-Autoriser (par défaut pour le niveau de zone)<br />-Refuser (par défaut au niveau serveur)<br />-Ignorer|  
|**Critères**|Condition de stratégie (et/ou) et la liste des critères à respecter pour appliquer la stratégie|-Opérateur de condition (et/ou)<br />-Liste des critères (voir le tableau de critère ci-dessous)|  
|**Étendue**|Liste des étendues de zone et les valeurs pondérées chaque étendue. Valeurs pondérées sont utilisés pour la distribution d’équilibrage de charge. Par exemple, si cette liste inclut datacenter1 avec un poids de 3 et datacenter2 avec un poids de 5 le serveur répond avec un enregistrement des datacentre1 trois fois hors des huit demandes|-Liste des étendues de zone (par nom) et des poids|  

> [!NOTE]
> Stratégies au niveau du serveur peuvent afficher uniquement les valeurs **refuser** ou **ignorer** en tant qu’action.

Le champ de critères de stratégie DNS est composé de deux éléments:

|Nom|Description|Exemples de valeurs|
|--------|---------------|-----------------|
|**Sous-réseau client**|Protocole utilisé dans la requête de transport. Entrées possibles sont **UDP** et **TCP**|-   **EQ, Espagne, France** -résout sur «True» si le sous-réseau est identifié comme Espagne ou France<br />-   **Because, Canada, Mexique** -correspond à «True» si le sous-réseau de client est un sous-réseau autre que le Canada et Mexique|  
|**Protocole de transport**|Protocole utilisé dans la requête de transport. Entrées possibles sont **UDP** et **TCP**|-   **EQ, TCP**<br />-   **EQ, UDP**|  
|**Protocole Internet**|Protocole réseau utilisé dans la requête. Entrées possibles sont **IPv4** et **IPv6**|-   **EQ, IPv4**<br />-   **EQ, IPv6**|  
|**Adresse IP de l’Interface du serveur**|Adresse IP de l’interface réseau du serveur DNS entrante|-   **EQ, 10.0.0.1**<br />-   **EQ, 192.168.1.1**|  
|**NOM DE DOMAINE COMPLET**|Nom de domaine complet de l’enregistrement de la requête, avec la possibilité d’utiliser un caractère générique|-   **EQ, www.contoso.com** -résout assurances rue if uniquement la requête tente de résoudre le *www.contoso.com* nom de domaine complet<br />-   **EQ,\*.contoso.com,\*.woodgrove.com** -correspond à «True» si la requête est pour tous les enregistrements se terminant par *contoso.com***ou***woodgrove.com*|  
|**Type de requête**|Type d’enregistrement interrogé (A, SVR, TXT)|-   **SRV EQ, TXT,** -résout assurances rue si la requête demande un TXT **ou** enregistrement SRV<br />-   **EQ, MX** -résout assurances rue si la requête demande un enregistrement MX|  
|**Heure du jour**|Heure de que la réception de la requête|-   **EQ, 10h à 12h, 22h à 23h** -résout assurances rue si la requête est reçue entre 10 h et à midi, **ou** entre 22 h 00 et 23 h 00|  
  
Comme point de départ, vous utilisez le tableau ci-dessus, le tableau ci-dessous peut être utilisé pour définir un critère qui est utilisé pour faire correspondre des requêtes pour n’importe quel type d’enregistrements, mais les enregistrements SRV dans le domaine contoso.com provenant d’un client dans le sous-réseau 10.0.0.0/24 via TCP entre 8 et 22 h 00 par le biais de l’interface 10.0.0.3:  
  
|Nom|Valeur|  
|--------|---------|  
|Sous-réseau client|EQ, 10.0.0.0/24|  
|Protocole de transport|EQ, TCP|  
|Adresse IP de l’Interface du serveur|EQ, 10.0.0.3|  
|NOM DE DOMAINE COMPLET|EQ, *. contoso.com|  
|Type de requête|BECAUSE, SRV|  
|Heure du jour|EQ, 20h à 22h|  
  
Vous pouvez créer plusieurs requêtes des stratégies de résolution du même niveau, dans la mesure où ils ont une valeur différente de l’ordre de traitement. Lorsque plusieurs stratégies sont disponibles, le serveur DNS traite les requêtes entrantes de la manière suivante:  
  
![Traitement de la stratégie DNS](../../media/DNS-Policies-Overview/DNSQueryResolutionPolicyFlowchart.png)  
  
### <a name="recursion-policies"></a>Stratégies de récurrence  
Stratégies de récurrence sont un spécial **type** de stratégies au niveau du serveur. Stratégies de récurrence de contrôlent comment le serveur DNS exécute la récursivité pour une requête. Stratégies de récurrence s’appliquent uniquement lorsque le traitement d’une requête atteint le chemin d’accès de la récursivité. Vous pouvez choisir d’ignorer ou de refuser une valeur pour la récursivité pour un ensemble de requêtes. Sinon, vous pouvez choisir un ensemble de redirecteurs pour un ensemble de requêtes.  
  
Vous pouvez utiliser des stratégies de récurrence pour implémenter un Split-brain configuration DNS. Dans cette configuration, le serveur DNS exécute la récursivité pour un ensemble de clients pour une requête, tandis que le serveur DNS n’effectue pas la récursivité pour d’autres clients de cette requête.  
  
Stratégies de récurrence contient les éléments mêmes une stratégie de résolution de requête DNS régulière contient, ainsi que les éléments dans le tableau ci-dessous:  
  
|Nom|Description|  
|--------|---------------|  
|**Appliquer la récursivité**|Spécifie que cette stratégie doit uniquement être utilisée pour la récursivité.|  
|**Étendue de la récursivité**|Nom de l’étendue de la récursivité.|  
  
> [!NOTE]  
> La récursivité stratégies peuvent uniquement être créées au niveau du serveur.  
  
### <a name="zone-transfer-policies"></a>Stratégies de transfert de zone  
Stratégies de transfert de zone contrôlent si un transfert de zone est autorisé ou non par votre serveur DNS. Vous pouvez créer des stratégies pour le transfert de zone au niveau du serveur ou de niveau de la zone. Appliquent des stratégies au niveau du serveur sur chaque requête de transfert de zone qui se produit sur le serveur DNS. Stratégies de niveau zone s’appliquent uniquement sur les requêtes sur une zone hébergée sur le serveur DNS. L’utilisation la plus courante pour les stratégies de niveau zone consiste à implémenter bloqués ou listes.  
  
> [!NOTE]  
> Stratégies de transfert de zone peuvent uniquement utiliser refuser ou ignorer en tant qu’actions.  
  
Vous pouvez utiliser la stratégie de transfert de zone au niveau serveur ci-dessous pour refuser un transfert de zone pour le domaine contoso.com à partir d’un sous-réseau donné:  
  
```  
Add-DnsServerZoneTransferPolicy -Name DenyTransferOfCOnsotostoFabrikam -Zone contoso.com -Action DENY -ClientSubnet "EQ,192.168.1.0/24"  
```  
  
Vous pouvez créer plusieurs transfert de zone stratégies du même niveau, dans la mesure où ils ont une valeur différente de l’ordre de traitement. Lorsque plusieurs stratégies sont disponibles, le serveur DNS traite les requêtes entrantes de la manière suivante:  
  
![Processus de DNS pour plusieurs stratégies de transfert de zone](../../media/DNS-Policies-Overview/DNSPolicyZone.png)  
  
## <a name="managing-dns-policies"></a>Gestion des stratégies DNS  
Vous pouvez créer et gérer les stratégies de DNS à l’aide de PowerShell. Les exemples ci-dessous passer par différents exemples de scénarios que vous pouvez configurer par le biais de stratégies DNS:  
  
### <a name="traffic-management"></a>Gestion du trafic  
Vous pouvez diriger le trafic basé sur un nom de domaine complet sur différents serveurs selon l’emplacement du client DNS. L’exemple ci-dessous montre comment créer des stratégies de gestion pour diriger les clients à partir d’un certain sous-réseau à un centre de données Amérique du Nord et d’un autre sous-réseau à un centre de données européen le trafic.  
  
```  
Add-DnsServerClientSubnet -Name "NorthAmericaSubnet" -IPv4Subnet "172.21.33.0/24"  
Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "172.17.44.0/24"  
Add-DnsServerZoneScope -ZoneName "Contoso.com" -Name "NorthAmericaZoneScope"  
Add-DnsServerZoneScope -ZoneName "Contoso.com" -Name "EuropeZoneScope"  
Add-DnsServerResourceRecord -ZoneName "Contoso.com" -A -Name "www" -IPv4Address "172.17.97.97" -ZoneScope "EuropeZoneScope"  
Add-DnsServerResourceRecord -ZoneName "Contoso.com" -A -Name "www" -IPv4Address "172.21.21.21" -ZoneScope "NorthAmericaZoneScope"  
Add-DnsServerQueryResolutionPolicy -Name "NorthAmericaPolicy" -Action ALLOW -ClientSubnet "eq,NorthAmericaSubnet" -ZoneScope "NorthAmericaZoneScope,1" -ZoneName "Contoso.com"  
Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "EuropeZoneScope,1" -ZoneName contoso.com  
```  
  
Les deux premières lignes du script de créent des objets de sous-réseaux pour l’Amérique du Nord et en Europe client. Les deux lignes ensuite créent une étendue de zone dans le domaine contoso.com, une pour chaque région. Les deux lignes ensuite créent un enregistrement dans chaque zone qui associe ww.contoso.com à l’adresse IP différente, un pour l’Europe, un autre pour l’Amérique du Nord. Enfin, les dernières lignes du script créent deux stratégies de résolution de requête de DNS, une à appliquer au sous-réseau de l’Amérique du Nord, un autre pour le sous-réseau Europe.  
  
### <a name="block-queries-for-a-domain"></a>Bloquer les requêtes pour un domaine  
Vous pouvez utiliser une stratégie de résolution de requête DNS aux requêtes de bloc à un domaine. L’exemple ci-dessous bloque toutes les requêtes treyresearch.net:  
  
```  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicy" -Action IGNORE -FQDN "EQ,*.treyresearch.com"  
```  
  
### <a name="block-queries-from-a-subnet"></a>Requêtes de bloc à partir d’un sous-réseau  
Vous pouvez également bloquer les requêtes provenant d’un sous-réseau spécifique. Le script ci-dessous crée un sous-réseau pour 172.0.33.0/24, puis crée une stratégie pour ignorer toutes les requêtes en provenance de ce sous-réseau:  
  
```  
Add-DnsServerClientSubnet -Name "MaliciousSubnet06" -IPv4Subnet 172.0.33.0/24  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06"  
```  
  
### <a name="allow-recursion-for-internal-clients"></a>Autoriser la récursivité pour les clients internes  
Vous pouvez contrôler la récursivité à l’aide d’une stratégie de résolution de requête DNS. L’exemple ci-dessous peut servir à activer la récursivité pour les clients internes, lors de la désactivation pour les clients externes dans un scénario de cerveau fractionné.  
  
```  
Set-DnsServerRecursionScope -Name . -EnableRecursion $False   
Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True  
Add-DnsServerQueryResolutionPolicy -Name "SplitBrainPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP  "EQ,10.0.0.34"  
```  
  
La première ligne du script modifie l’étendue de la récursivité par défaut, appelé simplement en tant que «.» (point) pour désactiver la récursivité. La deuxième ligne crée une étendue de la récursivité nommée *InternalClients* avec la récursivité activée. La troisième ligne crée une stratégie à appliquer la qui vient d’être créer une étendue la récursivité pour toutes les requêtes entrantes par le biais d’une interface de serveur qui a 10.0.0.34 une adresse IP.  
  
### <a name="create-a-server-level-zone-transfer-policy"></a>Créer une stratégie de transfert de zone au niveau serveur  
Vous pouvez contrôler le transfert de zone dans un formulaire plus granulaire à l’aide de stratégies de transfert de Zone DNS. L’exemple de script ci-dessous peut être utilisée pour autoriser les transferts de zone pour n’importe quel serveur sur un sous-réseau donné:  
  
```  
Add-DnsServerClientSubnet -Name "AllowedSubnet" -IPv4Subnet 172.21.33.0/24  
Add-DnsServerZoneTransferPolicy -Name "NorthAmericaPolicy" -Action IGNORE -ClientSubnet "ne,AllowedSubnet"  
```  
  
La première ligne du script crée un objet de sous-réseau nommé *AllowedSubnet* avec l’adresse IP bloquer 172.21.33.0/24. La deuxième ligne crée une stratégie de transfert de zone pour autoriser les transferts de zone vers n’importe quel serveur DNS sur le sous-réseau créé précédemment.  
  
### <a name="create-a-zone-level-zone-transfer-policy"></a>Créer une stratégie de transfert de zone au niveau de zone  
Vous pouvez également créer des stratégies de transfert de zone au niveau de zone. L’exemple ci-dessous ignore toute demande d’un transfert de zone pour contoso.com provenant d’une interface de serveur qui a une adresse IP de 10.0.0.33:  
  
```  
Add-DnsServerZoneTransferPolicy -Name "InternalTransfers" -Action IGNORE -ServerInterfaceIP "ne,10.0.0.33" -PassThru -ZoneName "contoso.com"  
```  
  
## <a name="dns-policy-scenarios"></a>Scénarios de stratégie DNS

Pour plus d’informations sur la façon d’utiliser une stratégie DNS pour des scénarios spécifiques, consultez les rubriques suivantes de ce guide.

- [Utiliser une stratégie DNS pour la géolocalisation en fonction de gestion du trafic avec des serveurs principaux](primary-geo-location.md)  
- [Utiliser une stratégie DNS pour la géolocalisation gestion basée sur le trafic avec déploiements principaux / secondaires](primary-secondary-geo-location.md)  
- [Utiliser une stratégie DNS pour les réponses DNS intelligentes basées sur l’heure du jour](dns-tod-intelligent.md)
- [Les réponses DNS basées sur l’heure du jour avec un Azure Cloud serveur d’applications](dns-tod-azure-cloud-app-server.md)
- [Utiliser une stratégie DNS pour Split-Brain déploiement DNS](split-brain-DNS-deployment.md)
- [Utiliser une stratégie DNS pour Split-Brain DNS dans ActiveDirectory](dns-sb-with-ad.md)
- [Utiliser une stratégie DNS pour l’application de filtres sur les requêtes DNS](apply-filters-on-dns-queries.md)
- [Utiliser une stratégie DNS pour l’équilibrage de la charge de l’Application](app-lb.md)
- [Utiliser une stratégie DNS pour l’Application équilibrage de charge avec géolocalisation](app-lb-geo.md)


