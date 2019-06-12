---
title: Vue d’ensemble des stratégies DNS
description: Cette rubrique fait partie de DNS stratégie scénario Guide pour Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 566bc270-81c7-48c3-a904-3cba942ad463
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6c4d8f9bb6c56e8f90a90cd4e77565a39211f719
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446436"
---
# <a name="dns-policies-overview"></a>Vue d’ensemble des stratégies DNS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur la politique de DNS, qui est une nouveauté dans Windows Server 2016. Vous pouvez utiliser une stratégie DNS pour l’emplacement géographique en fonction de gestion du trafic, les réponses DNS intelligentes basées sur l’heure de la journée, pour gérer un seul serveur DNS configuré pour le fractionnement\-déploiement cerveau, application de filtres sur les requêtes DNS et bien plus encore. Les éléments suivants fournissent plus de détails sur ces fonctionnalités.

-   **Équilibrage de charge application.** Lorsque vous avez déployé plusieurs instances d’une application à différents emplacements, vous pouvez utiliser une stratégie DNS pour équilibrer la charge du trafic entre les différentes instances d’application, attribution dynamique de la charge du trafic de l’application.

-   **Geo\-emplacement en fonction de gestion du trafic.** Vous pouvez utiliser une stratégie DNS pour autoriser les serveurs DNS principaux et secondaires répondre aux requêtes du client DNS basés sur l’emplacement géographique du client et la ressource à laquelle le client tente de se connecter, en fournissant le client avec l’adresse IP de la plus proche ressource. 

-   **Fractionnement cerveau DNS.** Fractionnement\-du cerveau DNS, enregistrements DNS sont divisées en différentes zones étendues sur le même serveur DNS, et les clients DNS reçoivent une réponse selon que les clients sont des clients internes ou externes. Vous pouvez configurer le fractionnement\-du cerveau DNS pour les zones intégrées à Active Directory ou pour les zones sur des serveurs DNS autonomes.

-   **De filtrage.** Vous pouvez configurer une stratégie DNS pour créer des filtres de requête qui sont basées sur des critères que vous fournissez. Filtres de requête dans une stratégie DNS vous autorise à configurer le serveur DNS pour répondre de manière personnalisée en fonction de la requête DNS et le client DNS qui envoie la requête DNS. 
-   **Investigation.** Vous pouvez utiliser une stratégie DNS pour rediriger les clients DNS malveillants vers un non\-existant adresse IP au lieu de les diriger vers l’ordinateur, il essaie d’atteindre.

-   **Heure du jour en fonction de la redirection.** Vous pouvez utiliser une stratégie DNS pour distribuer le trafic d’application entre différentes instances distribuées d’une application à l’aide de stratégies DNS qui sont basés sur l’heure du jour.

## <a name="new-concepts"></a>Nouveaux Concepts  
Pour créer des stratégies pour prendre en charge les scénarios répertoriés ci-dessus, il est nécessaire être en mesure d’identifier les groupes d’enregistrements dans une zone, les groupes de clients sur un réseau, notamment les éléments. Ces éléments sont représentés par les nouveaux objets DNS suivantes :  

- **Sous-réseau du client :** un objet de sous-réseau client représente un sous-réseau IPv4 ou IPv6 à partir de laquelle les requêtes sont envoyées à un serveur DNS. Vous pouvez créer des sous-réseaux pour définir des stratégies à appliquer selon quel sous-réseau que les demandes proviennent d’ultérieurement. Par exemple, dans un scénario DNS split brain, la demande pour la résolution pour un nom, tel que <em>www.microsoft.com</em> peuvent être traitées avec une adresse IP interne aux clients à partir de sous-réseaux internes et une adresse IP différente pour les clients externes sous-réseaux.

- **Étendue de la récursivité :** étendues de récursivité sont des instances uniques d’un groupe de paramètres qui contrôlent la récursivité sur un serveur DNS. Une étendue de récursivité contient une liste de redirecteurs et spécifie si la récursivité est activée. Un serveur DNS peut avoir plusieurs étendues de récursivité. DNS server récursivité stratégies vous permettent de choisir une étendue de récurrence pour un ensemble de requêtes. Si le serveur DNS ne fait pas autorité pour certaines requêtes, les stratégies de récurrence de serveur DNS vous autorise à contrôler la façon de résoudre ces requêtes. Vous pouvez spécifier les redirecteurs à utiliser et s’il faut utiliser la récursivité.

