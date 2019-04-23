---
title: Utiliser une stratégie DNS pour un déploiement DNS Split-Brain dans Active Directory
description: Vous pouvez utiliser cette rubrique pour tirer parti du trafic des fonctionnalités de gestion des stratégies DNS pour les déploiements « split brain » avec Active Directory intégré des zones DNS dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9533204-ad7e-4e49-81c1-559324a16aeb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7a5761cafff0a4bf148958a7f14aeaf311075b2e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839780"
---
# <a name="use-dns-policy-for-split-brain-dns-in-active-directory"></a>Utiliser une stratégie DNS pour un déploiement DNS Split-Brain dans Active Directory

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour exploiter les fonctionnalités de gestion du trafic DNS des stratégies de fractionnement\-déploiements cerveau avec Active Directory intégré des zones DNS dans Windows Server 2016.

Dans Windows Server 2016, la prise en charge des stratégies DNS est étendu à Active Directory les zones DNS intégrées. Intégration Active Directory fournit plusieurs\-maîtriser les fonctionnalités de haute disponibilité pour le serveur DNS. 

Auparavant, ce scénario nécessitait que les administrateurs DNS maintiennent deux serveurs DNS différents, chaque fournissant des services à chaque ensemble d’utilisateurs, internes et externes. Si seulement quelques enregistrements à l’intérieur de la zone ont été divisées\-brained ou les deux instances de la zone (interne et externe) ont été déléguées au même domaine parent, c’est devenu une énigme de gestion.

>[!NOTE]
> - Déploiements de DNS sont fractionnées\-du cerveau lorsqu’il existe deux versions d’une zone unique, une version pour les utilisateurs internes sur l’intranet de l’organisation et une version pour les utilisateurs externes – qui sont en règle générale, les utilisateurs sur Internet.
> - La rubrique [utiliser une stratégie DNS pour le déploiement de DNS Split-Brain](split-brain-DNS-deployment.md) explique comment vous pouvez utiliser des stratégies DNS et des étendues de zone pour déployer un fractionnement\-système DNS sur un seul serveur DNS de Windows Server 2016 du cerveau.



##  <a name="example-split-brain-dns-in-active-directory"></a>Fractionnement de l’exemple\-du cerveau DNS dans Active Directory

Cet exemple utilise une société fictive, Contoso, qui gère un site Web de votre carrière à www.career.contoso.com.

Le site a deux versions, un pour les utilisateurs internes où les validations de travail internes sont disponibles. Ce site interne est disponible à l’adresse IP locale 10.0.0.39. 

La deuxième version est la version publique du même site, qui est disponible à l’adresse IP publique 65.55.39.10.

En l’absence d’une stratégie DNS, l’administrateur est nécessaire pour héberger ces deux zones sur des serveurs DNS de Windows Server distincts et de les gérer séparément. 

À l’aide de stratégies DNS ces zones peuvent maintenant être hébergées sur le même serveur DNS.

Si le serveur DNS pour contoso.com est intégrée à Active Directory et est à l’écoute sur deux interfaces réseau, l’administrateur DNS de Contoso peut suivre les étapes décrites dans cette rubrique pour obtenir un fractionnement\-déploiement du cerveau.

L’administrateur DNS configure les interfaces de serveur DNS avec les adresses IP suivantes.

- L’adaptateur de réseau accessibles sur Internet est configuré avec une adresse IP publique de 208.84.0.53 pour les requêtes externes.
- La carte réseau de la tournée vers Intranet est configurée avec une adresse IP privée de 10.0.0.56 pour les requêtes internes.

L’illustration suivante représente ce scénario.

![Déploiement de DNS intégré à AD « split brain »](../../media/DNS-SB-AD/DNS-SB-AD.jpg)

## <a name="how-dns-policy-for-split-brain-dns-in-active-directory-works"></a>Comment une stratégie DNS pour fractionner\-du cerveau DNS dans Active Directory fonctionne

Lorsque le serveur DNS est configuré avec les stratégies DNS requises, chaque demande de résolution de nom est recherchée dans les stratégies sur le serveur DNS.

L’Interface du serveur est utilisé dans cet exemple comme critère de faire la distinction entre les clients internes et externes.

Si l’interface du serveur sur lequel la requête est reçue correspond à aucune des stratégies, l’étendue de la zone associée est utilisée pour répondre à la requête. 

Par conséquent, dans notre exemple, les requêtes DNS pour www.career.contoso.com qui sont reçus sur l’adresse IP privée (10.0.0.56) recevoir une réponse DNS qui contient une adresse IP interne ; et les requêtes DNS qui sont reçus sur l’interface de réseau public recevoir une réponse DNS qui contient l’adresse IP publique dans l’étendue de la zone par défaut (cela est identique à la résolution de requête normale).  

Prise en charge de DNS dynamique \(DDNS\) mises à jour et le nettoyage est pris en charge uniquement sur l’étendue de la zone par défaut. Étant donné que les clients internes sont traitées par l’étendue de la zone par défaut, les administrateurs de DNS de Contoso peuvent continuer à l’aide de mécanismes existants (dynamique DNS ou statique) pour mettre à jour les enregistrements dans contoso.com. Pour non\-par défaut étendues zone \(comme l’étendue externe dans cet exemple\), DDNS ou prise en charge le nettoyage n’est pas disponible.

### <a name="high-availability-of-policies"></a>Haute disponibilité des stratégies

Stratégies DNS ne sont pas intégrées à Active Directory. Pour cette raison, les stratégies DNS ne sont pas répliquées vers les autres serveurs DNS qui hébergent la même zone intégrée à Active Directory. 

