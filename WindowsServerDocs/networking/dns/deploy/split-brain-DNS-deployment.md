---
title: Utiliser une stratégie DNS pour un déploiement DNS Split-Brain
description: Cette rubrique fait partie du Guide de scénario de stratégie DNS pour Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: a255a4a5-c1a0-4edc-b41a-211bae397e3c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5449c9e96a5a9ecd08ca35e703a76927f4e27158
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356014"
---
# <a name="use-dns-policy-for-split-brain-dns-deployment"></a>Utiliser une stratégie DNS pour le déploiement DNS split @ no__t-0Brain

>S’applique à : Windows Server 2016

Vous pouvez utiliser cette rubrique pour apprendre à configurer une stratégie DNS dans Windows Server @ no__t-0 2016 pour les déploiements DNS de fractionnement Brain, où il existe deux versions d’une seule zone, une pour les utilisateurs internes sur l’intranet de votre organisation et une pour les utilisateurs externes, qui sont en général, les utilisateurs sur Internet.

>[!NOTE]
>Pour plus d’informations sur l’utilisation de la stratégie DNS pour le déploiement DNS de Split @ no__t-0brain avec Active Directory Zones DNS intégrée, consultez [utiliser une stratégie DNS pour le système DNS split-brain dans Active Directory](dns-sb-with-ad.md).

Auparavant, ce scénario nécessitait que les administrateurs DNS maintiennent deux serveurs DNS différents, chacun fournissant des services à chaque ensemble d’utilisateurs, interne et externe. Si seuls quelques enregistrements à l’intérieur de la zone étaient fractionnés @ no__t-0brained ou si les deux instances de la zone (interne et externe) ont été déléguées au même domaine parent, il s’agit d’un énigme de gestion. 

Un autre scénario de configuration pour le déploiement de Split Brain est le contrôle de récurrence sélective pour la résolution de noms DNS. Dans certains cas, les serveurs DNS de l’entreprise sont censés effectuer une résolution récursive sur Internet pour les utilisateurs internes, alors qu’ils doivent également jouer le rôle de serveurs de noms faisant autorité pour les utilisateurs externes et bloquer leur récurrence. 

Cette rubrique contient les sections suivantes.

