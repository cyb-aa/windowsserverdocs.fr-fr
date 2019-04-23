---
title: Utiliser une stratégie DNS pour l’application de filtres sur les requêtes DNS
description: Cette rubrique fait partie de DNS stratégie scénario Guide pour Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: b86beeac-b0bb-4373-b462-ad6fa6cbedfa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4b00c773462569f005a73f535b1a872ae7b389db
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859900"
---
# <a name="use-dns-policy-for-applying-filters-on-dns-queries"></a>Utiliser une stratégie DNS pour l’application de filtres sur les requêtes DNS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour savoir comment configurer une stratégie DNS dans Windows Server&reg; 2016 pour créer des filtres de requête qui sont basées sur des critères que vous fournissez. 

Filtres de requête dans une stratégie DNS vous autorise à configurer le serveur DNS pour répondre de manière personnalisée en fonction de la requête DNS et le client DNS qui envoie la requête DNS.

Par exemple, vous pouvez configurer une stratégie DNS avec le filtre de requête liste de blocage qui bloque les requêtes DNS provenant des domaines malveillants connus, ce qui empêche que DNS répond aux requêtes à partir de ces domaines. Car aucune réponse n’est envoyé à partir du serveur DNS, du membre domaine malveillant DNS expire la requête.

Un autre exemple consiste à créer un liste verte qui permet uniquement un ensemble spécifique de clients à résoudre certains noms de filtre de requête.

## <a name="bkmk_criteria"></a> Critères de filtre de requête
Vous pouvez créer des filtres de requête avec n’importe quelle combinaison logique (OR/NOT) des critères suivants.

|Nom|Description|
|-----------------|---------------------|
|Sous-réseau du client|Nom du sous-réseau client prédéfini. Permet de vérifier le sous-réseau à partir de laquelle la requête a été envoyée.|
|Protocole de transport|Protocole utilisé dans la requête de transport. Les valeurs possibles sont les protocoles UDP et TCP.|
|Protocole Internet|Protocole réseau utilisé dans la requête. Les valeurs possibles sont IPv4 et IPv6.|
|Adresse IP de l’Interface du serveur|Adresse IP de l’interface réseau du serveur DNS qui a reçu la requête DNS.|
|Nom de domaine complet (FQDN)|Nom de domaine complet de l’enregistrement de la requête, avec la possibilité d’utiliser un caractère générique.|
|Type de requête|Type d’enregistrement en cours d’interrogation \(A, SRV, TXT, etc.\).|
|Heure du jour|Heure de que la requête est reçue.|

Les exemples suivants montrent comment créer des filtres pour une stratégie DNS qui soit bloquer ou autoriser les requêtes de résolution de noms DNS.

>[!NOTE]
>Les commandes de l’exemple dans cette rubrique utilisent la commande Windows PowerShell **Add-DnsServerQueryResolutionPolicy**. Pour plus d’informations, consultez [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps). 

##<a name="bkmk_block1"></a>Requêtes de bloc à partir d’un domaine

Dans certains cas, vous souhaiterez bloquer la résolution de noms DNS pour les domaines que vous avez identifié comme étant malveillante ou pour les domaines qui ne sont pas conformes avec les instructions d’utilisation de votre organisation. Vous pouvez effectuer les requêtes bloquantes pour les domaines à l’aide d’une stratégie DNS.

La stratégie que vous configurez dans cet exemple n’est pas créée sur une zone donnée : au lieu de cela, vous créez une stratégie de niveau serveur qui est appliqué à toutes les zones configurés sur le serveur DNS. Stratégies de niveau serveur sont les premiers à évaluer et par conséquent, tout d’abord à mettre en correspondance lorsqu’une requête est reçue par le serveur DNS.

La commande suivante configure une stratégie de niveau serveur pour bloquer toutes les requêtes avec le domaine **suffixe contosomalicious.com**.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicy" -Action IGNORE -FQDN "EQ,*.contosomalicious.com" -PassThru 
`

>[!NOTE]
>Lorsque vous configurez le **Action** paramètre avec la valeur **ignorer**, le serveur DNS est configuré pour supprimer des requêtes avec aucune réponse du tout. Ce cas, le client DNS dans le domaine malveillant expiration du délai.

##<a name="bkmk_block2"></a>Requêtes de bloc à partir d’un sous-réseau
Avec cet exemple, vous pouvez bloquer les requêtes à partir d’un sous-réseau si elle se trouve être infectés par des logiciels malveillants et tente de contacter les sites malveillants à l’aide de votre serveur DNS. 

` Add-DnsServerClientSubnet -Name "MaliciousSubnet06" -IPv4Subnet 172.0.33.0/24 -PassThru

Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06" -PassThru `

L’exemple suivant montre comment vous pouvez utiliser les critères de sous-réseau en combinaison avec les critères de nom de domaine complet pour bloquer les requêtes pour certains domaines malveillants à partir de sous-réseaux infectés.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06" –FQDN “EQ,*.contosomalicious.com” -PassThru
`

##<a name="bkmk_block3"></a>Un type de requête de bloc
Vous devrez peut-être bloquer la résolution de noms pour certains types de requêtes sur vos serveurs. Par exemple, vous pouvez bloquer la requête 'ANY', qui peut être utilisée à des fins malveillantes pour créer des attaques d’amplification.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyQType" -Action IGNORE -QType "EQ,ANY" -PassThru
`

##<a name="bkmk_allow1"></a>Autoriser les requêtes uniquement à partir d’un domaine
Vous ne pouvez pas utiliser uniquement une stratégie DNS pour les requêtes de blocage, vous pouvez les utiliser pour approuver automatiquement les requêtes à partir des domaines spécifiques ou des sous-réseaux. Lorsque vous configurez les listes vertes, le serveur DNS traite uniquement les requêtes à partir de domaines autorisés, tout en bloquant toutes les autres requêtes à partir d’autres domaines.

La commande suivante autorise uniquement les ordinateurs et les appareils dans les domaines contoso.com et enfants pour interroger le serveur DNS.

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicyDomain" -Action IGNORE -FQDN "NE,*.contoso.com" -PassThru 
`

##<a name="bkmk_allow2"></a>Autoriser les requêtes uniquement à partir d’un sous-réseau
Vous pouvez également créer listes vertes pour des sous-réseaux IP, afin que toutes les requêtes qui ne proviennent ne pas de ces sous-réseaux sont ignorés.

`
Add-DnsServerClientSubnet -Name "AllowedSubnet06" -IPv4Subnet 172.0.33.0/24 -PassThru
`
`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicySubnet” -Action IGNORE -ClientSubnet  "NE, AllowedSubnet06" -PassThru
`

##<a name="bkmk_allow3"></a>Autoriser uniquement certaines QTypes
Vous pouvez appliquer des listes vertes à QTYPEs. 

Par exemple, si vous avez des clients externes interrogation interface du serveur DNS 164.8.1.1, uniquement certaines QTYPEs sont autorisés à être interrogées, bien qu’il existe des autres QTYPEs comme les enregistrements SRV ou TXT qui sont utilisées par les serveurs internes pour la résolution de noms ou des fonctions d’analyse.

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListQType" -Action IGNORE -QType "NE,A,AAAA,MX,NS,SOA" –ServerInterface “EQ,164.8.1.1” -PassThru
`

Vous pouvez créer des milliers de stratégies DNS, en fonction de votre trafic en matière de gestion, et toutes les nouvelles stratégies sont appliquées dynamiquement, sans avoir à redémarrer le serveur DNS : sur les requêtes entrantes. 
