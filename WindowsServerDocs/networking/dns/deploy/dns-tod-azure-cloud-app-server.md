---
title: Les réponses DNS basées sur l’heure du jour avec un Azure Cloud serveur d’applications
description: Cette rubrique fait partie de la DNS stratégie scénario Guide pour Windows Server2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 4846b548-8fbc-4a7f-af13-09e834acdec0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3255d3fe29f6a7dda821183fa4352964cc230a5f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="dns-responses-based-on-time-of-day-with-an-azure-cloud-app-server"></a>Les réponses DNS basées sur l’heure du jour avec un Azure Cloud serveur d’applications

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour savoir comment distribuer le trafic des applications sur différentes instances dispersés géographiquement d’une application à l’aide de stratégies DNS qui sont basées sur l’heure du jour. 

Ce scénario est utile dans les situations où vous souhaitez diriger le trafic dans un fuseau horaire pour les serveurs d’applications de remplacement, tels que les serveurs Web qui sont hébergées sur MicrosoftAzure, qui sont trouvent dans un autre fuseau horaire. Cela vous permet d’équilibrer le trafic entre les instances de l’application au cours de la pointe périodes lorsque vos serveurs principaux sont surchargées avec le trafic. 

>[!NOTE]
>Pour savoir comment utiliser une stratégie DNS pour les réponses DNS intelligentes sans l’aide d’Azure, voir [utiliser une stratégie DNS pour les réponses DNS intelligentes basées sur l’heure de la journée](Scenario--Use-DNS-Policy-for-Intelligent-DNS-Responses-Based-on-the-Time-of-Day.md). 

## <a name="bkmk_azureexample"></a>Exemple de réponses DNS intelligentes basées sur l’heure du jour avec le serveur d’applications Azure Cloud

Voici un exemple de comment vous pouvez utiliser une stratégie DNS pour équilibrer le trafic d’application basée sur l’heure de la journée.

Cet exemple utilise une société fictive, Contoso cadeau Services, qui offre des solutions donner en ligne dans le monde entier par le biais de son site Web, contosogiftservices.com. 

Le site Web contosogiftservices.com est hébergé uniquement sur un centre de données unique sur site de Seattle (avec l’adresse IP publique 192.68.30.2). 

Le serveur DNS se trouve également dans le centre de données locale. 

Avec un pic dans la récente dans l’entreprise, contosogiftservices.com a un plus grand nombre de visiteurs tous les jours, et certains clients ont signalé des problèmes de disponibilité de service. 

Contoso cadeau Services effectue une analyse de site et découvre que tous les soirs entre 18 h 00 et 21 h 00, heure locale, est un pic dans le trafic vers le serveur Web de Seattle. Le serveur Web ne peut pas mettre à l’échelle pour gérer l’augmentation du trafic à ces heures de pointe, entraînant un déni de service pour les clients. 

Pour vous assurer que les clients contosogiftservices.com une expérience réactive depuis le site Web, Services de cadeau Contoso décide que pendant les heures, qu'il sera louer une machine virtuelle de l’ordinateur \(VM\) sur MicrosoftAzure pour héberger une copie de son serveur Web.  

Contoso cadeau Services récupère une adresse IP publique dans Azure pour l’ordinateur virtuel (192.68.31.44) et développe l’automatisation pour déployer le serveur Web quotidiennement sur Azure entre 5-10 h, permettant ainsi une période d’urgence d’une heure.

