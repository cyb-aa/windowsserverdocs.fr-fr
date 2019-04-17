---
title: Utiliser une stratégie DNS pour Split-Brain déploiement DNS
description: Cette rubrique fait partie de la DNS stratégie scénario Guide pour Windows Server2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: a255a4a5-c1a0-4edc-b41a-211bae397e3c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0b25ac752ea347f4d184628eb26bc7e297443306
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-split-brain-dns-deployment"></a>Utiliser une stratégie DNS pour le déploiement de DNS Split\-Brain

>S’applique à: Windows Server2016

Vous pouvez utiliser cette rubrique pour savoir comment configurer une stratégie DNS dans Windows Server&reg; 2016 pour les déploiements DNS «split brain», où il existe deux versions d’une seule zone - un pour les utilisateurs internes sur l’intranet de votre organisation et un pour les utilisateurs externes, qui sont généralement des utilisateurs sur Internet.

>[!NOTE]
>Pour plus d’informations sur la façon d’utiliser une stratégie DNS pour les Zones DNS intégrées de déploiement de DNS split\-brain avec ActiveDirectory, voir [utiliser une stratégie DNS pour Split-Brain DNS dans ActiveDirectory](dns-sb-with-ad.md).

Auparavant, ce scénario nécessitait que les administrateurs DNS conservent deux serveurs DNS différents, chaque fournissant des services à chaque ensemble d’utilisateurs, internes et externes. Si seulement quelques enregistrements à l’intérieur de la zone ont été split\-brained ou les deux instances de la zone (interne et externe) ont été déléguées au même domaine parent, il est devenu une énigme de gestion. 

Un autre scénario de configuration pour le déploiement «Split Brain» est sélective contrôle la récursivité pour la résolution de noms DNS. Dans certains cas, les serveurs DNS d’entreprise sont tenues d’effectuer des résolution récursive via Internet pour les utilisateurs internes, pendant qu’ils également doivent agir en tant que serveurs de noms de référence pour les utilisateurs externes et bloquer la récursivité pour eux. 

Cette rubrique contient les sections suivantes.

