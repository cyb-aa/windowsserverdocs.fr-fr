---
title: Vue d’ensemble des stratégies DNS
description: Cette rubrique fait partie du Guide de scénario de stratégie DNS pour Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: 566bc270-81c7-48c3-a904-3cba942ad463
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 613bb7f43b382389dc0db953a48668147cfaee88
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356045"
---
# <a name="dns-policies-overview"></a>Vue d’ensemble des stratégies DNS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur la stratégie DNS, qui est une nouveauté de Windows Server 2016. Vous pouvez utiliser une stratégie DNS pour la gestion du trafic basée sur la géolocalisation, les réponses DNS intelligentes en fonction de l’heure de la journée, pour gérer un serveur DNS unique configuré pour le déploiement\-Brain de fractionnement, l’application de filtres sur les requêtes DNS, etc. Les éléments suivants fournissent plus de détails sur ces fonctionnalités.

-   **Équilibrage de charge de l’application.** Lorsque vous avez déployé plusieurs instances d’une application à différents emplacements, vous pouvez utiliser la stratégie DNS pour équilibrer la charge du trafic entre les différentes instances d’application, en allouant de manière dynamique la charge du trafic pour l’application.

-   **Zone géographique\-gestion du trafic basée sur l’emplacement.** Vous pouvez utiliser une stratégie DNS pour permettre aux serveurs DNS principaux et secondaires de répondre aux requêtes du client DNS en fonction de l’emplacement géographique du client et de la ressource à laquelle le client tente de se connecter, en fournissant au client l’adresse IP la plus proche. ressource. 

-   **Fractionnement du DNS Brain.** Avec le DNS de Split\-Brain, les enregistrements DNS sont répartis en différentes étendues de zones sur le même serveur DNS, et les clients DNS reçoivent une réponse selon que les clients sont des clients internes ou externes. Vous pouvez configurer le service DNS de fractionnement\-Brain pour les zones intégrées Active Directory ou pour les zones sur des serveurs DNS autonomes.

-   **Filtration.** Vous pouvez configurer une stratégie DNS pour créer des filtres de requête basés sur des critères que vous fournissez. Les filtres de requête dans la stratégie DNS vous permettent de configurer le serveur DNS pour qu’il réponde de manière personnalisée en fonction de la requête DNS et du client DNS qui envoie la requête DNS. 
-   **Investigation.** Vous pouvez utiliser une stratégie DNS pour rediriger les clients DNS malveillants vers une adresse IP non\-existante au lieu de les rediriger vers l’ordinateur auquel ils essaient d’accéder.

-   **Redirection basée sur l’heure de la journée.** Vous pouvez utiliser une stratégie DNS pour distribuer le trafic d’application sur différentes instances géographiquement distribuées d’une application à l’aide de stratégies DNS basées sur l’heure de la journée.

## <a name="new-concepts"></a>Nouveaux concepts  
Pour créer des stratégies pour prendre en charge les scénarios mentionnés ci-dessus, il est nécessaire de pouvoir identifier des groupes d’enregistrements dans une zone, des groupes de clients sur un réseau, entre autres éléments. Ces éléments sont représentés par les nouveaux objets DNS suivants :  

- **Sous-réseau client :** un objet sous-réseau client représente un sous-réseau IPv4 ou IPv6 à partir duquel les requêtes sont envoyées à un serveur DNS. Vous pouvez créer des sous-réseaux pour définir ultérieurement les stratégies à appliquer en fonction du sous-réseau d’où proviennent les demandes. Par exemple, dans un scénario DNS split brain, la demande de résolution d’un nom tel que <em>www.Microsoft.com</em> peut recevoir une réponse avec une adresse IP interne aux clients à partir de sous-réseaux internes, et une adresse IP différente aux clients dans les sous-réseaux externes.

- **Étendue de récursivité :** les étendues de récurrence sont des instances uniques d’un groupe de paramètres qui contrôlent la récursivité sur un serveur DNS. Une étendue de récurrence contient une liste de redirecteurs et spécifie si la récursivité est activée. Un serveur DNS peut avoir de nombreuses étendues de récursivité. Les stratégies de récurrence de serveur DNS vous permettent de choisir une étendue de récurrence pour un ensemble de requêtes. Si le serveur DNS ne fait pas autorité pour certaines requêtes, les stratégies de récurrence du serveur DNS vous permettent de contrôler la façon de résoudre ces requêtes. Vous pouvez spécifier les redirecteurs à utiliser et s’il faut utiliser la récursivité.

