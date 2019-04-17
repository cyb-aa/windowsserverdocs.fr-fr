---
title: Utiliser une stratégie DNS pour l’application de filtres sur les requêtes DNS
description: Cette rubrique fait partie de la DNS stratégie scénario Guide pour Windows Server2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: b86beeac-b0bb-4373-b462-ad6fa6cbedfa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ff10af5f88a03f806a5e5b5fa698c7c3637816bd
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-applying-filters-on-dns-queries"></a>Utiliser une stratégie DNS pour l’application de filtres sur les requêtes DNS

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour savoir comment configurer une stratégie DNS dans Windows Server&reg; 2016 pour créer des filtres de requête qui sont basées sur des critères que vous fournissez. 

Filtres de requête dans une stratégie DNS vous autorise à configurer le serveur DNS pour répondre de manière personnalisée en fonction de la requête DNS et client DNS qui envoie la requête DNS.

Par exemple, vous pouvez configurer une stratégie DNS avec un filtre de requête liste rouge qui bloque les requêtes DNS provenant des domaines malveillants connus, ce qui empêche la réponse aux requêtes à partir de ces domaines DNS. Dans la mesure où aucune réponse est envoyée à partir du serveur DNS, du membre domaine malveillant DNS expiration du délai.

Un autre exemple consiste à créer un liste d’autorisation qui permet uniquement d’un ensemble spécifique de clients à résoudre certains noms de filtre de requête.

## <a name="bkmk_criteria"></a>Critères de filtre de requête
Vous pouvez créer des filtres de requête avec n’importe quelle combinaison logique (ET/OU/non) des critères suivants.

|Nom|Description|
|-----------------|---------------------|
|Sous-réseau client|Nom d’un sous-réseau client prédéfini. Permet de vérifier le sous-réseau à partir de laquelle la requête a été envoyée.|
|Protocole de transport|Protocole utilisé dans la requête de transport. Les valeurs possibles sont UDP / TCP.|
|Protocole Internet|Protocole réseau utilisé dans la requête. Les valeurs possibles sont IPv4 et IPv6.|
|Adresse IP de l’Interface du serveur|Adresse IP de l’interface réseau du serveur DNS qui a reçu la requête DNS|
|NOM DE DOMAINE COMPLET|Nom de domaine complet de l’enregistrement de la requête, avec la possibilité d’utiliser un caractère générique.|
|Type de requête|Type d’enregistrement interrogé \ (A, SRV, TXT, etc.. \)|
|Heure du jour|Heure de que réception de la requête.|

Les exemples suivants montrent comment créer des filtres pour une stratégie DNS qui soit bloquer ou autoriser les requêtes de résolution de noms DNS.

>[!NOTE]
>Les commandes de l’exemple dans cette rubrique utilisent la commande Windows PowerShell **Add-DnsServerQueryResolutionPolicy**. Pour plus d’informations, voir [Add-DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx). 

##<a name="bkmk_block1"></a>Requêtes de bloc à partir d’un domaine

Dans certains cas, vous souhaiterez bloquer la résolution de noms DNS pour les domaines que vous avez identifiée comme malveillant, ou pour des domaines qui ne sont pas conformes avec les indications d’utilisation de votre organisation. Vous pouvez effectuer des requêtes de blocages pour les domaines en utilisant une stratégie DNS.

La stratégie que vous configurez dans cet exemple n’est pas créée sur une zone donnée, au lieu de cela, vous créez une stratégie au niveau de serveur qui est appliqué à toutes les zones configurés sur le serveur DNS. Les stratégies au niveau serveur sont les premiers à être évaluée et donc tout d’abord pour être mis en correspondance lorsqu’une requête est reçue par le serveur DNS.

