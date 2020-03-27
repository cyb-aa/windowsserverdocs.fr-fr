---
title: Stratégies réseau
description: Cette rubrique fournit une vue d’ensemble des stratégies réseau pour le serveur NPS (Network Policy Server) dans Windows Server 2016 et inclut des liens vers des conseils supplémentaires sur NPS.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: e4a9b134-6d1d-40d7-a49c-5f46d5fdb419
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 4ee256184cd551c5f2c2fcdb8544e4d061ea2bf3
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315766"
---
# <a name="network-policies"></a>Stratégies réseau

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour obtenir une vue d’ensemble des stratégies réseau dans NPS.

>[!NOTE]
>En plus de cette rubrique, la documentation de la stratégie réseau suivante est disponible.
> - [Autorisation d’accès](nps-np-access.md)
> - [Configurer des stratégies réseau](nps-np-configure.md)

Les stratégies réseau sont des ensembles de conditions, de contraintes et de paramètres qui vous permettent de désigner les personnes autorisées à se connecter au réseau et les circonstances dans lesquelles elles peuvent ou ne peuvent pas se connecter.

Lors du traitement des demandes de connexion en tant que serveur protocole RADIUS (Remote Authentication Dial-In User Service) (RADIUS), NPS effectue l’authentification et l’autorisation pour la demande de connexion. Pendant le processus d’authentification, NPS vérifie l’identité de l’utilisateur ou de l’ordinateur qui se connecte au réseau. Au cours du processus d’autorisation, le serveur NPS détermine si l’utilisateur ou l’ordinateur est autorisé à accéder au réseau.

Pour effectuer ces déterminations, le serveur NPS utilise des stratégies réseau qui sont configurées dans la console NPS. NPS examine également les propriétés de numérotation du compte d’utilisateur dans Active Directory&reg; services de domaine \(AD DS\) pour effectuer l’autorisation.

## <a name="network-policies---an-ordered-set-of-rules"></a>Stratégies réseau-un ensemble ordonné de règles

Les stratégies réseau peuvent être affichées en tant que règles. Chaque règle a un ensemble de conditions et de paramètres. NPS compare les conditions de la règle aux propriétés des demandes de connexion. Si une correspondance est établie entre la règle et la demande de connexion, les paramètres définis dans la règle sont appliqués à la connexion.

Lorsque plusieurs stratégies réseau sont configurées dans NPS, il s’agit d’un ensemble ordonné de règles. NPS vérifie chaque demande de connexion par rapport à la première règle de la liste, puis la deuxième, et ainsi de suite, jusqu’à ce qu’une correspondance soit trouvée.

Chaque stratégie réseau a un paramètre d’état de la **stratégie** qui vous permet d’activer ou de désactiver la stratégie. Lorsque vous désactivez une stratégie réseau, NPS n’évalue pas la stratégie lors de l’autorisation des demandes de connexion.

>[!NOTE]
>Si vous souhaitez que NPS évalue une stratégie réseau lorsque vous effectuez une autorisation pour les demandes de connexion, vous devez configurer le paramètre d’état de la **stratégie** en activant la case à cocher stratégie activée.

## <a name="network-policy-properties"></a>Propriétés de la stratégie réseau

Il existe quatre catégories de propriétés pour chaque stratégie réseau :

### <a name="overview"></a>Overview

 Ces propriétés vous permettent de spécifier si la stratégie est activée, si la stratégie accorde ou refuse l’accès, et si une méthode de connexion réseau spécifique, ou un type de serveur d’accès réseau (NAS), est requis pour les demandes de connexion. Les propriétés de vue d’ensemble vous permettent également de spécifier si les propriétés de numérotation des comptes d’utilisateur dans AD DS sont ignorées. Si vous sélectionnez cette option, seuls les paramètres de la stratégie réseau sont utilisés par NPS pour déterminer si la connexion est autorisée.


### <a name="conditions"></a>Conditions

 Ces propriétés vous permettent de spécifier les conditions que doit avoir la demande de connexion afin de correspondre à la stratégie réseau. Si les conditions configurées dans la stratégie correspondent à la demande de connexion, le serveur NPS applique les paramètres définis dans la stratégie réseau à la connexion. Par exemple, si vous spécifiez l’adresse IPv4 NAS comme condition de la stratégie réseau et que NPS reçoit une demande de connexion d’un NAS qui a l’adresse IP spécifiée, la condition dans la stratégie correspond à la demande de connexion. 


### <a name="constraints"></a>Contraintes

 Les contraintes sont des paramètres supplémentaires de la stratégie réseau qui sont requis pour correspondre à la demande de connexion. Si une contrainte n’est pas mise en correspondance par la demande de connexion, le serveur NPS rejette automatiquement la demande. Contrairement à la réponse NPS aux conditions sans correspondance dans la stratégie réseau, si une contrainte n’est pas mise en correspondance, le serveur NPS refuse la demande de connexion sans évaluer des stratégies réseau supplémentaires.

### <a name="settings"></a>Paramètres

 Ces propriétés vous permettent de spécifier les paramètres que le serveur NPS applique à la demande de connexion si toutes les conditions de la stratégie réseau pour la stratégie sont mises en correspondance.

Lorsque vous ajoutez une nouvelle stratégie réseau à l’aide de la console NPS, vous devez utiliser l’Assistant Nouvelle stratégie réseau. Une fois que vous avez créé une stratégie réseau à l’aide de l’Assistant, vous pouvez personnaliser la stratégie en double-cliquant sur la stratégie dans la console NPS pour obtenir les propriétés de la stratégie.

Pour obtenir des exemples de syntaxe de mise en correspondance de modèle pour spécifier des attributs de stratégie réseau, consultez [utiliser des expressions régulières dans NPS](nps-crp-reg-expressions.md).

Pour plus d’informations sur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).