- [Exemple de DNS Split-Brain déploiement](#bkmk_sbexample)
- [Exemple de contrôle de la récursivité sélective DNS](#bkmk_recursion)

##<a name="bkmk_sbexample"></a>Exemple de DNS Split-Brain déploiement
Voici un exemple de la façon dont vous pouvez utiliser une stratégie DNS pour réaliser le scénario décrit précédemment de DNS «split brain».

Cette section contient les rubriques suivantes.

- [Comment DNS Split-Brain fonctionne le déploiement](#bkmk_sbhow)
- [Comment configurer le DNS Split-Brain déploiement](#bkmk_sbconfigure)


Cet exemple utilise une société fictive, Contoso, qui gère un site Web carrière à www.career.contoso.com.

Le site possède deux versions, une pour les utilisateurs internes où les validations internes sont disponibles. Ce site interne est disponible à l’adresse IP locale 10.0.0.39. 

La deuxième version est la version publique du même site, qui est disponible à l’adresse IP publique 65.55.39.10.

En l’absence d’une stratégie DNS, l’administrateur est nécessaire pour héberger ces deux zones sur des serveurs DNS de Windows Server distincts et les gérer séparément. 

À l’aide de stratégies DNS ces zones peuvent désormais être hébergés sur le même serveur DNS.  

L’illustration suivante décrit ce scénario.

![Split-Brain Déploiement de DNS](../../media/DNS-Split-Brain/Dns-Split-Brain-01.jpg)  


##<a name="bkmk_sbhow"></a>Comment DNS Split-Brain fonctionne le déploiement

Lorsque le serveur DNS est configuré avec les stratégies DNS requis, chaque demande de résolution de nom est évalué sur les stratégies sur le serveur DNS.

L’Interface du serveur est utilisé dans cet exemple comme critère de faire la distinction entre les clients internes et externes.

Si l’interface du serveur sur lequel la requête est reçue correspond à aucune des stratégies, la portée de la zone associée est utilisée pour répondre à la requête. 

Par conséquent, dans notre exemple, le serveur DNS interroge www.career.contoso.com sont reçus sur l’adresse IP privée (10.0.0.56) reçoivent une réponse DNS qui contient une adresse IP interne; et les requêtes DNS qui sont reçus sur l’interface de réseau public reçoivent une réponse DNS qui contient l’adresse IP publique dans l’étendue de zone par défaut (cela est identique à la résolution de requête normal).  

##<a name="bkmk_sbconfigure"></a>Comment configurer le DNS Split-Brain déploiement
Pour configurer DNS Split-Brain déploiement en utilisant une stratégie DNS, vous devez utiliser les étapes suivantes.

- [Créer les étendues de Zone](#bkmk_zscopes)  
- [Ajouter des enregistrements pour les étendues de Zone](#bkmk_records)  
- [Créer les stratégies DNS](#bkmk_policies)

Les sections suivantes fournissent des instructions de configuration détaillées.

>[!IMPORTANT]
>Les sections suivantes incluent des exemples de commandes Windows PowerShell qui contiennent des exemples de valeurs pour un grand nombre de paramètres. Assurez-vous que vous remplacez les exemples de valeurs de ces commandes par les valeurs appropriées pour votre déploiement avant d’exécuter ces commandes. 

###<a name="bkmk_zscopes"></a>Créer les étendues de Zone

Une étendue de la zone est une instance unique de la zone. Une zone DNS peut avoir plusieurs étendues de zone, avec chaque étendue de la zone qui contient son propre ensemble d’enregistrements DNS. L’enregistrement même peut être présent dans plusieurs étendues, avec des adresses IP différentes ou les mêmes adresses IP. 

>[!NOTE]
>Par défaut, une étendue de la zone existe sur les zones DNS. Cette étendue de la zone a le même nom que la zone, et les opérations de DNS héritées fonctionnent sur cette étendue. Cette étendue de zone par défaut destiné à héberger la version de www.career.contoso.com externe.

Vous pouvez utiliser la commande suivante à l’étendue de la zone contoso.com pour créer une étendue de la zone interne de partition. L’étendue de la zone interne permet de conserver la version interne de www.career.contoso.com.

`Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "internal"`

Pour plus d’informations, voir [Add-DnsServerZoneScope](https://technet.microsoft.com/library/mt126267.aspx)

###<a name="bkmk_records"></a>Ajouter des enregistrements pour les étendues de Zone

L’étape suivante consiste à ajouter les enregistrements représentant l’hôte du serveur Web dans les étendues de deux zones - internes et par défaut (pour les clients externes). 

Dans l’étendue de la zone interne, l’enregistrement **www.career.contoso.com** est ajouté avec l’adresse IP 10.0.0.39, qui est une adresse IP privée; dans l’étendue de zone par défaut le même enregistrement, **www.career.contoso.com**, est ajouté avec l’adresse IP 65.55.39.10.

Ne **– ZonePortée** paramètre est fourni dans les exemples de commandes suivants lors de l’enregistrement est ajouté à l’étendue de la zone par défaut. Cela est similaire à l’ajout d’enregistrements à une zone classique.

`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10"
`
`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39” -ZoneScope "internal"
`

Pour plus d’informations, voir [Add-DnsServerResourceRecord](https://technet.microsoft.com/library/jj649925.aspx).

###<a name="bkmk_policies"></a>Créer les stratégies DNS

Une fois que vous avez identifié les interfaces de serveur pour le réseau externe et le réseau interne et que vous avez créé les étendues de zone, vous devez créer des stratégies DNS qui se connectent les étendues de zone internes et externes.

>[!NOTE]
>Cet exemple utilise l’interface du serveur en tant que les critères pour faire la différence entre les clients internes et externes. Une autre méthode pour faire la différence entre les clients externes et internes est à l’aide de sous-réseaux de client comme un critère. Si vous pouvez identifier les sous-réseaux auxquels appartiennent les clients internes, vous pouvez configurer une stratégie DNS pour différencier en fonction de sous-réseau de client. Pour plus d’informations sur la façon de configurer la gestion du trafic à l’aide des critères de sous-réseau de client, voir [utiliser une stratégie DNS pour la géolocalisation le trafic de gestion avec des serveurs principaux](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/scenario--use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers).

Lorsque le serveur DNS reçoit une requête sur l’interface privée, la réponse de la requête DNS est retournée à partir de l’étendue de la zone interne.

>[!NOTE]
>Aucune stratégie n’est requis pour le mappage de l’étendue de la zone par défaut. 

Dans la commande suivante, 10.0.0.56 est l’adresse IP sur l’interface de réseau privé, comme illustré dans l’illustration précédente.

`Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,10.0.0.56" -ZoneScope "internal,1" -ZoneName contoso.com`

Pour plus d’informations, voir [Add-DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx).  


## <a name="bkmk_recursion"></a>Exemple de contrôle de la récursivité sélective DNS

Voici un exemple de comment vous pouvez utiliser une stratégie DNS pour réaliser le scénario décrit précédemment du contrôle de la récursivité sélective DNS.

Cette section contient les rubriques suivantes.

- [Comment fonctionne le contrôle la récursivité sélective DNS](#bkmk_recursionhow)
- [Comment configurer le contrôle de la récursivité sélective DNS](#bkmk_recursionconfigure)

Cet exemple utilise la même entreprise fictive comme dans l’exemple précédent, Contoso, qui gère un site Web carrière www.career.contoso.com.

Dans l’exemple de déploiement «split brain» DNS, le même serveur DNS répond pour les clients internes et externes et lui offre différentes réponses. 

Certains déploiements DNS peuvent nécessiter le même serveur DNS pour effectuer la résolution de noms récursive pour les clients internes en plus agissant en tant que le serveur de noms faisant autorité pour les clients externes. Cette situation est appelée contrôle de la récursivité sélective DNS.

Dans les versions précédentes de Windows Server, l’activation de la récursivité signifiait qu’il a été activé sur l’ensemble du serveur DNS pour toutes les zones. Étant donné que le serveur DNS est également à l’écoute pour les requêtes externes, la récursivité est activée pour les clients internes et externes, rendant le serveur DNS un résolveur ouvert. 

Un serveur DNS est configuré comme un résolveur ouvrir peut être vulnérable à l’épuisement des ressources et peut être utilisée par les clients malveillants pour créer les attaques par réflexion. 

Pour cette raison, les administrateurs DNS Contoso ne souhaitez pas que le serveur DNS pour contoso.com effectuer la résolution de noms récursive pour les clients externes. Il est uniquement nécessaire pour le contrôle de la récursivité pour les clients internes, tandis que le contrôle de la récursivité peut être bloqué pour les clients externes. 

L’illustration suivante décrit ce scénario.

![Contrôle la récursivité sélective](../../media/DNS-Split-Brain/Dns-Split-Brain-02.jpg) 


### <a name="bkmk_recursionhow"></a>Comment fonctionne le contrôle la récursivité sélective DNS

Si une requête pour laquelle le serveur DNS de Contoso est faisant est reçue, comme pour www.microsoft.com, puis la demande de résolution de nom est évaluée sur les stratégies sur le serveur DNS. 

Étant donné que ces requêtes ne tombent pas sous n’importe quelle zone, la zone de niveau stratégies \ (comme défini dans l’example\ «split brain») ne sont pas évaluées. 

Le serveur DNS évalue les stratégies de la récursivité et les requêtes sont reçus sur la correspondance d’interface privée le **SplitBrainRecursionPolicy**. Cette stratégie pointe sur une étendue de la récursivité dans lequel la récursivité est activée.

Le serveur DNS exécute la récursivité pour obtenir la réponse pour www.microsoft.com à partir d’Internet, puis met en cache localement la réponse. 

Si la requête est reçue sur le paramètre par défaut la récursivité - dans ce cas, aucune correspondance de stratégies DNS et l’interface externe **désactivé** -est appliquée.

Cela empêche le serveur d’agir comme un résolveur ouvert pour les clients externes, pendant qu’il agit comme un résolveur mise en cache pour les clients internes. 

### <a name="bkmk_recursionconfigure"></a>Comment configurer le contrôle de la récursivité sélective DNS

Pour configurer le contrôle de la récursivité sélective de DNS à l’aide d’une stratégie DNS, vous devez utiliser les étapes suivantes.

- [Créez des étendues de la récursivité DNS](#bkmk_recscopes)
- [Créer des stratégies de la récursivité DNS](#bkmk_recpolicy)

#### <a name="bkmk_recscopes"></a>Créez des étendues de la récursivité DNS

Les étendues de la récursivité sont des instances uniques d’un groupe de paramètres qui contrôlent la récursivité sur un serveur DNS. Une étendue de la récursivité contient une liste des redirecteurs et indique si la récursivité est activée. Un serveur DNS peut avoir plusieurs étendues de la récursivité. 

Le paramètre de la récursivité hérité et de la liste des redirecteurs sont appelés l’étendue de la récursivité par défaut. Impossible d’ajouter ou supprimer l’étendue de la récursivité par défaut, identifié par le point de nom \("." \).

Dans cet exemple, le paramètre de la récursivité par défaut est désactivé, pendant la création d’une nouvelle étendue de la récursivité pour les clients internes où la récursivité est activée.

    
    Set-DnsServerRecursionScope -Name . -EnableRecursion $False
    Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True 
    

Pour plus d’informations, voir [Add-DnsServerRecursionScope](https://technet.microsoft.com/library/mt126268.aspx)

#### <a name="bkmk_recpolicy"></a>Créer des stratégies de la récursivité DNS

Vous pouvez créer le serveur DNS stratégies de récurrence de choisir une étendue de la récursivité pour un ensemble de requêtes qui correspondent aux critères spécifiques. 

Si le serveur DNS ne fait pas autorité pour certaines requêtes, les stratégies de récurrence de serveur DNS permettent de contrôler la façon de résoudre les requêtes. 

Dans cet exemple, l’étendue de la récursivité interne avec la récursivité activée est associé à l’interface de réseau privé

Vous pouvez utiliser la commande suivante pour configurer les stratégies de récurrence de DNS.

    
    Add-DnsServerQueryResolutionPolicy -Name "SplitBrainRecursionPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP  "EQ,10.0.0.39"
    

Pour plus d’informations, voir [Add-DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx).

Désormais, le serveur DNS est configuré avec les stratégies DNS requis pour un serveur de noms «split brain» ou un serveur DNS avec la récursivité sélective contrôle est activé pour les clients internes.

Vous pouvez créer des milliers de stratégies DNS en fonction de votre trafic en matière de gestion, et toutes les nouvelles stratégies sont appliquées dynamiquement, sans redémarrer le serveur DNS - dans les requêtes entrantes. 

Pour plus d’informations, voir [Guide de scénario de stratégie DNS](DNS-Policy-Scenario-Guide.md).
