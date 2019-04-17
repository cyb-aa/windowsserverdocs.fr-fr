---
title: Utiliser une stratégie DNS pour Split-Brain DNS dans ActiveDirectory
description: Vous pouvez utiliser cette rubrique pour tirer parti du trafic des fonctionnalités de gestion des stratégies DNS pour les déploiements de «split brain» avec ActiveDirectory intégré des zones DNS dans Windows Server2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9533204-ad7e-4e49-81c1-559324a16aeb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d294fcad3b48c8698dffd93e94f6ef7ffc681ea2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-split-brain-dns-in-active-directory"></a>Utiliser une stratégie DNS pour Split-Brain DNS dans ActiveDirectory

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour exploiter les fonctionnalités de gestion du trafic des stratégies DNS pour les déploiements split\-brain avec ActiveDirectory intégré DNS zones dans Windows Server2016.

Dans Windows Server2016, la prise en charge des stratégies DNS est étendue à ActiveDirectory des zones DNS intégrées. L’intégration ActiveDirectory fournit des fonctionnalités de haute disponibilité du maître d’opérations-prennent vers le serveur DNS. 

Auparavant, ce scénario nécessitait que les administrateurs DNS conservent deux serveurs DNS différents, chaque fournissant des services à chaque ensemble d’utilisateurs, internes et externes. Si seulement quelques enregistrements à l’intérieur de la zone ont été split\-brained ou les deux instances de la zone (interne et externe) ont été déléguées au même domaine parent, il est devenu une énigme de gestion.

>[!NOTE]
> - Les déploiements DNS sont split\-brain lorsqu’il existe deux versions d’une zone unique, une version pour les utilisateurs internes sur l’intranet d’entreprise et une version pour les utilisateurs externes – qui sont en règle générale, les utilisateurs sur Internet.
> - La rubrique [utiliser une stratégie DNS pour Split-Brain déploiement DNS](split-brain-DNS-deployment.md) explique comment vous pouvez utiliser des stratégies DNS et des étendues de zone pour déployer un système DNS split\-brain sur un seul serveur DNS de Windows Server2016.



##  <a name="example-split-brain-dns-in-active-directory"></a>Exemple Split\-Brain DNS dans ActiveDirectory

Cet exemple utilise une société fictive, Contoso, qui gère un site Web carrière à www.career.contoso.com.

Le site possède deux versions, une pour les utilisateurs internes où les validations internes sont disponibles. Ce site interne est disponible à l’adresse IP locale 10.0.0.39. 

La deuxième version est la version publique du même site, qui est disponible à l’adresse IP publique 65.55.39.10.

En l’absence d’une stratégie DNS, l’administrateur est nécessaire pour héberger ces deux zones sur des serveurs DNS de Windows Server distincts et les gérer séparément. 

À l’aide de stratégies DNS ces zones peuvent désormais être hébergés sur le même serveur DNS.

Si le serveur DNS pour contoso.com est intégrée à ActiveDirectory et est à l’écoute sur deux interfaces réseau, l’administrateur DNS de Contoso pouvez suivre les étapes décrites dans cette rubrique pour effectuer un déploiement split\-brain.

L’administrateur DNS configure les interfaces de serveur DNS avec les adresses IP suivantes.

- La carte réseau tournée vers Internet est configurée avec une adresse IP publique de 208.84.0.53 pour les requêtes externes.
- Carte réseau tournée vers l’Intranet est configurée avec une adresse IP privée 10.0.0.56 pour les requêtes internes.

L’illustration suivante décrit ce scénario.

![Split-Brain Déploiement de DNS intégré à ActiveDirectory](../../media/DNS-SB-AD/DNS-SB-AD.jpg)

## <a name="how-dns-policy-for-split-brain-dns-in-active-directory-works"></a>Fonctionne d’une stratégie DNS pour DNS Split\-Brain dans ActiveDirectory