- **Étendues de zone :** une zone DNS peut avoir plusieurs étendues de zone, chaque étendue contenant son propre ensemble d’enregistrements DNS. Le même enregistrement peut être présent dans plusieurs étendues, avec des adresses IP différentes. En outre, les transferts de zone sont effectués au niveau de l’étendue de la zone. Cela signifie que les enregistrements d’une étendue de zone dans une zone principale sont transférés vers la même étendue de zone dans une zone secondaire.

## <a name="types-of-policy"></a>Types de stratégie

Les stratégies DNS sont divisées par niveau et par type. Vous pouvez utiliser des stratégies de résolution de requêtes pour définir le mode de traitement des requêtes et des stratégies de transfert de zone pour définir le mode de transfert des zones. Vous pouvez appliquer chaque type de stratégie au niveau du serveur ou au niveau de la zone.

### <a name="query-resolution-policies"></a>Stratégies de résolution des requêtes

Vous pouvez utiliser des stratégies de résolution de requêtes DNS pour spécifier la façon dont les requêtes de résolution entrantes sont gérées par un serveur DNS. Chaque stratégie de résolution de requête DNS contient les éléments suivants :  

|Champ|Description|Valeurs possibles|  
|---------|---------------|-------------------|  
|**Nom**|Nom de la stratégie|-Jusqu’à 256 caractères<br />-Peut contenir n’importe quel caractère valide pour un nom de fichier|  
|**État**|État de la stratégie|-Enable (par défaut)<br />-Désactivé|  
|**Niveau**|Niveau de stratégie|-Serveur<br />-Zone|  
|**Ordre de traitement**|Une fois qu’une requête est classée par niveau et s’applique à, le serveur recherche la première stratégie pour laquelle la requête correspond aux critères et l’applique à la requête.|-Valeur numérique<br />-Valeur unique par stratégie contenant le même niveau et s’applique à la valeur|  
|**Action**|Action à effectuer par le serveur DNS|-Allow (valeur par défaut pour le niveau zone)<br />-Deny (valeur par défaut au niveau du serveur)<br />-Ignorer|  
|**Critère**|Condition de stratégie (et/ou) et liste de critères à respecter pour que la stratégie soit appliquée|-Opérateur de condition (AND/OR)<br />-Liste de critères (voir le tableau des critères ci-dessous)|  
|**Portée**|Liste des étendues de zone et des valeurs pondérées par étendue. Les valeurs pondérées sont utilisées pour la distribution de l’équilibrage de charge. Par exemple, si cette liste comprend Datacenter1 avec un poids de 3 et datacenter2 avec un poids de 5, le serveur répond avec un enregistrement de datacentre1 trois fois plus de huit requêtes|-Liste des étendues de zone (par nom) et pondérations|  

> [!NOTE]
> Les stratégies au niveau du serveur peuvent uniquement avoir les valeurs **Deny** ou **ignore** comme action.

Le champ de critères de stratégie DNS est composé de deux éléments :