>[!NOTE]
>Pour plus d’informations sur les machines virtuelles Azure, voir [documentation sur les ordinateurs virtuels](https://azure.microsoft.com/documentation/services/virtual-machines/) 

Les serveurs DNS sont configurés avec des étendues de zone et stratégies DNS afin qu’entre 5 et 9 h tous les jours, 30% des requêtes sont envoyées à l’instance du serveur Web qui s’exécute dans Azure.

L’illustration suivante décrit ce scénario.

![Stratégie DNS pour le temps de réponses par jour](../../media/DNS-Policy-Tod2/dns_policy_tod2.jpg)  

## <a name="bkmk_azurehow"></a>Comment les réponses DNS intelligentes basées sur l’heure du jour avec Azure application serveur fonctionne
 
Cet article explique comment configurer le serveur DNS pour répondre aux requêtes DNS avec les adresses IP du serveur d’application deux - un serveur web est à Seattle et l’autre est dans un centre de données Azure.

Après la configuration d’une nouvelle stratégie DNS qui est basée sur les heures de pointe de 18 h à 9 h à Seattle, le serveur DNS envoie 70% des réponses DNS pour les clients contenant l’adresse IP du serveur Web de Seattle et trente pour cent des réponses DNS pour les clients contenant l’adresse IP du serveur Web Azure, ce qui diriger le trafic du client vers le nouveau serveur Web Azure et empêche la surcharge du serveur Web de Seattle. 

Dans tous les autres heures de la journée, le traitement normal de requête a lieu et les réponses sont envoyées à partir de l’étendue par défaut, qui contient un enregistrement pour le serveur web du centre de données locale. 

La durée de vie de 10minutes sur l’enregistrement Azure garantit que l’enregistrement est arrivé à expiration à partir du cache LDNS avant de l’ordinateur virtuel est supprimé de Azure. Un des avantages de cette mise à l’échelle est que vous pouvez conserver votre DNS en local et conservez évolution vers Azure nécessite à la demande.

## <a name="bkmk_azureconfigure"></a>Comment configurer une stratégie DNS pour les réponses DNS intelligentes basées sur l’heure du jour avec le serveur d’applications Azure
Pour configurer une stratégie DNS pour les réponses heure du jour application l’équilibrage de charge en fonction de requêtes, vous devez effectuer les étapes suivantes.


- [Créer les étendues de Zone](#bkmk_zscopes)
- [Ajouter des enregistrements pour les étendues de Zone](#bkmk_records)
- [Créer les stratégies DNS](#bkmk_policies)


>[!NOTE]
>Vous devez effectuer ces étapes sur le serveur DNS faisant autorité pour la zone que vous souhaitez configurer. L’appartenance au groupe Administrateurs, ou équivalent, est requis pour effectuer les procédures suivantes. 

Les sections suivantes fournissent des instructions de configuration détaillées.

>[!IMPORTANT]
>Les sections suivantes incluent des exemples de commandes Windows PowerShell qui contiennent des exemples de valeurs pour un grand nombre de paramètres. Assurez-vous que vous remplacez les exemples de valeurs de ces commandes par les valeurs appropriées pour votre déploiement avant d’exécuter ces commandes. 


### <a name="bkmk_zscopes"></a>Créer les étendues de Zone
Une étendue de la zone est une instance unique de la zone. Une zone DNS peut avoir plusieurs étendues de zone, avec chaque étendue de la zone qui contient son propre ensemble d’enregistrements DNS. L’enregistrement même peut être présent dans plusieurs étendues, avec des adresses IP différentes ou les mêmes adresses IP. 

>[!NOTE]
>Par défaut, une étendue de la zone existe sur les zones DNS. Cette étendue de la zone a le même nom que la zone, et les opérations de DNS héritées fonctionnent sur cette étendue. 

Vous pouvez utiliser la commande suivante pour créer une étendue de la zone pour héberger les enregistrements Azure.

```
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AzureZoneScope"
```

Pour plus d’informations, voir [Add-DnsServerZoneScope](https://technet.microsoft.com/library/mt126267.aspx)

### <a name="bkmk_records"></a>Ajouter des enregistrements pour les étendues de Zone
L’étape suivante consiste à ajouter les enregistrements représentant l’hôte du serveur Web dans les étendues de zone. 

Dans AzureZoneScope, l’enregistrement www.contosogiftservices.com est ajouté avec l’adresse IP 192.68.31.44, qui se trouve dans le cloud public Azure. 

De même, dans la valeur par défaut zone étendue \(contosogiftservices.com\), un \(www.contosogiftservices.com\) enregistrement est ajouté avec adresse IP 192.68.30.2 du serveur Web en cours d’exécution dans le centre de données local Seattle.

Dans la deuxième applet de commande ci-dessous, le paramètre – ZonePortée n’est pas inclus. Pour cette raison, les enregistrements sont ajoutés dans la valeur par défaut ZonePortée. 

En outre, la durée de vie de l’enregistrement pour les machines virtuelles Azure est conservée sur 600 s (10minutes) afin que les LDNS ne pas dans le cache pendant une période plus longue, ce qui risque d’interférer avec l’équilibrage de charge. En outre, les machines virtuelles Azure sont disponibles pour 1heure supplémentaire comme une éventualité pour vous assurer que les clients même avec les enregistrements mis en cache sont en mesure de résoudre.

```
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.31.44" -ZoneScope "AzureZoneScope" –TimeToLive 600

Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.30.2"
```

Pour plus d’informations, voir [Add-DnsServerResourceRecord](https://technet.microsoft.com/library/jj649925.aspx).  

### <a name="bkmk_policies"></a>Créer les stratégies DNS 
Après avoir créé les étendues de zone, vous pouvez créer des stratégies DNS qui distribuer les requêtes entrantes sur ces étendues afin que les éléments suivants se produisent.

1. À partir de 6 h à 21 h tous les jours, 30% des clients reçoivent l’adresse IP du serveur Web du centre de données Azure dans la réponse DNS, tandis que 70% des clients reçoivent l’adresse IP du serveur Web sur site Seattle.
2. À tout autre moment, tous les clients reçoivent l’adresse IP du serveur Web sur site Seattle.

L’heure du jour doit être exprimée en heure locale du serveur DNS.

Vous pouvez utiliser la commande suivante pour créer la stratégie DNS.

```
Add-DnsServerQueryResolutionPolicy -Name "Contoso6To9Policy" -Action ALLOW –-ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” -ZoneName "contosogiftservices.com" –ProcessingOrder 1
```

Pour plus d’informations, voir [Add-DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx).  
  
Désormais, le serveur DNS est configuré avec les stratégies DNS requis pour rediriger le trafic vers le serveur Web Azure basé sur l’heure de la journée. 

Notez l’expression:

`
 -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” 
`

Cette expression configure le serveur DNS avec une combinaison de ZonePortée et la pondération indique au serveur DNS pour envoyer l’adresse IP du serveur Web Seattle 70% du temps, lors de l’envoi de l’adresse IP du serveur Web Azure trente pour cent du temps.

Vous pouvez créer des milliers de stratégies DNS en fonction de votre trafic en matière de gestion, et toutes les nouvelles stratégies sont appliquées dynamiquement, sans redémarrer le serveur DNS - dans les requêtes entrantes.