- **Étendues de la zone :** une zone DNS peut avoir plusieurs étendues de zone, avec chaque étendue de la zone contenant leur propre jeu d’enregistrements DNS. Le même enregistrement peut être présent dans plusieurs étendues, avec des adresses IP différentes. En outre, les transferts de zone sont effectuées au niveau de la portée de zone. Cela signifie que les enregistrements à partir d’une étendue de la zone dans une zone principale seront transférées vers la même étendue de la zone dans une zone secondaire.

## <a name="types-of-policy"></a>Types de stratégies

Stratégies de DNS sont divisés par niveau et le type. Vous pouvez utiliser des stratégies de résolution de requête pour définir le mode de traitement des requêtes et des stratégies de transfert de Zone pour définir comment les transferts de zone se produisent. Vous pouvez appliquer chaque type de stratégie au niveau du serveur ou le niveau de la zone.

### <a name="query-resolution-policies"></a>Stratégies de résolution de requête

Vous pouvez utiliser des stratégies de résolution de requête DNS pour spécifier la résolution entrante requêtes sont gérées par un serveur DNS. Chaque stratégie de résolution de requête DNS contient les éléments suivants :  

|Champ|Description|Valeurs possibles|  
|---------|---------------|-------------------|  
|**Nom**|Nom de la stratégie|-Jusqu'à 256 caractères<br />-Peut contenir n’importe quel caractère valide pour un nom de fichier|  
|**État**|État de la stratégie|-Activer (par défaut)<br />-Désactivé|  
|**Niveau**|Niveau de stratégie|-   Server<br />-   Zone|  
|**Ordre de traitement**|Une fois qu’une requête est classifiée par niveau et s’applique sur, le serveur recherche la première stratégie pour laquelle la requête correspond aux critères et l’applique à interroger|-Valeur numérique<br />-Valeur unique par la stratégie qui contient le même niveau et l’applique à la valeur|  
|**Action**|Action à effectuer par le serveur DNS|-Autoriser (valeur par défaut pour le niveau de zone)<br />-Deny (valeur par défaut au niveau du serveur)<br />-   Ignore|  
|**Critères**|Condition de stratégie (et/ou) et la liste des critères à respecter pour la stratégie à appliquer|-Opérateur de condition (et/ou)<br />-Liste des critères (voir le tableau de critère ci-dessous)|  
|**Portée**|Liste des étendues de zone et de valeurs pondérées par étendue. Valeurs pondérées sont utilisés pour la distribution d’équilibrage de charge. Par exemple, si cette liste inclut datacenter1 avec un poids de 3 et datacenter2 avec un poids de 5 le serveur répond avec un enregistrement à partir de datacentre1 trois fois hors huit requêtes|-Liste des étendues de zone (par nom) et des poids|  

> [!NOTE]
> Les stratégies de niveau serveur peuvent avoir uniquement les valeurs **Deny** ou **ignorer** en tant qu’action.

Le champ de critères de stratégie DNS est composé de deux éléments :