|              Nom               |                                         Description                                          |                                                                                                                               Exemples de valeurs                                                                                                                               |
|---------------------------------|----------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        **Sous-réseau client**        | Nom d’un sous-réseau client prédéfini. Utilisé pour vérifier le sous-réseau à partir duquel la requête a été envoyée. |                             -   **EQ, Espagne, France** -correspond à true si le sous-réseau est identifié comme Espagne ou France<br />-   ne **, Canada, Mexique** -correspond à true si le sous-réseau client est un sous-réseau autre que le Canada et le Mexique                             |
|     **Protocole de transport**      |        Protocole de transport utilisé dans la requête. Les entrées possibles sont **UDP** et **TCP**        |                                                                                                                    -   **EQ, TCP**<br />-   **EQ, UDP**                                                                                                                     |
|      **Protocole Internet**      |        Protocole réseau utilisé dans la requête. Les entrées possibles sont **IPv4** et **IPv6**        |                                                                                                                   -   **EQ, IPv4**<br />-   **EQ, IPv6**                                                                                                                    |
| **Adresse IP de l’interface serveur** |                   Adresse IP de l’interface réseau du serveur DNS entrant                   |                                                                                                              -   **EQ, 10.0.0.1**<br />-   **EQ, 192.168.1.1**                                                                                                              |
|            **DOMAINE complet**             |            Nom de domaine complet de l’enregistrement dans la requête, avec la possibilité d’utiliser un caractère générique            | -   **EQ, www. contoso. com** : se résout en true uniquement si la requête tente de résoudre le nom de domaine complet <em>www.contoso.com</em><br />-   **EQ,\*. contoso.com,\*. woodgrove.com** : prend la valeur true si la requête concerne un enregistrement se terminant par *contoso.com***ou***Woodgrove.com* |
|         **Type de requête**          |                          Type d’enregistrement interrogé (A, SRV, TXT)                          |                                                  -   **EQ, txt, SRV** -correspond à true si la requête demande un enregistrement txt **ou** SRV<br />-   **EQ, MX** -correspond à true si la requête demande un enregistrement MX                                                   |
|         **Heure de la journée**         |                              Heure à laquelle la requête est reçue                               |                                                                    -   **EQ, 10:00-12:00, 22:00-23:00** -correspond à true si la requête est reçue entre 10 heures et midi, **ou** entre 22h00 et 23 h 00                                                                    |

En utilisant le tableau ci-dessus comme point de départ, le tableau ci-dessous peut être utilisé pour définir un critère utilisé pour faire correspondre des requêtes pour tout type d’enregistrement, mais des enregistrements SRV dans le domaine contoso.com provenant d’un client dans le sous-réseau 10.0.0.0/24 via TCP entre 8 et 10 h 00 via l’interface 10.0.0.3 :  

|Nom|Valeur|  
|--------|---------|  
|Sous-réseau client|EQ, 10.0.0.0/24|  
|Protocole de transport|EQ, TCP|  
|Adresse IP de l’interface serveur|EQ, 10.0.0.3|  
|Nom de domaine complet (FQDN)|EQ, *. contoso. com|  
|Type de requête|NE, SRV|  
|Heure de la journée|EQ, 20:00-22:00|  

Vous pouvez créer plusieurs stratégies de résolution de requêtes du même niveau, à condition qu’elles aient une valeur différente pour l’ordre de traitement. Lorsque plusieurs stratégies sont disponibles, le serveur DNS traite les requêtes entrantes de la manière suivante :  

![Traitement de la stratégie DNS](../../media/DNS-Policies-Overview/DNSQueryResolutionPolicyFlowchart.png)  

### <a name="recursion-policies"></a>Stratégies de récurrence  
Les stratégies de récurrence sont un **type** spécial de stratégies au niveau du serveur. Les stratégies de récurrence contrôlent la façon dont le serveur DNS effectue la récursivité pour une requête. Les stratégies de récurrence s’appliquent uniquement lorsque le traitement des requêtes atteint le chemin de récursivité. Vous pouvez choisir la valeur refuser ou ignorer pour la récursivité pour un ensemble de requêtes. Vous pouvez également choisir un ensemble de redirecteurs pour un ensemble de requêtes.  

Vous pouvez utiliser des stratégies de récurrence pour implémenter une configuration DNS split-brain. Dans cette configuration, le serveur DNS effectue une récursivité pour un ensemble de clients pour une requête, alors que le serveur DNS n’effectue pas de récursivité pour d’autres clients pour cette requête.  

Les stratégies de récurrence contiennent les mêmes éléments que contient une stratégie de résolution de requêtes DNS standard, ainsi que les éléments du tableau ci-dessous :  

|Nom|Description|  
|--------|---------------|  
|**Appliquer à la récursivité**|Spécifie que cette stratégie ne doit être utilisée que pour la récursivité.|  
|**Étendue de récurrence**|Nom de l’étendue de récursivité.|  

> [!NOTE]  
> Les stratégies de récurrence ne peuvent être créées qu’au niveau du serveur.  

