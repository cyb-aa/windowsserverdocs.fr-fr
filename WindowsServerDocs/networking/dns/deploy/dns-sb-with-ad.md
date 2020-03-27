---
title: Utiliser une stratégie DNS pour un déploiement DNS Split-Brain dans Active Directory
description: Vous pouvez utiliser cette rubrique pour tirer parti des fonctionnalités de gestion du trafic des stratégies DNS pour les déploiements de fractionnement Brain avec Active Directory zones DNS intégrées dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9533204-ad7e-4e49-81c1-559324a16aeb
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 8bb95412702fed5b7f809e3e699b0ab0a556455c
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317856"
---
# <a name="use-dns-policy-for-split-brain-dns-in-active-directory"></a>Utiliser une stratégie DNS pour un déploiement DNS Split-Brain dans Active Directory

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour tirer parti des fonctionnalités de gestion du trafic des stratégies DNS pour les déploiements de fractionnement\-Brain avec Active Directory zones DNS intégrées dans Windows Server 2016.

Dans Windows Server 2016, la prise en charge des stratégies DNS est étendue à Active Directory zones DNS intégrées. L’intégration de Active Directory offre des fonctionnalités de haute disponibilité maître à plusieurs\-au serveur DNS. 

Auparavant, ce scénario nécessitait que les administrateurs DNS maintiennent deux serveurs DNS différents, chacun fournissant des services à chaque ensemble d’utilisateurs, interne et externe. Si seuls quelques enregistrements à l’intérieur de la zone étaient fractionnés\-cerveaud ou si les deux instances de la zone (interne et externe) étaient déléguées au même domaine parent, cela devenait un énigme de gestion.

> [!NOTE]
> - Les déploiements DNS sont répartis\-cerveau lorsqu’il existe deux versions d’une seule zone, une version pour les utilisateurs internes sur l’intranet de l’organisation et une version pour les utilisateurs externes, qui sont généralement des utilisateurs sur Internet.
> - La rubrique [utiliser une stratégie DNS pour le déploiement DNS split-brain](split-brain-DNS-deployment.md) explique comment utiliser des stratégies DNS et des étendues de zone pour déployer un système DNS de fractionnement\-Brain sur un serveur DNS Windows Server 2016 unique.



##  <a name="example-split-brain-dns-in-active-directory"></a>Exemple de fractionnement du DNS du cerveau\-dans Active Directory

Cet exemple utilise une société fictive contoso, qui gère un site Web de carrière sur www.career.contoso.com.

Le site comporte deux versions, une pour les utilisateurs internes où des publications de travaux internes sont disponibles. Ce site interne est disponible à l’adresse IP locale 10.0.0.39. 

La deuxième version est la version publique du même site, disponible à l’adresse IP publique 65.55.39.10.

En l’absence de stratégie DNS, l’administrateur doit héberger ces deux zones sur des serveurs DNS Windows Server distincts et les gérer séparément. 

À l’aide de stratégies DNS, ces zones peuvent désormais être hébergées sur le même serveur DNS.

Si le serveur DNS pour contoso.com est Active Directory intégré et qu’il est à l’écoute de deux interfaces réseau, l’administrateur DNS contoso peut suivre les étapes de cette rubrique pour réaliser un déploiement\-Brain Split.

L’administrateur DNS configure les interfaces du serveur DNS avec les adresses IP suivantes.

- La carte réseau Internet est configurée avec une adresse IP publique de 208.84.0.53 pour les requêtes externes.
- La carte réseau intranet est configurée avec une adresse IP privée de 10.0.0.56 pour les requêtes internes.

L’illustration suivante représente ce scénario.

![Déploiement du DNS intégré à AD split-brain](../../media/DNS-SB-AD/DNS-SB-AD.jpg)

## <a name="how-dns-policy-for-split-brain-dns-in-active-directory-works"></a>Fonctionnement de la stratégie DNS pour le\-Split DNS du cerveau dans Active Directory

Lorsque le serveur DNS est configuré avec les stratégies DNS requises, chaque demande de résolution de nom est évaluée par rapport aux stratégies sur le serveur DNS.

L’interface serveur est utilisée dans cet exemple comme critère pour faire la distinction entre les clients internes et externes.

Si l’interface de serveur sur laquelle la requête est reçue correspond à l’une des stratégies, l’étendue de zone associée est utilisée pour répondre à la requête. 

Ainsi, dans notre exemple, les requêtes DNS pour www.career.contoso.com reçues sur l’adresse IP privée (10.0.0.56) reçoivent une réponse DNS qui contient une adresse IP interne ; et les requêtes DNS reçues sur l’interface réseau publique reçoivent une réponse DNS qui contient l’adresse IP publique dans l’étendue de la zone par défaut (cela est identique à la résolution de requête normale).  

La prise en charge des mises à jour et du nettoyage du DNS dynamique \(DDNS\) est prise en charge uniquement sur l’étendue de la zone par défaut. Étant donné que les clients internes sont desservis par l’étendue de zone par défaut, les administrateurs DNS contoso peuvent continuer à utiliser les mécanismes existants (DNS dynamique ou statique) pour mettre à jour les enregistrements dans contoso.com. Pour les étendues de zone par défaut non\-\(telles que l’étendue externe dans cet exemple\), la prise en charge DDNS ou de nettoyage n’est pas disponible.

### <a name="high-availability-of-policies"></a>Haute disponibilité des stratégies

Les stratégies DNS ne sont pas Active Directory intégrées. Pour cette raison, les stratégies DNS ne sont pas répliquées sur les autres serveurs DNS qui hébergent la même zone intégrée Active Directory. 