La commande suivante configure une stratégie au niveau de serveur pour bloquer toutes les requêtes avec le domaine **suffixe contosomalicious.com**.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicy" -Action IGNORE -FQDN "EQ,*.contosomalicious.com" -PassThru 
`

>[!NOTE]
>Lorsque vous configurez le **Action** paramètre avec la valeur **ignorer**, le serveur DNS est configuré pour supprimer des requêtes avec aucune réponse du tout. Cela entraîne le client DNS dans le domaine malveillant expiration du délai.

##<a name="bkmk_block2"></a>Requêtes de bloc à partir d’un sous-réseau
Dans cet exemple, vous pouvez bloquer des requêtes à partir d’un sous-réseau s’il est trouvé d’être infecté par des logiciels malveillants et tente de contacter les sites malveillants à l’aide de votre serveur DNS. 

«Add-DnsServerClientSubnet-nom de «MaliciousSubnet06»-IPv4Subnet 172.0.33.0/24 - PassThru

Add-DnsServerQueryResolutionPolicy-nom de «BlockListPolicyMalicious06»-Action ignorer - ClientSubnet «EQ, MaliciousSubnet06» - PassThru»

L’exemple suivant montre comment vous pouvez utiliser les critères de sous-réseau en combinaison avec les critères de nom de domaine complet pour bloquer les requêtes pour certains domaines malveillants à partir de sous-réseaux infectés.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06" –FQDN “EQ,*.contosomalicious.com” -PassThru
`

##<a name="bkmk_block3"></a>Bloquer un type de requête
Vous devrez peut-être bloquer la résolution de noms pour certains types de requêtes sur vos serveurs. Par exemple, vous pouvez bloquer 'ANY' requête, qui peut être utilisée à des fins malveillantes pour créer des attaques amplification.

`
Add-DnsServerQueryResolutionPolicy -Name "BlockListPolicyQType" -Action IGNORE -QType "EQ,ANY" -PassThru
`

##<a name="bkmk_allow1"></a>Autoriser les requêtes uniquement à partir d’un domaine
Vous ne pouvez pas utiliser uniquement une stratégie DNS pour les requêtes de bloc, vous pouvez les utiliser pour approuver automatiquement les requêtes à partir des sous-réseaux ou des domaines spécifiques. Lorsque vous configurez répertorie les autoriser, le serveur DNS traite uniquement des requêtes à partir de domaines autorisés, tout en bloquant toutes les autres requêtes provenant d’autres domaines.

La commande suivante permet uniquement les ordinateurs et périphériques de l’enfant et contoso.com domaines interroger le serveur DNS.

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicyDomain" -Action IGNORE -FQDN "NE,*.contoso.com" -PassThru 
`

##<a name="bkmk_allow2"></a>Autoriser les requêtes uniquement à partir d’un sous-réseau
Vous pouvez également créer autoriser répertorie des sous-réseaux IP, afin que toutes les requêtes ne pas provenant de ces sous-réseaux sont ignorés.

`
Add-DnsServerClientSubnet -Name "AllowedSubnet06" -IPv4Subnet 172.0.33.0/24 -PassThru
`
`
Add-DnsServerQueryResolutionPolicy -Name "AllowListPolicySubnet” -Action IGNORE -ClientSubnet  "NE, AllowedSubnet06" -PassThru
`

##<a name="bkmk_allow3"></a>Autoriser uniquement certaines QTypes
Vous pouvez appliquer répertorie les autoriser à QTYPEs. 

Par exemple, si vous avez des clients externes interface du serveur DNS 164.8.1.1 d’interrogation, uniquement certaines QTYPEs sont autorisés à être interrogé, bien qu’il existe des autres QTYPEs comme les enregistrements SRV ou TXT qui sont utilisées par les serveurs internes pour la résolution de noms ou à des fins d’analyse.

`
Add-DnsServerQueryResolutionPolicy -Name "AllowListQType" -Action IGNORE -QType "NE,A,AAAA,MX,NS,SOA" –ServerInterface “EQ,164.8.1.1” -PassThru
`

Vous pouvez créer des milliers de stratégies DNS en fonction de votre trafic en matière de gestion, et toutes les nouvelles stratégies sont appliquées dynamiquement, sans redémarrer le serveur DNS - dans les requêtes entrantes. 