Stratégies DNS sont stockées sur le serveur DNS local. Vous pouvez facilement exporter les stratégies DNS d’un serveur vers un autre à l’aide de l’exemple de commande Windows PowerShell suivant.

    $policies = Get-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server01
    
    $policies |  Add-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server02

Pour plus d’informations, consultez les rubriques de référence Windows PowerShell suivantes.

- [Get-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/get-dnsserverqueryresolutionpolicy?view=win10-ps)
- [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps)


## <a name="how-to-configure-dns-policy-for-split-brain-dns-in-active-directory"></a>Comment configurer une stratégie DNS pour le fractionnement\-du cerveau DNS dans Active Directory

Pour configurer le déploiement de Split-Brain DNS à l’aide d’une stratégie DNS, vous devez utiliser les sections suivantes, qui fournissent des instructions de configuration détaillées.

### <a name="add-the-active-directory-integrated-zone"></a>Ajouter la zone intégrée à Active Directory

Vous pouvez utiliser la commande suivante pour ajouter la zone contoso.com intégrée Active Directory vers le serveur DNS.

    Add-DnsServerPrimaryZone -Name "contoso.com" -ReplicationScope "Domain" -PassThru

Pour plus d’informations, consultez [afin d’illustrer](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverprimaryzone?view=win10-ps).

### <a name="create-the-scopes-of-the-zone"></a>Créer les étendues de la Zone

Vous pouvez utiliser cette section pour partitionner la zone contoso.com pour créer une étendue de la zone externe.

Une étendue de la zone est une instance unique de la zone. Une zone DNS peut avoir plusieurs étendues de zone, avec chaque étendue de la zone contenant son propre ensemble d’enregistrements DNS. Le même enregistrement peut être présent dans plusieurs étendues, avec différentes adresses IP ou les mêmes adresses IP. 

Étant donné que vous ajoutez cette étendue de la nouvelle zone dans une zone intégrée à Active Directory, l’étendue de la zone et les enregistrements qu’il contient seront répliquées via Active Directory à d’autres serveurs de réplica dans le domaine.

Par défaut, une étendue de la zone existe dans chaque zone DNS. Cette étendue de la zone a le même nom que la zone, et des opérations DNS héritées travailler sur cette étendue. Cette étendue de la zone par défaut va héberger la version interne de www.career.contoso.com.

Vous pouvez utiliser la commande suivante pour créer l’étendue de la zone sur le serveur DNS.

    Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "external"

Pour plus d’informations, consultez [Add-DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).

### <a name="add-records-to-the-zone-scopes"></a>Ajoutez des enregistrements dans les étendues de Zone

L’étape suivante consiste à ajouter les enregistrements représentant l’hôte du serveur web dans les deux étendues externes de zone et par défaut \(pour les clients internes\). 

Dans l’étendue de la zone interne par défaut, l’enregistrement www.career.contoso.com est ajoutée avec l’adresse IP 10.0.0.39, qui est une adresse IP privée ; et dans l’étendue de la zone externe, le même enregistrement \(www.career.contoso.com\) est ajouté avec l’adresse IP publique 65.55.39.10. 

Les enregistrements \(à la fois dans la valeur par défaut interne de la zone étendue et l’étendue de la zone externe\) vont être répliqués automatiquement entre le domaine avec leurs étendues de la zone concernée.

Vous pouvez utiliser la commande suivante pour ajouter des enregistrements pour les étendues de la zone sur le serveur DNS.

    Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10" -ZoneScope "external"
    
    Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39”

>[!NOTE]
>Le **– ZonePortée** paramètre n’est pas inclus lorsque l’enregistrement est ajouté à l’étendue de la zone par défaut. Cette action est la même que l’ajout d’enregistrements à une zone normale.

Pour plus d’informations, consultez [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

### <a name="create-the-dns-policies"></a>Créer les stratégies DNS

Une fois que vous avez identifié les interfaces de serveur pour le réseau externe et le réseau interne et que vous avez créé les étendues de zone, vous devez créer des stratégies DNS qui se connectent les étendues de zone internes et externes.

>[!NOTE]
>Cet exemple utilise l’interface du serveur \(le paramètre - ServerInterface dans l’exemple de commande ci-dessous\) comme critère pour faire la distinction entre les clients internes et externes. Une autre méthode permettant de faire la distinction entre les clients internes et externes est à l’aide de sous-réseaux du client comme un critère. Si vous pouvez identifier les sous-réseaux auxquels appartiennent les clients internes, vous pouvez configurer une stratégie DNS pour différencier en fonction de sous-réseau du client. Pour plus d’informations sur la façon de configurer la gestion du trafic à l’aide de critères de sous-réseau client, consultez [utiliser une stratégie DNS pour l’emplacement géographique en fonction de la gestion du trafic avec des serveurs principaux](primary-geo-location.md).

Après avoir configuré des stratégies, lorsqu’une requête DNS est reçue sur l’interface publique, la réponse est retournée à partir de la portée externe de la zone. 

>[!NOTE]
>Aucune stratégie n’est requis pour le mappage de l’étendue de la zone interne par défaut. 

    Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,208.84.0.53" -ZoneScope "external,1" -ZoneName contoso.com

>[!NOTE]
>208.84.0.53 est l’adresse IP sur l’interface de réseau public.

Pour plus d’informations, consultez [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).

Maintenant le serveur DNS est configuré avec les stratégies DNS requises pour un serveur de noms « split brain » avec un annuaire Active Directory intégré DNS zone.

Vous pouvez créer des milliers de stratégies DNS, en fonction de votre trafic en matière de gestion, et toutes les nouvelles stratégies sont appliquées dynamiquement, sans avoir à redémarrer le serveur DNS : sur les requêtes entrantes. 
