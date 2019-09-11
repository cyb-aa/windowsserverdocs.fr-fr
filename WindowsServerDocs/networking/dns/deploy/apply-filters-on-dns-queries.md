---
title: Utiliser une stratégie DNS pour l’application de filtres sur les requêtes DNS
description: Cette rubrique fait partie du Guide de scénario de stratégie DNS pour Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: b86beeac-b0bb-4373-b462-ad6fa6cbedfa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 71e6bb5bf5fd439682277a9a8304aa785eba658d
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868911"
---
# <a name="use-dns-policy-for-applying-filters-on-dns-queries"></a>Utiliser une stratégie DNS pour l’application de filtres sur les requêtes DNS

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour apprendre à configurer une stratégie DNS dans Windows Server&reg; 2016 afin de créer des filtres de requête basés sur des critères que vous fournissez. 

Les filtres de requête dans la stratégie DNS vous permettent de configurer le serveur DNS pour qu’il réponde de manière personnalisée en fonction de la requête DNS et du client DNS qui envoie la requête DNS.

Par exemple, vous pouvez configurer une stratégie DNS avec une liste de blocage de filtre de requête qui bloque les requêtes DNS à partir de domaines malveillants connus, ce qui empêche DNS de répondre aux requêtes de ces domaines. Étant donné qu’aucune réponse n’est envoyée à partir du serveur DNS, la requête DNS du membre du domaine malveillant expire.

Un autre exemple consiste à créer une liste d’autorisation de filtre de requête qui autorise uniquement un ensemble spécifique de clients à résoudre certains noms.

## <a name="bkmk_criteria"></a>Critères de filtre de requête
Vous pouvez créer des filtres de requête avec n’importe quelle combinaison logique (et/ou/ou non) des critères suivants.

|Nom|Description|
|-----------------|---------------------|
|Sous-réseau client|Nom d’un sous-réseau client prédéfini. Utilisé pour vérifier le sous-réseau à partir duquel la requête a été envoyée.|
|Protocole de transport|Protocole de transport utilisé dans la requête. Les valeurs possibles sont UDP et TCP.|
|Protocole Internet|Protocole réseau utilisé dans la requête. Les valeurs possibles sont IPv4 et IPv6.|
|Adresse IP de l’interface serveur|Adresse IP de l’interface réseau du serveur DNS qui a reçu la demande DNS.|
|Nom de domaine complet (FQDN)|Nom de domaine complet de l’enregistrement dans la requête, avec la possibilité d’utiliser un caractère générique.|
|Type de requête|Type d’enregistrement interrogé \(, SRV, txt, etc.\)|
|Heure de la journée|Heure à laquelle la requête est reçue.|

Les exemples suivants montrent comment créer des filtres pour une stratégie DNS qui bloquent ou autorisent les requêtes de résolution de noms DNS.

>[!NOTE]
>Les exemples de commandes de cette rubrique utilisent la commande Windows PowerShell **Add-DnsServerQueryResolutionPolicy**. Pour plus d’informations, consultez [Add-DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps). 

## <a name="bkmk_block1"></a>Bloquer les requêtes d’un domaine

Dans certains cas, vous souhaiterez peut-être bloquer la résolution de noms DNS pour les domaines que vous avez identifiés comme étant malveillants, ou pour les domaines qui ne sont pas conformes aux instructions d’utilisation de votre organisation. Vous pouvez effectuer des requêtes de blocage pour les domaines à l’aide d’une stratégie DNS.

La stratégie que vous configurez dans cet exemple n’est pas créée sur une zone particulière. au lieu de cela, vous créez une stratégie au niveau du serveur qui est appliquée à toutes les zones configurées sur le serveur DNS. Les stratégies au niveau du serveur sont le premier à être évaluées et, par conséquent, sont d’abord mises en correspondance lorsqu’une requête est reçue par le serveur DNS.