|              Nom               |                                         Description                                          |                                                                                                                               Exemples de valeurs                                                                                                                               |
|---------------------------------|----------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        **Sous-réseau du client**        | Nom du sous-réseau client prédéfini. Permet de vérifier le sous-réseau à partir de laquelle la requête a été envoyée. |                             -   **EQ, Espagne, France** -correspond à la valeur true si le sous-réseau est identifié en tant que l’Espagne ou France<br />-   **NOU, Canada, Mexique** -correspond à la valeur true si le sous-réseau du client est n’importe quel sous-réseau autre que le Canada et le Mexique                             |
|     **Protocole de transport**      |        Protocole utilisé dans la requête de transport. Les entrées possibles sont **UDP** et **TCP**        |                                                                                                                    -   **EQ,TCP**<br />-   **EQ, UDP**                                                                                                                     |
|      **Protocole Internet**      |        Protocole réseau utilisé dans la requête. Les entrées possibles sont **IPv4** et **IPv6**        |                                                                                                                   -   **EQ, IPv4**<br />-   **EQ, IPv6**                                                                                                                    |
| **Adresse IP de l’Interface du serveur** |                   Adresse IP de l’interface réseau du serveur DNS entrante                   |                                                                                                              -   **EQ, 10.0.0.1**<br />-   **EQ, 192.168.1.1**                                                                                                              |
|            **FQDN**             |            Nom de domaine complet de l’enregistrement de la requête, avec la possibilité d’utiliser un caractère générique            | -   **EQ, www.contoso.com** -résout assurances rue uniquement le si la requête tente de résoudre le <em>www.contoso.com</em> nom de domaine complet<br />-   **EQ,\*. contoso.com,\*. woodgrove.com** -correspond à la valeur true si la requête est pour n’importe quel enregistrement se terminant par *contoso.com***ou***woodgrove.com* |
|         **Type de requête**          |                          Type d’enregistrement en cours d’interrogation (A, SVR, TXT)                          |                                                  -   **EQ, TXT, SRV** -correspond à true si la requête demande un fichier texte **ou** enregistrement SRV<br />-   **EQ, MX** -correspond à true si la requête demande un enregistrement MX                                                   |
|         **Heure du jour**         |                              Heure de que la requête est reçue.                               |                                                                    -   **EQ, 10:00 à 12:00, 22:00 à 23:00** -correspond à true si la requête est reçue entre 10 h à midi et **ou** entre 22 h 00 et 23 h 00                                                                    |

À l’aide du tableau ci-dessus comme point de départ, le tableau ci-dessous peut servir à définir un critère qui est utilisé pour les requêtes pour tous les types d’enregistrements, mais les enregistrements SRV dans le domaine contoso.com provenant d’un client dans le sous-réseau 10.0.0.0/24 via TCP entre 8 et 22 h 00 via je interface 10.0.0.3 :  

|Nom|Value|  
|--------|---------|  
|Sous-réseau du client|EQ,10.0.0.0/24|  
|Protocole de transport|EQ, TCP|  
|Adresse IP de l’Interface du serveur|EQ,10.0.0.3|  
|Nom de domaine complet (FQDN)|EQ,*.contoso.com|  
|Type de requête|NOU, SRV|  
|Heure du jour|EQ, 20:00 À 22:00|  

Vous pouvez créer une requête plusieurs stratégies de résolution du même niveau, tant qu’ils disposent d’une valeur différente pour l’ordre de traitement. Lorsque plusieurs stratégies sont disponibles, le serveur DNS traite les requêtes entrantes de la manière suivante :  

![Traitement de la stratégie DNS](../../media/DNS-Policies-Overview/DNSQueryResolutionPolicyFlowchart.png)  

### <a name="recursion-policies"></a>Stratégies de récurrence  
Stratégies de récurrence sont une spéciale **type** des stratégies au niveau du serveur. Stratégies de récurrence de contrôlent comment le serveur DNS exécute la récursivité pour une requête. Les stratégies de la récursivité s’appliquent uniquement lorsque le traitement des requêtes atteint le chemin d’accès de la récursivité. Vous pouvez choisir de refuser ou ignorer une valeur pour la récursivité pour un ensemble de requêtes. Vous pouvez également choisir un ensemble de redirecteurs pour un ensemble de requêtes.  

Vous pouvez utiliser des stratégies de récurrence pour implémenter une configuration DNS de Split-brain. Dans cette configuration, le serveur DNS exécute la récursivité pour un ensemble de clients pour une requête, alors que le serveur DNS n’effectue pas de récursivité pour d’autres clients de cette requête.  

Stratégies de récurrence contient les mêmes éléments que contient par une stratégie de résolution de requête DNS standard, ainsi que les éléments dans le tableau ci-dessous :  

|Nom|Description|  
|--------|---------------|  
|**Appliquer envers la récursivité**|Spécifie que cette stratégie doit uniquement être utilisée pour la récursivité.|  
|**Étendue de récursivité**|Nom de l’étendue de la récursivité.|  

> [!NOTE]  
> La récursivité stratégies peuvent uniquement être créées au niveau du serveur.  