Les stratégies DNS sont stockées sur le serveur DNS local. Vous pouvez facilement exporter des stratégies DNS d’un serveur vers un autre à l’aide de l’exemple de commandes Windows PowerShell suivantes.

    $policies = Get-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server01
    
    $policies |  Add-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server02

Pour plus d’informations, consultez les rubriques de référence Windows PowerShell suivantes.

- [DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/get-dnsserverqueryresolutionpolicy?view=win10-ps)
- [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)


## <a name="how-to-configure-dns-policy-for-split-brain-dns-in-active-directory"></a>Comment configurer la stratégie DNS pour le\-Split DNS Brain dans Active Directory

Pour configurer le déploiement de Split-Brain DNS à l’aide d’une stratégie DNS, vous devez utiliser les sections suivantes, qui fournissent des instructions de configuration détaillées.

### <a name="add-the-active-directory-integrated-zone"></a>Ajouter la zone intégrée Active Directory

Vous pouvez utiliser l’exemple de commande suivant pour ajouter la Active Directory zone contoso.com intégrée au serveur DNS.

    Add-DnsServerPrimaryZone -Name "contoso.com" -ReplicationScope "Domain" -PassThru

Pour plus d’informations, consultez [Add-DnsServerPrimaryZone](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverprimaryzone?view=win10-ps).

### <a name="create-the-scopes-of-the-zone"></a>Créer les étendues de la zone

Vous pouvez utiliser cette section pour partitionner la zone contoso.com pour créer une étendue de zone externe.

Une étendue de zone est une instance unique de la zone. Une zone DNS peut avoir plusieurs étendues de zone, chaque étendue contenant son propre ensemble d’enregistrements DNS. Le même enregistrement peut être présent dans plusieurs étendues, avec des adresses IP différentes ou les mêmes adresses IP. 

Étant donné que vous ajoutez cette nouvelle étendue de zone dans une zone intégrée Active Directory, l’étendue de zone et les enregistrements qu’elle contient seront répliqués via Active Directory vers d’autres serveurs de réplication du domaine.

Par défaut, une étendue de zone existe dans chaque zone DNS. Cette étendue de zone porte le même nom que la zone, et les opérations DNS héritées fonctionnent sur cette étendue. Cette étendue de zone par défaut hébergera la version interne de www.career.contoso.com.

Vous pouvez utiliser l’exemple de commande suivant pour créer l’étendue de zone sur le serveur DNS.

    Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "external"

Pour plus d’informations, consultez [Add-DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).

### <a name="add-records-to-the-zone-scopes"></a>Ajouter des enregistrements aux étendues de zone

L’étape suivante consiste à ajouter les enregistrements qui représentent l’hôte du serveur Web dans les deux étendues de zone : External et default \(pour les clients internes\). 

Dans l’étendue de la zone interne par défaut, l’enregistrement www.career.contoso.com est ajouté avec l’adresse IP 10.0.0.39, qui est une adresse IP privée. et dans l’étendue de la zone externe, le même enregistrement \(www.career.contoso.com\) est ajouté avec l’adresse IP publique 65.55.39.10. 

Les enregistrements \(à la fois dans l’étendue de zone interne par défaut et dans l’étendue de la zone externe\) automatiquement répliqués sur le domaine avec leurs étendues de zone respectives.

Vous pouvez utiliser l’exemple de commande suivant pour ajouter des enregistrements aux étendues de zone sur le serveur DNS.

    Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10" -ZoneScope "external"
    
    Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39”

> [!NOTE]
> Le paramètre **– ZoneScope** n’est pas inclus lorsque l’enregistrement est ajouté à l’étendue de la zone par défaut. Cette action est identique à l’ajout d’enregistrements à une zone normale.

Pour plus d’informations, consultez [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

### <a name="create-the-dns-policies"></a>Créer les stratégies DNS

Une fois que vous avez identifié les interfaces de serveur pour le réseau externe et le réseau interne et que vous avez créé les étendues de zone, vous devez créer des stratégies DNS qui connectent les étendues de zone interne et externe.

> [!NOTE]
> Cet exemple utilise l’interface de serveur \(le paramètre-ServerInterface dans l’exemple de commande ci-dessous\) en tant que critères pour faire la distinction entre les clients internes et externes. Une autre méthode pour différencier les clients externes et internes consiste à utiliser des sous-réseaux clients en tant que critères. Si vous pouvez identifier les sous-réseaux auxquels appartiennent les clients internes, vous pouvez configurer la stratégie DNS pour la différencier en fonction du sous-réseau client. Pour plus d’informations sur la configuration de la gestion du trafic à l’aide de critères de sous-réseau client, consultez [utiliser une stratégie DNS pour la gestion du trafic basée sur la géolocalisation avec les serveurs principaux](primary-geo-location.md).

Une fois que vous avez configuré des stratégies, lorsqu’une requête DNS est reçue sur l’interface publique, la réponse est retournée à partir de la portée externe de la zone. 

> [!NOTE]
> Aucune stratégie n’est requise pour le mappage de l’étendue de zone interne par défaut. 

    Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,208.84.0.53" -ZoneScope "external,1" -ZoneName contoso.com

> [!NOTE]
> 208.84.0.53 est l’adresse IP sur l’interface réseau publique.

Pour plus d’informations, consultez [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).

À présent, le serveur DNS est configuré avec les stratégies DNS requises pour un serveur de noms split-brain avec une zone DNS intégrée Active Directory.

Vous pouvez créer des milliers de stratégies DNS en fonction de vos besoins en matière de gestion du trafic, et toutes les nouvelles stratégies sont appliquées de manière dynamique, sans redémarrer le serveur DNS-sur les requêtes entrantes. 
