---
title: Réponses DNS basées sur l’heure du jour avec un serveur d’applications Azure Cloud
description: Cette rubrique fait partie de DNS stratégie scénario Guide pour Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 4846b548-8fbc-4a7f-af13-09e834acdec0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 68f30973ef58b64006181990425e6ca84c39c059
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812040"
---
# <a name="dns-responses-based-on-time-of-day-with-an-azure-cloud-app-server"></a>Réponses DNS basées sur l’heure du jour avec un serveur d’applications Azure Cloud

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour savoir comment distribuer le trafic d’application entre différentes instances distribuées d’une application à l’aide de stratégies DNS qui sont basés sur l’heure du jour. 

Ce scénario est utile dans les situations où vous souhaitez diriger le trafic dans un fuseau horaire vers les serveurs d’applications de remplacement, tels que les serveurs Web qui sont hébergés sur Microsoft Azure, qui sont trouvent dans un autre fuseau horaire. Cela vous permet de répartir le trafic entre les instances de l’application pendant le pic périodes lorsque vos serveurs principaux sont surchargées avec le trafic. 

> [!NOTE]
> Pour savoir comment utiliser une stratégie DNS pour les réponses DNS intelligentes sans l’utilisation d’Azure, consultez [utiliser une stratégie DNS pour Intelligent DNS réponses selon l’heure de la journée](Scenario--Use-DNS-Policy-for-Intelligent-DNS-Responses-Based-on-the-Time-of-Day.md). 

## <a name="example-of-intelligent-dns-responses-based-on-the-time-of-day-with-azure-cloud-app-server"></a>Exemple de réponses DNS intelligentes basées sur l’heure du jour avec le serveur d’applications Cloud Azure

Voici un exemple de comment vous pouvez utiliser une stratégie DNS pour équilibrer le trafic d’application basée sur l’heure de la journée.

Cet exemple utilise une société fictive, Contoso cadeau Services, qui offre des solutions de donation de tels en ligne dans le monde entier via son site Web, contosogiftservices.com. 

Le site web de contosogiftservices.com est hébergé uniquement à un seul centre de données local à Seattle (avec l’adresse IP publique 192.68.30.2). 

Le serveur DNS se trouve également dans le centre de données sur site. 

Avec une hausse du nombre récent dans l’entreprise, contosogiftservices.com a un nombre plus élevé de visiteurs tous les jours, et certains clients ont signalé des problèmes de disponibilité de service. 

Contoso cadeau Services effectue une analyse de site et découvre que tous les soirs entre 18 h 00 et 21 h 00 heure locale, il existe une forte hausse dans le trafic vers le serveur Web de Seattle. Le serveur Web ne peut pas mettre à l’échelle pour gérer l’augmentation du trafic sur ces heures de pointe, ce qui entraîne un déni de service aux clients. 

Pour vous assurer que les clients contosogiftservices.com une expérience réactive à partir du site Web, Services cadeau de Contoso décide que pendant les heures, il sera louer une machine virtuelle \(machine virtuelle\) sur Microsoft Azure pour héberger une copie de son serveur Web .  

Contoso cadeau Services Obtient une adresse IP publique à partir d’Azure pour la machine virtuelle (192.68.31.44) et développe l’automatisation pour déployer le serveur Web quotidiennes sur Azure entre 17-10 h 00, permettant ainsi une période d’urgence d’une heure.