### <a name="zone-transfer-policies"></a>Stratégies de transfert de zone  
Stratégies de transfert de zone contrôlent si un transfert de zone est autorisé ou non par votre serveur DNS. Vous pouvez créer des stratégies pour le transfert de zone au niveau du serveur ou de niveau de la zone. Stratégies de niveau serveur s’appliquent à chaque requête de transfert de zone qui se produit sur le serveur DNS. Stratégies de niveau de zone s’appliquent uniquement sur les requêtes sur une zone hébergée sur le serveur DNS. L’utilisation la plus courante pour les stratégies de niveau zone consiste à implémenter les listes bloquées ou sans échec.  

> [!NOTE]  
> Stratégies de transfert de zone peuvent uniquement utiliser refuser ou ignorer en tant qu’actions.  

Vous pouvez utiliser la stratégie de transfert de zone au niveau serveur ci-dessous pour refuser un transfert de zone pour le domaine contoso.com à partir d’un sous-réseau donné :  

```  
Add-DnsServerZoneTransferPolicy -Name DenyTransferOfContosoToFabrikam -Zone contoso.com -Action DENY -ClientSubnet "EQ,192.168.1.0/24"  
```  

Vous pouvez créer des stratégies du même niveau, de transfert de zone plusieurs tant qu’ils disposent d’une valeur différente pour l’ordre de traitement. Lorsque plusieurs stratégies sont disponibles, le serveur DNS traite les requêtes entrantes de la manière suivante :  

![Processus de DNS pour plusieurs stratégies de transfert de zone](../../media/DNS-Policies-Overview/DNSPolicyZone.png)  

## <a name="managing-dns-policies"></a>La gestion des stratégies DNS  
Vous pouvez créer et gérer des stratégies de DNS à l’aide de PowerShell. Les exemples ci-dessous passent par différents exemples de scénarios que vous pouvez configurer via des stratégies DNS :  

### <a name="traffic-management"></a>Gestion du trafic  
Vous pouvez diriger le trafic basé sur un nom de domaine complet sur différents serveurs selon l’emplacement du client DNS. L’exemple ci-dessous montre comment créer des stratégies de gestion pour diriger les clients à partir d’un certain sous-réseau à un centre de données nord-américain et à partir d’un autre sous-réseau à un centre de données européen de trafic.  

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

Les deux premières lignes du script de créent des objets de sous-réseau pour l’Amérique du Nord et Europe de clients. Les deux lignes après cela créent une étendue de la zone dans le domaine contoso.com, un pour chaque région. Les deux lignes ensuite créent un enregistrement dans chaque zone qui associe ww.contoso.com à l’adresse IP différente, un pour l’Europe, un autre pour l’Amérique du Nord. Enfin, les dernières lignes du script créent deux stratégies de résolution de requête de DNS, un pour être appliqué au sous-réseau Amérique du Nord, un autre pour le sous-réseau de l’Europe.  

### <a name="block-queries-for-a-domain"></a>Requêtes de bloc pour un domaine  
Vous pouvez utiliser une stratégie de résolution de requête DNS aux requêtes de bloc à un domaine. L’exemple ci-dessous bloque toutes les requêtes treyresearch.net :  

```  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicy" -Action IGNORE -FQDN "EQ,*.treyresearch.com"  
```  

### <a name="block-queries-from-a-subnet"></a>Requêtes de bloc à partir d’un sous-réseau  
Vous pouvez également bloquer les requêtes provenant d’un sous-réseau spécifique. Le script ci-dessous crée un sous-réseau pour 172.0.33.0/24, puis crée une stratégie pour ignorer toutes les requêtes en provenance de ce sous-réseau :  

```  
Add-DnsServerClientSubnet -Name "MaliciousSubnet06" -IPv4Subnet 172.0.33.0/24  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06"  
```  

### <a name="allow-recursion-for-internal-clients"></a>Autoriser la récursivité pour les clients internes  
Vous pouvez contrôler la récursivité à l’aide d’une stratégie de résolution de requête DNS. L’exemple ci-dessous peut être utilisé pour activer la récursivité pour les clients internes, lors de la désactivation pour les clients externes dans un scénario de cerveau de fractionnement.  