### <a name="zone-transfer-policies"></a>Stratégies de transfert de zone  
Les stratégies de transfert de zone contrôlent si un transfert de zone est autorisé ou non par votre serveur DNS. Vous pouvez créer des stratégies pour le transfert de zone au niveau du serveur ou au niveau de la zone. Les stratégies au niveau du serveur s’appliquent à chaque requête de transfert de zone qui se produit sur le serveur DNS. Les stratégies de niveau zone s’appliquent uniquement aux requêtes sur une zone hébergée sur le serveur DNS. L’utilisation la plus courante pour les stratégies de niveau zone consiste à implémenter des listes bloquées ou sécurisées.  

> [!NOTE]  
> Les stratégies de transfert de zone peuvent uniquement utiliser DENY ou IGNORe comme actions.  

Vous pouvez utiliser la stratégie de transfert de zone au niveau du serveur ci-dessous pour refuser un transfert de zone pour le domaine contoso.com à partir d’un sous-réseau donné :  

```  
Add-DnsServerZoneTransferPolicy -Name DenyTransferOfContosoToFabrikam -Zone contoso.com -Action DENY -ClientSubnet "EQ,192.168.1.0/24"  
```  

Vous pouvez créer plusieurs stratégies de transfert de zone du même niveau, à condition qu’elles aient une valeur différente pour l’ordre de traitement. Lorsque plusieurs stratégies sont disponibles, le serveur DNS traite les requêtes entrantes de la manière suivante :  

![Processus DNS pour plusieurs stratégies de transfert de zone](../../media/DNS-Policies-Overview/DNSPolicyZone.png)  

## <a name="managing-dns-policies"></a>Gestion des stratégies DNS  
Vous pouvez créer et gérer des stratégies DNS à l’aide de PowerShell. Les exemples ci-dessous passent en revue différents exemples de scénarios que vous pouvez configurer par le biais de stratégies DNS :  

### <a name="traffic-management"></a>Gestion du trafic  
Vous pouvez diriger le trafic basé sur un nom de domaine complet vers différents serveurs en fonction de l’emplacement du client DNS. L’exemple ci-dessous montre comment créer des stratégies de gestion du trafic pour diriger les clients d’un sous-réseau donné vers un centre de donnés nord-américain et d’un autre sous-réseau vers un centre de donnée européen.  

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

Les deux premières lignes du script créent des objets de sous-réseau client pour Amérique du Nord et Europe. Les deux lignes qui suivent créent une étendue de zone dans le domaine contoso.com, une pour chaque région. Les deux lignes qui suivent créent un enregistrement dans chaque zone qui associe ww.contoso.com à une adresse IP différente, l’un pour l’Europe, l’autre pour Amérique du Nord. Enfin, les dernières lignes du script créent deux stratégies de résolution de requêtes DNS, une à appliquer au sous-réseau Amérique du Nord, une autre au sous-réseau Europe.  

### <a name="block-queries-for-a-domain"></a>Bloquer les requêtes pour un domaine  
Vous pouvez utiliser une stratégie de résolution de requêtes DNS pour bloquer des requêtes vers un domaine. L’exemple ci-dessous bloque toutes les requêtes à treyresearch.net :  

```  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicy" -Action IGNORE -FQDN "EQ,*.treyresearch.com"  
```  

### <a name="block-queries-from-a-subnet"></a>Bloquer les requêtes à partir d’un sous-réseau  
Vous pouvez également bloquer les requêtes provenant d’un sous-réseau spécifique. Le script ci-dessous crée un sous-réseau pour 172.0.33.0/24, puis crée une stratégie pour ignorer toutes les requêtes provenant de ce sous-réseau :  

```  
Add-DnsServerClientSubnet -Name "MaliciousSubnet06" -IPv4Subnet 172.0.33.0/24  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06"  
```  

### <a name="allow-recursion-for-internal-clients"></a>Autoriser la récursivité pour les clients internes  
Vous pouvez contrôler la récursivité à l’aide d’une stratégie de résolution de requête DNS. L’exemple ci-dessous peut être utilisé pour activer la récursivité pour les clients internes, tout en le désactivant pour les clients externes dans un scénario split brain.  