> [!NOTE]
> Pour plus d’informations sur les machines virtuelles Azure, consultez [documentation Machines virtuelles](https://azure.microsoft.com/documentation/services/virtual-machines/) 

Les serveurs DNS sont configurés avec des étendues de zone et les stratégies DNS afin qu’entre 5-9 h tous les jours, 30 % des requêtes sont envoyées à l’instance du serveur Web qui s’exécute dans Azure.

L’illustration suivante représente ce scénario.

![Stratégie DNS pour le temps de réponses du jour](../../media/DNS-Policy-Tod2/dns_policy_tod2.jpg)  

## <a name="how-intelligent-dns-responses-based-on-time-of-day-with-azure-app-server-works"></a>Comment les réponses DNS intelligentes basées sur l’heure du jour avec Azure du serveur d’applications fonctionne
 
Cet article explique comment configurer le serveur DNS pour répondre aux requêtes DNS avec les adresses IP du serveur d’application deux - un serveur web est à Seattle et l’autre est dans un centre de données Azure.

Après la configuration d’une nouvelle stratégie DNS est basée sur les heures de pointe de 18 h 00 à 9 h à Seattle, le serveur DNS envoie les 70 % des réponses DNS pour les clients contenant l’adresse IP du serveur Web de Seattle et trente pour cent des réponses DNS pour clie nts contenant l’adresse IP du serveur Web Microsoft Azure ainsi diriger le trafic client vers le nouveau serveur Web Microsoft Azure et empêche la surcharge du serveur Web de Seattle. 

Toutes les autres heures de la journée, le traitement des requêtes normales a lieu et les réponses sont renvoyées à partir de l’étendue de la zone par défaut qui contient un enregistrement pour le serveur web du centre de données sur site. 

La durée de vie de 10 minutes sur l’enregistrement Azure garantit que l’enregistrement a expiré à partir du cache LDNS avant la suppression de la machine virtuelle à partir d’Azure. Un des avantages de ce type de mise à l’échelle est que vous pouvez conserver votre DNS données localement et conserverez montée sur Azure nécessite à la demande.

## <a name="how-to-configure-dns-policy-for-intelligent-dns-responses-based-on-time-of-day-with-azure-app-server"></a>Comment configurer une stratégie DNS pour les réponses DNS intelligentes basées sur l’heure du jour avec le serveur d’applications Azure

Pour configurer une stratégie DNS pour les réponses aux requêtes temporel jour application d’équilibrage de charge, vous devez effectuer les étapes suivantes.

- [Créer des étendues de la Zone](#create-the-zone-scopes)
- [Ajoutez des enregistrements dans les étendues de Zone](#add-records-to-the-zone-scopes)
- [Créer les stratégies DNS](#create-the-dns-policies)

> [!NOTE]
> Vous devez effectuer ces étapes sur le serveur DNS faisant autorité pour la zone que vous souhaitez configurer. L’appartenance au groupe Administrateurs, ou équivalent, est requis pour effectuer les procédures suivantes. 

Les sections suivantes fournissent des instructions de configuration détaillées.

> [!IMPORTANT]
> Les sections suivantes incluent des exemples de commandes Windows PowerShell qui contiennent des exemples de valeurs de paramètres. Veillez à remplacer les exemples de valeurs dans ces commandes avec les valeurs appropriées pour votre déploiement avant d’exécuter ces commandes. 


### <a name="create-the-zone-scopes"></a>Créer des étendues de la Zone

Une étendue de la zone est une instance unique de la zone. Une zone DNS peut avoir plusieurs étendues de zone, avec chaque étendue de la zone contenant son propre ensemble d’enregistrements DNS. Le même enregistrement peut être présent dans plusieurs étendues, avec différentes adresses IP ou les mêmes adresses IP. 

> [!NOTE]
> Par défaut, une étendue de la zone existe sur les zones DNS. Cette étendue de la zone a le même nom que la zone, et des opérations DNS héritées travailler sur cette étendue. 

Vous pouvez utiliser la commande suivante pour créer une étendue de la zone pour héberger les enregistrements d’Azure.

```
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AzureZoneScope"
```

Pour plus d’informations, consultez [DnsServerZoneScope-ajouter](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

### <a name="add-records-to-the-zone-scopes"></a>Ajoutez des enregistrements dans les étendues de Zone
L’étape suivante consiste à ajouter les enregistrements représentant l’hôte du serveur Web dans les étendues de zone. 

Dans AzureZoneScope, l’enregistrement www.contosogiftservices.com est ajouté avec l’adresse IP 192.68.31.44, qui se trouve dans le cloud public Azure. 

De même, dans l’étendue de la zone par défaut \(contosogiftservices.com\), un enregistrement \(www.contosogiftservices.com\) est ajouté avec l’adresse IP 192.68.30.2 du serveur Web en cours d’exécution dans le Seattle en local Centre de données.

Dans la deuxième applet de commande ci-dessous, le paramètre – ZonePortée n’est pas inclus. Pour cette raison, les enregistrements sont ajoutés dans la valeur par défaut ZonePortée. 

En outre, la durée de vie de l’enregistrement pour les machines virtuelles Azure est conservée à 600 s (10 minutes) afin que les LDNS ne pas mettre en cache il pendant une période plus longue, ce qui entraînerait une interférence avec l’équilibrage de charge. En outre, les machines virtuelles Azure sont disponibles pour 1 heure supplémentaire sous la forme d’un plan d’urgence pour vous assurer que même les clients avec les enregistrements mis en cache sont en mesure de résoudre.

```
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.31.44" -ZoneScope "AzureZoneScope" –TimeToLive 600

Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.30.2"
```

Pour plus d’informations, consultez [Add-DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).  

### <a name="create-the-dns-policies"></a>Créer les stratégies DNS 
Une fois que les étendues de zone sont créés, vous pouvez créer des stratégies DNS qui distribuent les requêtes entrantes entre ces étendues afin que le processus suivant a lieu.

1. À partir de 18 h 00 à 21 h 00 tous les jours, 30 % des clients recevoir l’adresse IP du serveur Web du centre de données Azure dans la réponse DNS, tandis que 70 % des clients reçoivent l’adresse IP du serveur Web sur site Seattle.
2. Toutes les autres fois, tous les clients recevoir l’adresse IP du serveur Web sur site Seattle.

L’heure de la journée doit être exprimée en heure locale du serveur DNS.

Vous pouvez utiliser la commande suivante pour créer la stratégie DNS.

```
Add-DnsServerQueryResolutionPolicy -Name "Contoso6To9Policy" -Action ALLOW -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” -ZoneName "contosogiftservices.com" –ProcessingOrder 1
```

Pour plus d’informations, consultez [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Désormais, le serveur DNS est configuré avec les stratégies DNS requises pour rediriger le trafic vers le serveur Web Microsoft Azure en fonction de l’heure de la journée. 

Notez l’expression :

`
 -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” 
`

Cette expression configure le serveur DNS avec une combinaison de ZonePortée et poids qui indique au serveur DNS pour envoyer l’adresse IP du serveur Web de Seattle 70 % du temps, lors de l’envoi de l’adresse IP du serveur Web de Azure trente pour cent du temps.

Vous pouvez créer des milliers de stratégies DNS, en fonction de votre trafic en matière de gestion, et toutes les nouvelles stratégies sont appliquées dynamiquement, sans avoir à redémarrer le serveur DNS : sur les requêtes entrantes.