```  
Set-DnsServerRecursionScope -Name . -EnableRecursion $False   
Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True  
Add-DnsServerQueryResolutionPolicy -Name "SplitBrainPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP  "EQ,10.0.0.34"  
```  

La première ligne dans le script modifie l’étendue de la récursivité par défaut, appelé simplement «. » (point) pour désactiver la récursivité. La deuxième ligne crée une étendue de récursivité nommée *InternalClients* avec la récurrence activée. Et la troisième ligne crée une stratégie à appliquer le nouvellement créé étendue de récursivité à toutes les requêtes parviennent une interface de serveur qui a 10.0.0.34 comme une adresse IP.  

### <a name="create-a-server-level-zone-transfer-policy"></a>Créer une stratégie de transfert de zone au niveau serveur  
Vous pouvez contrôler le transfert de zone dans un formulaire plus granulaire à l’aide de stratégies de transfert de Zone DNS. L’exemple de script ci-dessous peut être utilisé pour autoriser les transferts de zone pour n’importe quel serveur sur un sous-réseau donné :  

```  
Add-DnsServerClientSubnet -Name "AllowedSubnet" -IPv4Subnet 172.21.33.0/24  
Add-DnsServerZoneTransferPolicy -Name "NorthAmericaPolicy" -Action IGNORE -ClientSubnet "ne,AllowedSubnet"  
```  

La première ligne dans le script crée un objet de sous-réseau nommé *AllowedSubnet* avec l’adresse IP bloquer 172.21.33.0/24. La deuxième ligne crée une stratégie de transfert de zone pour autoriser les transferts de zone à n’importe quel serveur DNS sur le sous-réseau créé précédemment.  

### <a name="create-a-zone-level-zone-transfer-policy"></a>Créer une stratégie de transfert de zone au niveau de zone  
Vous pouvez également créer des stratégies de transfert de zone au niveau de zone. L’exemple ci-dessous ignore toute demande de transfert de zone pour contoso.com provenant d’une interface de serveur qui a une adresse IP de 10.0.0.33 :  

```  
Add-DnsServerZoneTransferPolicy -Name "InternalTransfers" -Action IGNORE -ServerInterfaceIP "ne,10.0.0.33" -PassThru -ZoneName "contoso.com"  
```  

## <a name="dns-policy-scenarios"></a>Scénarios de stratégie DNS

Pour plus d’informations sur la façon d’utiliser une stratégie DNS pour des scénarios spécifiques, consultez les rubriques suivantes dans ce guide.

- [Utiliser une stratégie DNS pour l’emplacement géographique en fonction de gestion du trafic avec des serveurs principaux](primary-geo-location.md)  
- [Utiliser une stratégie DNS pour l’emplacement géographique en fonction de gestion du trafic avec des déploiements principaux / secondaires](primary-secondary-geo-location.md)  
- [Utiliser une stratégie DNS pour les réponses DNS intelligentes basées sur l’heure du jour](dns-tod-intelligent.md)
- [Réponses DNS basées sur l’heure du jour avec Azure Cloud du serveur d’applications](dns-tod-azure-cloud-app-server.md)
- [Utiliser une stratégie DNS pour le déploiement de DNS « split brain »](split-brain-DNS-deployment.md)
- [Utiliser une stratégie DNS pour DNS « split brain » dans Active Directory](dns-sb-with-ad.md)
- [Utiliser une stratégie DNS pour l’application des filtres sur les requêtes DNS](apply-filters-on-dns-queries.md)
- [Utiliser une stratégie DNS pour l’équilibrage de charge de l’Application](app-lb.md)
- [Utiliser une stratégie DNS pour l’Application équilibrage de charge avec géolocalisation](app-lb-geo.md)

## <a name="using-dns-policy-on-read-only-domain-controllers"></a>À l’aide d’une stratégie DNS sur les contrôleurs de domaine en lecture seule

Une stratégie DNS est compatible avec les contrôleurs de domaine en lecture seule. Notez qu’un redémarrage du service serveur DNS est requis pour les nouvelles stratégies DNS à charger sur les contrôleurs de domaine en lecture seule. Cela n’est pas nécessaire sur les contrôleurs de domaine accessible en écriture.
