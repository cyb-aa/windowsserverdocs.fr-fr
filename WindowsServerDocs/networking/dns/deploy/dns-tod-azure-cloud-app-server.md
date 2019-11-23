---
title: Réponses DNS basées sur l’heure du jour avec un serveur d’applications Azure Cloud
description: Cette rubrique fait partie du Guide de scénario de stratégie DNS pour Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: 4846b548-8fbc-4a7f-af13-09e834acdec0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4307ce1512980277af819e0710e0447d8dbac8c4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406196"
---
# <a name="dns-responses-based-on-time-of-day-with-an-azure-cloud-app-server"></a>Réponses DNS basées sur l’heure du jour avec un serveur d’applications Azure Cloud

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour savoir comment distribuer le trafic d’application sur différentes instances géographiquement distribuées d’une application à l’aide de stratégies DNS basées sur l’heure de la journée. 

Ce scénario est utile dans les situations où vous souhaitez diriger le trafic dans un fuseau horaire vers d’autres serveurs d’applications, tels que les serveurs Web hébergés sur des Microsoft Azure, qui se trouvent dans un autre fuseau horaire. Cela vous permet d’équilibrer la charge du trafic entre les instances d’application pendant les périodes de pointe lorsque vos serveurs principaux sont surchargés avec le trafic. 

> [!NOTE]
> Pour savoir comment utiliser une stratégie DNS pour les réponses DNS intelligentes sans utiliser Azure, consultez [utiliser une stratégie DNS pour les réponses DNS intelligentes en fonction de l’heure de la journée](Scenario--Use-DNS-Policy-for-Intelligent-DNS-Responses-Based-on-the-Time-of-Day.md). 

## <a name="example-of-intelligent-dns-responses-based-on-the-time-of-day-with-azure-cloud-app-server"></a>Exemple de réponses DNS intelligentes en fonction de l’heure de la journée avec Azure Cloud App Server

Vous trouverez ci-dessous un exemple de la façon dont vous pouvez utiliser la stratégie DNS pour équilibrer le trafic d’application en fonction de l’heure de la journée.

Cet exemple utilise une société fictive, contoso Gift services, qui fournit des solutions de don en ligne dans le monde entier via son site Web, contosogiftservices.com. 

Le site Web contosogiftservices.com est hébergé uniquement sur un centre de donnée local à Seattle (avec adresse IP publique 192.68.30.2). 

Le serveur DNS est également situé dans le centre de centres local. 

Avec une récente augmentation des activités, contosogiftservices.com a un plus grand nombre de visiteurs chaque jour et certains clients ont signalé des problèmes de disponibilité des services. 

Contoso Gift services effectue une analyse de site et découvre que chaque soir entre 18 h 00 et 9 h 00 heure locale, il y a un accroissement du trafic vers le serveur Web de Seattle. Le serveur Web ne peut pas se mettre à l’échelle pour gérer l’augmentation du trafic à ces heures de pointe, ce qui entraîne un déni de service pour les clients. 

Pour vous assurer que les clients contosogiftservices.com bénéficient d’une expérience réactive à partir du site Web, contoso Gift services décide qu’au cours de ces heures, il loue un ordinateur virtuel \(machine virtuelle\) sur Microsoft Azure pour héberger une copie de son serveur Web.  

Contoso Gift services obtient une adresse IP publique à partir d’Azure pour la machine virtuelle (192.68.31.44) et développe l’automatisation pour déployer le serveur Web chaque jour sur Azure entre 5-10 PM, ce qui permet une période d’urgence d’une heure.

