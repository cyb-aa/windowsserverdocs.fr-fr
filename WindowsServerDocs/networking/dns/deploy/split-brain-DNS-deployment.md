---
title: Utiliser une stratégie DNS pour un déploiement DNS Split-Brain
description: Cette rubrique fait partie de DNS stratégie scénario Guide pour Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: a255a4a5-c1a0-4edc-b41a-211bae397e3c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c74bb2ee2f1647716c8c38e392434a5b7f01805f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446393"
---
# <a name="use-dns-policy-for-split-brain-dns-deployment"></a>Utiliser une stratégie DNS pour le fractionnement\-déploiement DNS du cerveau

>S’applique à : Windows Server 2016

Vous pouvez utiliser cette rubrique pour savoir comment configurer une stratégie DNS dans Windows Server&reg; 2016 pour les déploiements DNS « split brain », où il existe deux versions d’une seule zone - un pour les utilisateurs internes sur l’intranet de votre organisation et un pour les utilisateurs externes qui sont généralement des utilisateurs sur Internet.

>[!NOTE]
>Pour plus d’informations sur la façon d’utiliser une stratégie DNS pour le fractionnement\-cerveau déploiement DNS à Active Directory des Zones DNS intégrées, consultez [utiliser une stratégie DNS pour DNS Split-Brain dans Active Directory](dns-sb-with-ad.md).

Auparavant, ce scénario nécessitait que les administrateurs DNS maintiennent deux serveurs DNS différents, chaque fournissant des services à chaque ensemble d’utilisateurs, internes et externes. Si seulement quelques enregistrements à l’intérieur de la zone ont été divisées\-brained ou les deux instances de la zone (interne et externe) ont été déléguées au même domaine parent, c’est devenu une énigme de gestion. 

Un autre scénario de configuration pour le déploiement « Split Brain » est sélective contrôle la récursivité pour la résolution de nom DNS. Dans certains cas, les serveurs DNS d’entreprise sont tenues d’effectuer des résolution récursive sur Internet pour les utilisateurs internes, pendant qu’ils également doivent agir en tant que serveurs de noms faisant autorité pour les utilisateurs externes et bloquer la récursivité pour eux. 

Cette rubrique contient les sections suivantes.