Lorsque le serveur DNS est configuré avec les stratégies DNS requis, chaque demande de résolution de nom est évalué sur les stratégies sur le serveur DNS.

L’Interface du serveur est utilisé dans cet exemple comme critère de faire la distinction entre les clients internes et externes.

Si l’interface du serveur sur lequel la requête est reçue correspond à aucune des stratégies, la portée de la zone associée est utilisée pour répondre à la requête. 

Par conséquent, dans notre exemple, le serveur DNS interroge www.career.contoso.com sont reçus sur l’adresse IP privée (10.0.0.56) reçoivent une réponse DNS qui contient une adresse IP interne; et les requêtes DNS qui sont reçus sur l’interface de réseau public reçoivent une réponse DNS qui contient l’adresse IP publique dans l’étendue de zone par défaut (cela est identique à la résolution de requête normal).  

Prise en charge pour les mises à jour DNS dynamique \(DDNS\) et le nettoyage est pris en charge uniquement sur l’étendue de la zone par défaut. Étant donné que les clients internes sont traitées par l’étendue de la zone par défaut, les administrateurs DNS Contoso peuvent continuer d’utiliser les mécanismes existants (dynamique DNS ou statique) pour mettre à jour les enregistrements dans contoso.com. Pour les étendues de zone autre que celle par défaut \ (par exemple, l’étendue externe dans cette example\), DDNS ou nettoyage prise en charge n’est pas disponible.

### <a name="high-availability-of-policies"></a>Haute disponibilité des stratégies

Stratégies DNS ne sont pas intégrées à ActiveDirectory. Pour cette raison, les stratégies DNS ne sont pas répliqués vers les autres serveurs DNS qui hébergent la même zone intégrée à ActiveDirectory. 

Stratégies DNS sont stockés sur le serveur DNS local. Vous pouvez facilement exporter des stratégies DNS d’un serveur à un autre en utilisant les exemples de commandes Windows PowerShell suivantes.

    $policies = Get-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server01
    
    $policies |  Add-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server02

Pour plus d’informations, consultez les rubriques de référence Windows PowerShell suivantes.