```  
Set-DnsServerRecursionScope -Name . -EnableRecursion $False   
Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True  
Add-DnsServerQueryResolutionPolicy -Name "SplitBrainPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP  "EQ,10.0.0.34"  
```  

La première ligne du script modifie l’étendue de récurrence par défaut, simplement nommée « . » (point) pour désactiver la récursivité. La deuxième ligne crée une étendue de récursivité nommée *InternalClients* avec la récursivité activée. La troisième ligne crée une stratégie pour appliquer l’étendue de récursivité nouvellement créée à toutes les requêtes entrantes via une interface de serveur qui a 10.0.0.34 comme adresse IP.  

### <a name="create-a-server-level-zone-transfer-policy"></a>Créer une stratégie de transfert de zone au niveau du serveur  
Vous pouvez contrôler le transfert de zone dans une forme plus granulaire en utilisant des stratégies de transfert de zone DNS. L’exemple de script ci-dessous peut être utilisé pour autoriser les transferts de zone pour n’importe quel serveur sur un sous-réseau donné :  

```  
Add-DnsServerClientSubnet -Name "AllowedSubnet" -IPv4Subnet 172.21.33.0/24  
Add-DnsServerZoneTransferPolicy -Name "NorthAmericaPolicy" -Action IGNORE -ClientSubnet "ne,AllowedSubnet"  
```  

La première ligne du script crée un objet sous-réseau nommé *AllowedSubnet* avec le bloc IP 172.21.33.0/24. La deuxième ligne crée une stratégie de transfert de zone pour autoriser les transferts de zone vers n’importe quel serveur DNS sur le sous-réseau créé précédemment.  

### <a name="create-a-zone-level-zone-transfer-policy"></a>Créer une stratégie de transfert de zone au niveau de la zone  
Vous pouvez également créer des stratégies de transfert de zone au niveau de la zone. L’exemple ci-dessous ignore toute demande de transfert de zone pour contoso.com provenant d’une interface de serveur qui a l’adresse IP 10.0.0.33 :  

```  
Add-DnsServerZoneTransferPolicy -Name "InternalTransfers" -Action IGNORE -ServerInterfaceIP "eq,10.0.0.33" -PassThru -ZoneName "contoso.com"  
```  

## <a name="dns-policy-scenarios"></a>Scénarios de stratégie DNS

Pour plus d’informations sur l’utilisation de la stratégie DNS pour des scénarios spécifiques, consultez les rubriques suivantes de ce guide.

- [Utiliser une stratégie DNS pour la gestion du trafic basée sur la géolocalisation avec les serveurs principaux](primary-geo-location.md)  
- [Utiliser une stratégie DNS pour la gestion du trafic basée sur la géolocalisation avec des déploiements principaux secondaires](primary-secondary-geo-location.md)  
- [Utiliser une stratégie DNS pour les réponses DNS intelligentes en fonction de l’heure de la journée](dns-tod-intelligent.md)
- [Réponses DNS basées sur l’heure de la journée avec un serveur d’applications Cloud Azure](dns-tod-azure-cloud-app-server.md)
- [Utiliser une stratégie DNS pour le déploiement DNS split-brain](split-brain-DNS-deployment.md)
- [Utiliser la stratégie DNS pour le DNS split-brain dans Active Directory](dns-sb-with-ad.md)
- [Utiliser une stratégie DNS pour appliquer des filtres sur les requêtes DNS](apply-filters-on-dns-queries.md)
- [Utiliser une stratégie DNS pour l’équilibrage de charge d’application](app-lb.md)
- [Utiliser une stratégie DNS pour l’équilibrage de charge d’application avec reconnaissance de géolocalisation](app-lb-geo.md)

## <a name="using-dns-policy-on-read-only-domain-controllers"></a>Utilisation de la stratégie DNS sur les contrôleurs de domaine en lecture seule

La stratégie DNS est compatible avec les contrôleurs de domaine en lecture seule. Notez qu’un redémarrage du service serveur DNS est nécessaire pour charger les nouvelles stratégies DNS sur les contrôleurs de domaine en lecture seule. Cela n’est pas nécessaire sur les contrôleurs de domaine accessibles en écriture.