- [Exemple de déploiement de DNS « split brain »](#bkmk_sbexample)
- [Exemple de contrôle de récursivité sélectif de DNS](#bkmk_recursion)

## <a name="bkmk_sbexample"></a>Exemple de déploiement de DNS « split brain »
Voici un exemple de comment vous pouvez utiliser une stratégie DNS pour accomplir le scénario décrit précédemment dans DNS « split brain ».

Cette section contient les rubriques suivantes :

- [Fonctionne du déploiement DNS « split brain »](#bkmk_sbhow)
- [Comment configurer le déploiement de DNS « split brain »](#bkmk_sbconfigure)


Cet exemple utilise une société fictive, Contoso, qui gère un site Web de votre carrière à www.career.contoso.com.

Le site a deux versions, un pour les utilisateurs internes où les validations de travail internes sont disponibles. Ce site interne est disponible à l’adresse IP locale 10.0.0.39. 

La deuxième version est la version publique du même site, qui est disponible à l’adresse IP publique 65.55.39.10.

En l’absence d’une stratégie DNS, l’administrateur est nécessaire pour héberger ces deux zones sur des serveurs DNS de Windows Server distincts et de les gérer séparément. 

À l’aide de stratégies DNS ces zones peuvent maintenant être hébergées sur le même serveur DNS.  

L’illustration suivante représente ce scénario.

![Déploiement de DNS « split brain »](../../media/DNS-Split-Brain/Dns-Split-Brain-01.jpg)  


## <a name="bkmk_sbhow"></a>Fonctionne du déploiement DNS « split brain »

Lorsque le serveur DNS est configuré avec les stratégies DNS requises, chaque demande de résolution de nom est recherchée dans les stratégies sur le serveur DNS.

L’Interface du serveur est utilisé dans cet exemple comme critère de faire la distinction entre les clients internes et externes.

Si l’interface du serveur sur lequel la requête est reçue correspond à aucune des stratégies, l’étendue de la zone associée est utilisée pour répondre à la requête. 

Par conséquent, dans notre exemple, les requêtes DNS pour www.career.contoso.com qui sont reçus sur l’adresse IP privée (10.0.0.56) recevoir une réponse DNS qui contient une adresse IP interne ; et les requêtes DNS qui sont reçus sur l’interface de réseau public recevoir une réponse DNS qui contient l’adresse IP publique dans l’étendue de la zone par défaut (cela est identique à la résolution de requête normale).  

## <a name="bkmk_sbconfigure"></a>Comment configurer le déploiement de DNS « split brain »
Pour configurer le déploiement de Split-Brain DNS à l’aide d’une stratégie DNS, vous devez utiliser les étapes suivantes.

- [Créer des étendues de la Zone](#bkmk_zscopes)  
- [Ajoutez des enregistrements dans les étendues de Zone](#bkmk_records)  
- [Créer les stratégies DNS](#bkmk_policies)

Les sections suivantes fournissent des instructions de configuration détaillées.

>[!IMPORTANT]
>Les sections suivantes incluent des exemples de commandes Windows PowerShell qui contiennent des exemples de valeurs de paramètres. Veillez à remplacer les exemples de valeurs dans ces commandes avec les valeurs appropriées pour votre déploiement avant d’exécuter ces commandes. 

### <a name="bkmk_zscopes"></a>Créer des étendues de la Zone

Une étendue de la zone est une instance unique de la zone. Une zone DNS peut avoir plusieurs étendues de zone, avec chaque étendue de la zone contenant son propre ensemble d’enregistrements DNS. Le même enregistrement peut être présent dans plusieurs étendues, avec différentes adresses IP ou les mêmes adresses IP. 

> [!NOTE]
> Par défaut, une étendue de la zone existe sur les zones DNS. Cette étendue de la zone a le même nom que la zone, et des opérations DNS héritées travailler sur cette étendue. Cette étendue de la zone par défaut va héberger la version externe de www.career.contoso.com.

Vous pouvez utiliser la commande suivante pour partitionner la zone étendue contoso.com pour créer une étendue de la zone interne. L’étendue de la zone interne permet de conserver la version interne de www.career.contoso.com.

`Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "internal"`

Pour plus d’informations, consultez [DnsServerZoneScope-ajouter](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

### <a name="bkmk_records"></a>Ajoutez des enregistrements dans les étendues de Zone

L’étape suivante consiste à ajouter les enregistrements représentant l’hôte du serveur Web dans les étendues de deux zone - internes et la valeur par défaut (pour les clients externes). 

Dans l’étendue de la zone interne, l’enregistrement <strong>www.career.contoso.com</strong> a été ajouté avec l’adresse IP 10.0.0.39, qui est une adresse IP privée ; et dans l’étendue de la zone par défaut le même enregistrement, <strong>www.career.contoso.com</strong>, est ajouté par l’adresse IP 65.55.39.10.

Ne **– ZonePortée** paramètre est fourni dans l’exemple de commande suivant lorsque l’enregistrement est ajouté à l’étendue de la zone par défaut. Cela revient à ajouter des enregistrements à une zone vanille.

`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10"
`
`
Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39” -ZoneScope "internal"
`

Pour plus d’informations, consultez [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

### <a name="bkmk_policies"></a>Créer les stratégies DNS

Une fois que vous avez identifié les interfaces de serveur pour le réseau externe et le réseau interne et que vous avez créé les étendues de zone, vous devez créer des stratégies DNS qui se connectent les étendues de zone internes et externes.

>[!NOTE]
>Cet exemple utilise l’interface du serveur comme critère de faire la distinction entre les clients internes et externes. Une autre méthode permettant de faire la distinction entre les clients internes et externes est à l’aide de sous-réseaux du client comme un critère. Si vous pouvez identifier les sous-réseaux auxquels appartiennent les clients internes, vous pouvez configurer une stratégie DNS pour différencier en fonction de sous-réseau du client. Pour plus d’informations sur la façon de configurer la gestion du trafic à l’aide de critères de sous-réseau client, consultez [utiliser une stratégie DNS pour l’emplacement géographique en fonction de la gestion du trafic avec des serveurs principaux](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/scenario--use-dns-policy-for-geo-location-based-traffic-management-with-primary-servers).

Lorsque le serveur DNS reçoit une requête sur l’interface privée, la réponse de la requête DNS est retournée à partir de l’étendue de la zone interne.

>[!NOTE]
>Aucune stratégie n’est requis pour le mappage de l’étendue de la zone par défaut. 

Dans l’exemple de commande, 10.0.0.56 est l’adresse IP sur l’interface de réseau privé, comme indiqué dans l’illustration précédente.

`Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,10.0.0.56" -ZoneScope "internal,1" -ZoneName contoso.com`

Pour plus d’informations, consultez [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  


## <a name="bkmk_recursion"></a>Exemple de contrôle de récursivité sélectif de DNS

Voici un exemple de comment vous pouvez utiliser une stratégie DNS pour accomplir le scénario décrit précédemment du contrôle de récursivité sélectif de DNS.

Cette section contient les rubriques suivantes :

- [Comment le fonctionnement du contrôle DNS récursivité sélective](#bkmk_recursionhow)
- [Comment configurer le contrôle de récursivité sélectif de DNS](#bkmk_recursionconfigure)

Cet exemple utilise la même société fictive, comme dans l’exemple précédent, Contoso, qui gère un site Web de votre carrière à www.career.contoso.com.

Dans l’exemple de déploiement « split brain » DNS, le même serveur DNS répond pour les clients internes et externes et leur offre des réponses différentes. 

Certains déploiements DNS peuvent nécessiter le même serveur DNS pour effectuer la résolution de noms récursive pour les clients internes en plus agissant comme le serveur de noms faisant autorité pour les clients externes. Cela s’appelle le contrôle de récursivité sélectif de DNS.

Dans les versions précédentes de Windows Server, l’activation de la récursivité signifiait qu’il a été activé sur l’ensemble du serveur DNS pour toutes les zones. Étant donné que le serveur DNS est également à l’écoute pour les requêtes externes, la récursivité est activée pour les clients internes et externes, rendre le serveur DNS un résolveur d’ouvrir. 

Un serveur DNS qui est configuré comme un programme de résolution ouvert peut-être être vulnérable à un épuisement des ressources et peut être employée abusivement malveillant aux clients de créer les attaques par réflexion. 

Pour cette raison, les administrateurs DNS de Contoso ne souhaite pas le serveur DNS pour contoso.com effectuer la résolution de noms récursive pour les clients externes. Il est uniquement nécessaire pour le contrôle de la récursivité pour les clients internes, tandis que le contrôle de la récursivité peut être bloqué pour les clients externes. 

L’illustration suivante représente ce scénario.

![Contrôle de récursivité sélective](../../media/DNS-Split-Brain/Dns-Split-Brain-02.jpg) 


### <a name="bkmk_recursionhow"></a>Comment le fonctionnement du contrôle DNS récursivité sélective

Si une requête pour laquelle le serveur DNS de Contoso est non fiable est reçue, tels que www.Microsoft.com, puis la demande de résolution de nom est recherchée dans les stratégies sur le serveur DNS. 

Étant donné que ces requêtes ne relèvent pas de n’importe quelle zone, la zone de niveau stratégies \(tel que défini dans l’exemple « split brain »\) ne sont pas évaluées. 

Le serveur DNS évalue les stratégies de récursivité et les requêtes qui sont reçus sur la correspondance de l’interface privée le **SplitBrainRecursionPolicy**. Cette stratégie pointe vers une étendue de récursivité où la récursivité est activée.

Le serveur DNS exécute la récursivité pour obtenir la réponse de www.microsoft.com à partir d’Internet, puis met en cache la réponse localement. 

Si la requête est reçue sur l’interface externe, aucune correspondance de stratégies DNS et le paramètre de récurrence par défaut - qui dans ce cas est **désactivé** -est appliqué.

Cela empêche le serveur agissant comme un programme de résolution ouvert pour les clients externes, alors qu’il agit comme un programme de résolution de mise en cache pour les clients internes. 

### <a name="bkmk_recursionconfigure"></a>Comment configurer le contrôle de récursivité sélectif de DNS

Pour configurer le contrôle de récursivité sélectif de DNS à l’aide d’une stratégie DNS, vous devez utiliser les étapes suivantes.

- [Créer des étendues de récursivité DNS](#bkmk_recscopes)
- [Créer des stratégies de récursivité DNS](#bkmk_recpolicy)

#### <a name="bkmk_recscopes"></a>Créer des étendues de récursivité DNS

Les étendues de récursivité sont des instances uniques d’un groupe de paramètres qui contrôlent la récursivité sur un serveur DNS. Une étendue de récursivité contient une liste de redirecteurs et spécifie si la récursivité est activée. Un serveur DNS peut avoir plusieurs étendues de récursivité. 

Le paramètre de récursivité hérité et de la liste des redirecteurs sont appelés à l’étendue de la récursivité par défaut. Vous ne pouvez pas ajouter ou supprimer l’étendue de la récursivité par défaut, identifié par le point de nom \(«. » \).

Dans cet exemple, le paramètre de récurrence par défaut est désactivé, pendant la création d’une nouvelle étendue de récursivité pour les clients internes où la récursivité est activée.

    
    Set-DnsServerRecursionScope -Name . -EnableRecursion $False
    Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True 
    

Pour plus d’informations, consultez [DnsServerRecursionScope-ajouter](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverrecursionscope?view=win10-ps)

#### <a name="bkmk_recpolicy"></a>Créer des stratégies de récursivité DNS

Vous pouvez créer le serveur DNS des stratégies de récursivité pour choisir une étendue de récurrence pour un ensemble de requêtes qui correspondent à des critères spécifiques. 

Si le serveur DNS ne fait pas autorité pour certaines requêtes, les stratégies de récurrence de serveur DNS vous autorise à contrôler la façon de résoudre les requêtes. 

Dans cet exemple, l’étendue de la récursivité interne avec la récurrence activée est associé à l’interface de réseau privé.

Vous pouvez utiliser la commande suivante pour configurer des stratégies de récursivité DNS.

    
    Add-DnsServerQueryResolutionPolicy -Name "SplitBrainRecursionPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP "EQ,10.0.0.39"
    

Pour plus d’informations, consultez [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).

Désormais, le serveur DNS est configuré avec les stratégies DNS requises pour un serveur de noms « split brain » ou un serveur DNS avec le contrôle de récursivité sélective activé pour les clients internes.

Vous pouvez créer des milliers de stratégies DNS, en fonction de votre trafic en matière de gestion, et toutes les nouvelles stratégies sont appliquées dynamiquement, sans avoir à redémarrer le serveur DNS : sur les requêtes entrantes. 

Pour plus d’informations, consultez [Guide de scénario de stratégie DNS](DNS-Policy-Scenario-Guide.md).