> [!NOTE]
> Pour plus d’informations sur les machines virtuelles Azure, consultez [la documentation relative aux machines virtuelles](https://azure.microsoft.com/documentation/services/virtual-machines/) 

Les serveurs DNS sont configurés avec des étendues de zone et des stratégies DNS de sorte qu’entre 5-9 PM tous les jours, 30% des requêtes sont envoyées à l’instance du serveur Web qui s’exécute dans Azure.

L’illustration suivante représente ce scénario.

![Stratégie DNS pour les réponses horaires de la journée](../../media/DNS-Policy-Tod2/dns_policy_tod2.jpg)  

## <a name="how-intelligent-dns-responses-based-on-time-of-day-with-azure-app-server-works"></a>Comment les réponses DNS intelligentes basées sur l’heure de la journée avec Azure App Server fonctionnent
 
Cet article explique comment configurer le serveur DNS pour répondre aux requêtes DNS avec deux adresses IP de serveur d’applications différentes : un serveur Web se trouve à Seattle et l’autre dans un centre de informations Azure.

Après la configuration d’une nouvelle stratégie DNS qui est basée sur les heures de pointe de 6 h à 9 h 00 à Seattle, le serveur DNS envoie 70% des réponses DNS aux clients contenant l’adresse IP du serveur Web de Seattle, et trente par cent des réponses DNS à client NTS contenant l’adresse IP du serveur Web Azure, ce qui permet de diriger le trafic client vers le nouveau serveur Web Azure et d’empêcher le serveur Web de Seattle de devenir surchargé. 

À tous les autres moments de la journée, le traitement normal des requêtes a lieu et les réponses sont envoyées à partir de l’étendue de la zone par défaut qui contient un enregistrement pour le serveur Web dans le centre de centres local. 

La durée de vie de 10 minutes sur l’enregistrement Azure garantit que l’enregistrement est arrivé à expiration du cache LDNS avant la suppression de la machine virtuelle d’Azure. L’un des avantages de cette mise à l’échelle est que vous pouvez conserver vos données DNS localement et faire évoluer la mise à l’échelle vers Azure à mesure que la demande l’exige.

## <a name="how-to-configure-dns-policy-for-intelligent-dns-responses-based-on-time-of-day-with-azure-app-server"></a>Comment configurer la stratégie DNS pour les réponses DNS intelligentes en fonction de l’heure de la journée avec Azure App serveur

Pour configurer une stratégie DNS pour les réponses de requêtes basées sur l’équilibrage de charge des applications, vous devez effectuer les étapes suivantes.

- [Créer les étendues de zone](#create-the-zone-scopes)
- [Ajouter des enregistrements aux étendues de zone](#add-records-to-the-zone-scopes)
- [Créer les stratégies DNS](#create-the-dns-policies)

> [!NOTE]
> Vous devez effectuer ces étapes sur le serveur DNS qui fait autorité pour la zone que vous souhaitez configurer. L’appartenance à DnsAdmins, ou équivalent, est nécessaire pour effectuer les procédures suivantes. 

Les sections suivantes fournissent des instructions de configuration détaillées.

> [!IMPORTANT]
> Les sections suivantes incluent des exemples de commandes Windows PowerShell qui contiennent des exemples de valeurs pour de nombreux paramètres. Veillez à remplacer les valeurs d’exemple dans ces commandes par des valeurs appropriées pour votre déploiement avant d’exécuter ces commandes. 


### <a name="create-the-zone-scopes"></a>Créer les étendues de zone

Une étendue de zone est une instance unique de la zone. Une zone DNS peut avoir plusieurs étendues de zone, chaque étendue contenant son propre ensemble d’enregistrements DNS. Le même enregistrement peut être présent dans plusieurs étendues, avec des adresses IP différentes ou les mêmes adresses IP. 

> [!NOTE]
> Par défaut, il existe une étendue de zone sur les zones DNS. Cette étendue de zone porte le même nom que la zone, et les opérations DNS héritées fonctionnent sur cette étendue. 

Vous pouvez utiliser l’exemple de commande suivant pour créer une étendue de zone pour héberger les enregistrements Azure.

```
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AzureZoneScope"
```

Pour plus d’informations, consultez [Add-DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

### <a name="add-records-to-the-zone-scopes"></a>Ajouter des enregistrements aux étendues de zone
L’étape suivante consiste à ajouter les enregistrements qui représentent l’hôte du serveur Web dans les étendues de la zone. 

Dans AzureZoneScope, l’enregistrement www.contosogiftservices.com est ajouté avec l’adresse IP 192.68.31.44, qui se trouve dans le cloud public Azure. 

De même, dans l’étendue de zone par défaut \(contosogiftservices.com\), un enregistrement \(www.contosogiftservices.com\) est ajouté avec l’adresse IP 192.68.30.2 du serveur Web qui s’exécute dans le centre de centres local de Seattle.

Dans la deuxième applet de commande ci-dessous, le paramètre – ZoneScope n’est pas inclus. Pour cette raison, les enregistrements sont ajoutés dans le ZoneScope par défaut. 

En outre, la durée de vie de l’enregistrement pour les machines virtuelles Azure est conservée à 600s (10 minutes), de sorte que le LDNS ne le met pas en cache pour une durée plus longue, ce qui interfère avec l’équilibrage de charge. En outre, les machines virtuelles Azure sont disponibles pendant 1 heure supplémentaire pour garantir que même les clients avec des enregistrements mis en cache peuvent être résolus.

```
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.31.44" -ZoneScope "AzureZoneScope" –TimeToLive 600

Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.30.2"
```

Pour plus d’informations, consultez [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).  

### <a name="create-the-dns-policies"></a>Créer les stratégies DNS 
Une fois les étendues de zone créées, vous pouvez créer des stratégies DNS qui distribuent les requêtes entrantes sur ces étendues afin que les opérations suivantes se produisent.

1. Entre 18 h et 9 h 00, 30% des clients reçoivent l’adresse IP du serveur Web dans le centre de distribution Azure dans la réponse DNS, tandis que 70% des clients reçoivent l’adresse IP du serveur Web local Seattle.
2. À tout moment, tous les clients reçoivent l’adresse IP du serveur Web local Seattle.

L’heure de la journée doit être exprimée en heure locale du serveur DNS.

Vous pouvez utiliser l’exemple de commande suivant pour créer la stratégie DNS.

```
Add-DnsServerQueryResolutionPolicy -Name "Contoso6To9Policy" -Action ALLOW -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” -ZoneName "contosogiftservices.com" –ProcessingOrder 1
```

Pour plus d’informations, consultez [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
À présent, le serveur DNS est configuré avec les stratégies DNS requises pour rediriger le trafic vers le serveur Web Azure en fonction de l’heure de la journée. 

Notez l’expression :

`
 -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” 
`

Cette expression configure le serveur DNS à l’aide d’une combinaison de ZoneScope et de poids qui indique au serveur DNS d’envoyer l’adresse IP du serveur Web de Seattle 70 pour cent du temps, tout en envoyant l’adresse IP du serveur Web Azure trente par cent du temps.

Vous pouvez créer des milliers de stratégies DNS en fonction de vos besoins en matière de gestion du trafic, et toutes les nouvelles stratégies sont appliquées de manière dynamique, sans redémarrer le serveur DNS-sur les requêtes entrantes.