- [Get-DnsServerQueryResolutionPolicy](https://technet.microsoft.com/itpro/powershell/windows/dns-server/get-dnsserverqueryresolutionpolicy)
- [Add-DnsServerQueryResolutionPolicy](https://technet.microsoft.com/itpro/powershell/windows/dns-server/add-dnsserverqueryresolutionpolicy)


## <a name="how-to-configure-dns-policy-for-split-brain-dns-in-active-directory"></a>Comment configurer une stratégie DNS pour DNS Split\-Brain dans ActiveDirectory

Pour configurer DNS Split-Brain déploiement en utilisant une stratégie DNS, vous devez utiliser les sections suivantes, qui fournissent des instructions de configuration détaillées.

### <a name="add-the-active-directory-integrated-zone"></a>Ajouter la zone intégrée à ActiveDirectory

Vous pouvez utiliser la commande suivante pour ajouter la zone contoso.com intégrée à ActiveDirectory pour le serveur DNS.

    Add-DnsServerPrimaryZone -Name "contoso.com" -ReplicationScope "Domain" -PassThru

Pour plus d’informations, voir [afin d’illustrer](https://technet.microsoft.com/library/jj649876.aspx).

### <a name="create-the-scopes-of-the-zone"></a>Créer les étendues de la Zone

Vous pouvez utiliser cette section pour partitionner la zone contoso.com pour créer une étendue de la zone externe.

Une étendue de la zone est une instance unique de la zone. Une zone DNS peut avoir plusieurs étendues de zone, avec chaque étendue de la zone qui contient son propre ensemble d’enregistrements DNS. L’enregistrement même peut être présent dans plusieurs étendues, avec des adresses IP différentes ou les mêmes adresses IP. 

Étant donné que vous ajoutez cette nouvelle étendue de zone dans une zone intégrée à ActiveDirectory, l’étendue de la zone et les enregistrements qu’elle contient seront répliqués via ActiveDirectory à d’autres serveurs réplica dans le domaine.

Par défaut, une étendue de la zone existe dans chaque zone DNS. Cette étendue de la zone a le même nom que la zone, et les opérations de DNS héritées fonctionnent sur cette étendue. Cette étendue de zone par défaut destiné à héberger la version interne de www.career.contoso.com.

Vous pouvez utiliser la commande suivante pour créer l’étendue de la zone sur le serveur DNS.

    Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "external"

Pour plus d’informations, voir [Add-DnsServerZoneScope](https://technet.microsoft.com/library/mt126267.aspx).

### <a name="add-records-to-the-zone-scopes"></a>Ajouter des enregistrements pour les étendues de Zone

L’étape suivante consiste à ajouter les enregistrements représentant l’hôte du serveur web dans les deux étendues externes de la zone et par défaut \(for internal clients\). 

Dans l’étendue de la zone interne par défaut, l’enregistrement www.career.contoso.com est ajouté avec l’adresse IP 10.0.0.39, qui est une adresse IP privée; et dans l’étendue de la zone externe, le même \(www.career.contoso.com\) enregistrement est ajouté avec l’adresse IP publique 65.55.39.10. 

Les enregistrements \ (à la fois dans l’étendue de la zone interne par défaut et la zone externe scope\) vont être répliqués automatiquement dans le domaine avec leurs étendues de la zone concernée.

Vous pouvez utiliser la commande suivante pour ajouter des enregistrements pour les étendues de zone sur le serveur DNS.

    Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10" -ZoneScope "external"
    
    Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39”

>[!NOTE]
>Le **– ZonePortée** paramètre n’est pas inclus lorsque l’enregistrement est ajouté à l’étendue de la zone par défaut. Cette action est la même que l’ajout d’enregistrements à une zone normale.

Pour plus d’informations, voir [Add-DnsServerResourceRecord](https://technet.microsoft.com/library/jj649925.aspx).

### <a name="create-the-dns-policies"></a>Créer les stratégies DNS

Une fois que vous avez identifié les interfaces de serveur pour le réseau externe et le réseau interne et que vous avez créé les étendues de zone, vous devez créer des stratégies DNS qui se connectent les étendues de zone internes et externes.

>[!NOTE]
>Cet exemple utilise l’interface serveur \ (le paramètre - ServerInterface dans le below\ commande exemple) en tant que les critères à faire la différence entre les clients internes et externes. Une autre méthode pour faire la différence entre les clients externes et internes est à l’aide de sous-réseaux de client comme un critère. Si vous pouvez identifier les sous-réseaux auxquels appartiennent les clients internes, vous pouvez configurer une stratégie DNS pour différencier en fonction de sous-réseau de client. Pour plus d’informations sur la façon de configurer la gestion du trafic à l’aide des critères de sous-réseau de client, voir [utiliser une stratégie DNS pour la géolocalisation le trafic de gestion avec des serveurs principaux](primary-geo-location.md).

Après avoir configuré les stratégies, lors de la réception d’une requête DNS sur l’interface publique, la réponse est renvoyée à partir de l’étendue de la zone externe. 

>[!NOTE]
>Aucune stratégie n’est requis pour le mappage de l’étendue de la zone interne par défaut. 

    Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,208.84.0.53" -ZoneScope "external,1" -ZoneName contoso.com

>[!NOTE]
>208.84.0.53 Est l’adresse IP sur l’interface de réseau public.

Pour plus d’informations, voir [Add-DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx).

À présent le serveur DNS est configuré avec les stratégies DNS requis pour un serveur de nom de «split brain» avec un annuaire ActiveDirectory intégré DNS zone.

Vous pouvez créer des milliers de stratégies DNS en fonction de votre trafic en matière de gestion, et toutes les nouvelles stratégies sont appliquées dynamiquement, sans redémarrer le serveur DNS - dans les requêtes entrantes. 