- [Exemple de déploiement de fractionnement DNS](#bkmk_sbexample)
- [Exemple de contrôle de récurrence sélective DNS](#bkmk_recursion)

## <a name="bkmk_sbexample"></a>Exemple de déploiement de fractionnement DNS
Vous trouverez ci-dessous un exemple de la façon dont vous pouvez utiliser la stratégie DNS pour accomplir le scénario décrit précédemment du DNS split-brain.

Cette section contient les rubriques suivantes :

- [Fonctionnement du déploiement de fractionnement DNS](#bkmk_sbhow)
- [Comment configurer un déploiement de fractionnement DNS](#bkmk_sbconfigure)


Cet exemple utilise une société fictive contoso, qui gère un site Web de carrière sur www.career.contoso.com.

Le site comporte deux versions, une pour les utilisateurs internes où des publications de travaux internes sont disponibles. Ce site interne est disponible à l’adresse IP locale 10.0.0.39. 

La deuxième version est la version publique du même site, disponible à l’adresse IP publique 65.55.39.10.

En l’absence de stratégie DNS, l’administrateur doit héberger ces deux zones sur des serveurs DNS Windows Server distincts et les gérer séparément. 

À l’aide de stratégies DNS, ces zones peuvent désormais être hébergées sur le même serveur DNS.  

L’illustration suivante représente ce scénario.

![Déploiement du DNS split-brain](../../media/DNS-Split-Brain/Dns-Split-Brain-01.jpg)  


## <a name="bkmk_sbhow"></a>Fonctionnement du déploiement de fractionnement DNS

Lorsque le serveur DNS est configuré avec les stratégies DNS requises, chaque demande de résolution de nom est évaluée par rapport aux stratégies sur le serveur DNS.

L’interface serveur est utilisée dans cet exemple comme critère pour faire la distinction entre les clients internes et externes.

Si l’interface de serveur sur laquelle la requête est reçue correspond à l’une des stratégies, l’étendue de zone associée est utilisée pour répondre à la requête. 

Ainsi, dans notre exemple, les requêtes DNS pour www.career.contoso.com reçues sur l’adresse IP privée (10.0.0.56) reçoivent une réponse DNS qui contient une adresse IP interne ; et les requêtes DNS reçues sur l’interface réseau publique reçoivent une réponse DNS qui contient l’adresse IP publique dans l’étendue de la zone par défaut (cela est identique à la résolution de requête normale).  

## <a name="bkmk_sbconfigure"></a>Comment configurer un déploiement de fractionnement DNS
Pour configurer le déploiement de Split-Brain DNS à l’aide d’une stratégie DNS, vous devez suivre les étapes ci-dessous.

- [Créer les étendues de zone](#bkmk_zscopes)  
- [Ajouter des enregistrements aux étendues de zone](#bkmk_records)  
- [Créer les stratégies DNS](#bkmk_policies)

Les sections suivantes fournissent des instructions de configuration détaillées.

>[!IMPORTANT]
>Les sections suivantes incluent des exemples de commandes Windows PowerShell qui contiennent des exemples de valeurs pour de nombreux paramètres. Veillez à remplacer les valeurs d’exemple dans ces commandes par des valeurs appropriées pour votre déploiement avant d’exécuter ces commandes. 

### <a name="bkmk_zscopes"></a>Créer les étendues de zone

Une étendue de zone est une instance unique de la zone. Une zone DNS peut avoir plusieurs étendues de zone, chaque étendue contenant son propre ensemble d’enregistrements DNS. Le même enregistrement peut être présent dans plusieurs étendues, avec des adresses IP différentes ou les mêmes adresses IP. 

> [!NOTE]
> Par défaut, il existe une étendue de zone sur les zones DNS. Cette étendue de zone porte le même nom que la zone, et les opérations DNS héritées fonctionnent sur cette étendue. Cette étendue de zone par défaut hébergera la version externe de www.career.contoso.com.

Vous pouvez utiliser l’exemple de commande suivant pour partitionner l’étendue de zone contoso.com afin de créer une étendue de zone interne. L’étendue interne de la zone sera utilisée pour conserver la version interne de www.career.contoso.com.

`Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "internal"`

Pour plus d’informations, consultez [Add-DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

### <a name="bkmk_records"></a>Ajouter des enregistrements aux étendues de zone

L’étape suivante consiste à ajouter les enregistrements qui représentent l’hôte du serveur Web dans les deux étendues de zone : interne et par défaut (pour les clients externes). 

Dans l’étendue de la zone interne, l’enregistrement <strong>www.Career.contoso.com</strong> est ajouté avec l’adresse IP 10.0.0.39, qui est une adresse IP privée. et dans l’étendue de zone par défaut, le même enregistrement, <strong>www.Career.contoso.com</strong>, est ajouté avec l’adresse IP 65.55.39.10.

Non **–** le paramètre ZoneScope est fourni dans les exemples de commande suivants lorsque l’enregistrement est ajouté à l’étendue de la zone par défaut. Cela revient à ajouter des enregistrements à une zone vanille.

`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10"
`
`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39” -ZoneScope "internal"
`

Pour plus d’informations, consultez [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

### <a name="bkmk_policies"></a>Créer les stratégies DNS

Une fois que vous avez identifié les interfaces de serveur pour le réseau externe et le réseau interne et que vous avez créé les étendues de zone, vous devez créer des stratégies DNS qui connectent les étendues de zone interne et externe.

>[!NOTE]
>Cet exemple utilise l’interface de serveur comme critère pour faire la distinction entre les clients internes et externes. Une autre méthode pour différencier les clients externes et internes consiste à utiliser des sous-réseaux clients en tant que critères. Si vous pouvez identifier les sous-réseaux auxquels appartiennent les clients internes, vous pouvez configurer la stratégie DNS pour la différencier en fonction du sous-réseau client. Pour plus d’informations sur la configuration de la gestion du trafic à l’aide de critères de sous-réseau client, consultez [utiliser une stratégie DNS pour la gestion du trafic basée sur la géolocalisation avec les serveurs principaux](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/scenario--use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers).

Lorsque le serveur DNS reçoit une requête sur l’interface privée, la réponse de la requête DNS est retournée à partir de l’étendue de la zone interne.

>[!NOTE]
>Aucune stratégie n’est requise pour le mappage de l’étendue de zone par défaut. 

Dans l’exemple de commande suivant, 10.0.0.56 est l’adresse IP sur l’interface réseau privée, comme indiqué dans l’illustration précédente.

`Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,10.0.0.56" -ZoneScope "internal,1" -ZoneName contoso.com`

Pour plus d’informations, consultez [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  


## <a name="bkmk_recursion"></a>Exemple de contrôle de récurrence sélective DNS

Voici un exemple de la façon dont vous pouvez utiliser la stratégie DNS pour accomplir le scénario décrit précédemment du contrôle de récurrence sélective DNS.

Cette section contient les rubriques suivantes :

- [Fonctionnement du contrôle de récurrence sélective DNS](#bkmk_recursionhow)
- [Comment configurer le contrôle de récurrence sélective DNS](#bkmk_recursionconfigure)

Cet exemple utilise la même société fictive que dans l’exemple précédent, contoso, qui gère un site Web de carrière sur www.career.contoso.com.

Dans l’exemple de déploiement de fractionnement DNS, le même serveur DNS répond aux clients externes et internes et fournit des réponses différentes. 

Certains déploiements DNS peuvent nécessiter que le même serveur DNS effectue une résolution de noms récursive pour les clients internes, en plus d’agir comme serveur de noms faisant autorité pour les clients externes. C’est ce que l’on appelle le contrôle de récurrence sélective DNS.

Dans les versions précédentes de Windows Server, l’activation de la récursivité signifiait qu’elle était activée sur l’ensemble du serveur DNS pour toutes les zones. Étant donné que le serveur DNS écoute également les requêtes externes, la récursivité est activée pour les clients internes et externes, ce qui fait du serveur DNS un programme de résolution ouvert. 

Un serveur DNS configuré comme un programme de résolution ouvert peut être vulnérable à l’épuisement des ressources et peut être utilisé par des clients malveillants pour créer des attaques par réflexion. 

Pour cette raison, les administrateurs DNS de contoso ne souhaitent pas que le serveur DNS pour contoso.com exécute la résolution de noms récursive pour les clients externes. Le contrôle de récurrence n’est nécessaire que pour les clients internes, tandis que le contrôle de récurrence peut être bloqué pour les clients externes. 

L’illustration suivante représente ce scénario.

![Contrôle de récurrence sélective](../../media/DNS-Split-Brain/Dns-Split-Brain-02.jpg) 


### <a name="bkmk_recursionhow"></a>Fonctionnement du contrôle de récurrence sélective DNS

Si une requête pour laquelle le serveur DNS contoso ne fait pas autorité est reçue, par exemple pour www.microsoft.com, la demande de résolution de nom est évaluée par rapport aux stratégies sur le serveur DNS. 

Étant donné que ces requêtes ne se trouvent pas dans une zone, les stratégies de niveau zone \(AS définies dans l’exemple split-brain @ no__t-1 ne sont pas évaluées. 

Le serveur DNS évalue les stratégies de récurrence, et les requêtes reçues sur l’interface privée correspondent à **SplitBrainRecursionPolicy**. Cette stratégie pointe vers une étendue de récurrence où la récursivité est activée.

Le serveur DNS effectue ensuite la récursivité pour obtenir la réponse de www.microsoft.com à partir d’Internet et met en cache la réponse localement. 

Si la requête est reçue sur l’interface externe, aucune stratégie DNS ne correspond et le paramètre de récursivité par défaut, qui est **désactivé** dans le cas présent, est appliqué.

Cela empêche le serveur d’agir comme un programme de résolution ouvert pour les clients externes, alors qu’il agit comme un programme de résolution de mise en cache pour les clients internes. 

### <a name="bkmk_recursionconfigure"></a>Comment configurer le contrôle de récurrence sélective DNS

Pour configurer le contrôle de récurrence sélective DNS à l’aide de la stratégie DNS, vous devez suivre les étapes ci-dessous.

- [Créer des étendues de récursivité DNS](#bkmk_recscopes)
- [Créer des stratégies de récursivité DNS](#bkmk_recpolicy)

#### <a name="bkmk_recscopes"></a>Créer des étendues de récursivité DNS

Les étendues de récurrence sont des instances uniques d’un groupe de paramètres qui contrôlent la récursivité sur un serveur DNS. Une étendue de récurrence contient une liste de redirecteurs et spécifie si la récursivité est activée. Un serveur DNS peut avoir de nombreuses étendues de récursivité. 

Le paramètre de récurrence héritée et la liste des redirecteurs sont appelés étendue de récurrence par défaut. Vous ne pouvez pas ajouter ou supprimer l’étendue de récursivité par défaut, identifiée par le nom point \(». \).

Dans cet exemple, le paramètre de récurrence par défaut est désactivé, tandis qu’une nouvelle étendue de récurrence pour les clients internes est créée lorsque la récursivité est activée.

    
    Set-DnsServerRecursionScope -Name . -EnableRecursion $False
    Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True 
    

Pour plus d’informations, consultez [Add-DnsServerRecursionScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverrecursionscope?view=win10-ps)

#### <a name="bkmk_recpolicy"></a>Créer des stratégies de récursivité DNS

Vous pouvez créer des stratégies de récurrence du serveur DNS pour choisir une étendue de récurrence pour un ensemble de requêtes qui correspondent à des critères spécifiques. 

Si le serveur DNS ne fait pas autorité pour certaines requêtes, les stratégies de récurrence du serveur DNS vous permettent de contrôler la résolution des requêtes. 

Dans cet exemple, l’étendue de récursivité interne avec récursivité activée est associée à l’interface réseau privée.

Vous pouvez utiliser l’exemple de commande suivant pour configurer des stratégies de récursivité DNS.

    
    Add-DnsServerQueryResolutionPolicy -Name "SplitBrainRecursionPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP "EQ,10.0.0.39"
    

Pour plus d’informations, consultez [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).

À présent, le serveur DNS est configuré avec les stratégies DNS requises pour un serveur de noms split-brain ou un serveur DNS avec le contrôle de récurrence sélective activé pour les clients internes.

Vous pouvez créer des milliers de stratégies DNS en fonction de vos besoins en matière de gestion du trafic, et toutes les nouvelles stratégies sont appliquées de manière dynamique, sans redémarrer le serveur DNS-sur les requêtes entrantes. 

Pour plus d’informations, consultez le [Guide du scénario de stratégie DNS](DNS-Policy-Scenario-Guide.md).