L’exemple de commande suivant configure une stratégie au niveau du serveur pour bloquer toutes les requêtes avec le **suffixe**de domaine contosomalicious.com.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicy" -Action IGNORE -FQDN "EQ,*.contosomalicious.com" -PassThru 
`

>[!NOTE]
>Quand vous configurez le paramètre d' **action** avec la valeur **ignore**, le serveur DNS est configuré pour supprimer les requêtes sans aucune réponse du tout. Le client DNS dans le domaine malveillant expire alors.

## <a name="bkmk_block2"></a>Bloquer les requêtes à partir d’un sous-réseau
Dans cet exemple, vous pouvez bloquer des requêtes à partir d’un sous-réseau s’il est détecté par un programme malveillant et tente de contacter des sites malveillants à l’aide de votre serveur DNS. 

'Add-DnsServerClientSubnet-Name "MaliciousSubnet06"-IPv4Subnet 172.0.33.0/24-PassThru

Add-DnsServerQueryResolutionPolicy-Name « BlockListPolicyMalicious06 »-action IGNORe-ClientSubnet « EQ, MaliciousSubnet06 «-PassThru »

L’exemple suivant montre comment vous pouvez utiliser les critères de sous-réseau en association avec les critères de nom de domaine complet pour bloquer des requêtes pour certains domaines malveillants à partir de sous-réseaux infectés.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06" –FQDN “EQ,*.contosomalicious.com” -PassThru
`

## <a name="bkmk_block3"></a>Bloquer un type de requête
Vous devrez peut-être bloquer la résolution de noms pour certains types de requêtes sur vos serveurs. Par exemple, vous pouvez bloquer la requête « ANY », qui peut être utilisée à des fins malveillantes pour créer des attaques par amplification.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyQType" -Action IGNORE -QType "EQ,ANY" -PassThru
`

## <a name="bkmk_allow1"></a>Autoriser les requêtes uniquement à partir d’un domaine
Vous pouvez non seulement utiliser une stratégie DNS pour bloquer des requêtes, mais vous pouvez les utiliser pour approuver automatiquement des requêtes à partir de domaines ou sous-réseaux spécifiques. Quand vous configurez les listes d’autorisation, le serveur DNS traite uniquement les requêtes des domaines autorisés, tout en bloquant toutes les autres requêtes d’autres domaines.

L’exemple de commande suivant autorise uniquement les ordinateurs et les appareils dans les domaines contoso.com et enfant à interroger le serveur DNS.

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicyDomain" -Action IGNORE -FQDN "NE,*.contoso.com" -PassThru 
`

## <a name="bkmk_allow2"></a>Autoriser les requêtes uniquement à partir d’un sous-réseau
Vous pouvez également créer des listes d’autorisation pour les sous-réseaux IP, afin que toutes les requêtes qui ne proviennent pas de ces sous-réseaux soient ignorées.

`
Add-DnsServerClientSubnet -Name "AllowedSubnet06" -IPv4Subnet 172.0.33.0/24 -PassThru
`
`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicySubnet” -Action IGNORE -ClientSubnet  "NE, AllowedSubnet06" -PassThru
`

## <a name="bkmk_allow3"></a>Autoriser uniquement certains QTypes
Vous pouvez appliquer des listes d’autorisation à QTYPEs. 

Par exemple, si vous avez des clients externes interrogeant l’interface de serveur DNS 164.8.1.1, seuls certains QTYPEs sont autorisés à être interrogés, alors qu’il existe d’autres QTYPEs tels que des enregistrements SRV ou TXT utilisés par des serveurs internes pour la résolution de noms ou à des fins de surveillance.

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListQType" -Action IGNORE -QType "NE,A,AAAA,MX,NS,SOA" –ServerInterface “EQ,164.8.1.1” -PassThru
`

Vous pouvez créer des milliers de stratégies DNS en fonction de vos besoins en matière de gestion du trafic, et toutes les nouvelles stratégies sont appliquées de manière dynamique, sans redémarrer le serveur DNS-sur les requêtes entrantes. 
