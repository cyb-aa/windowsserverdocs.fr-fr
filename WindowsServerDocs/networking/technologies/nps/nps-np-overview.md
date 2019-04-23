---
title: Stratégies réseau
description: Cette rubrique fournit une vue d’ensemble de stratégies de réseau pour le serveur NPS dans Windows Server 2016 et inclut des liens vers des conseils supplémentaires sur le serveur NPS.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: e4a9b134-6d1d-40d7-a49c-5f46d5fdb419
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 60ab80bb6cf26578430b76806405a65a0f596ef2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848950"
---
# <a name="network-policies"></a>Stratégies réseau

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour une vue d’ensemble des stratégies réseau dans NPS.

>[!NOTE]
>Outre cette rubrique, la documentation de stratégie réseau suivante est disponible.
> - [Autorisation d’accès](nps-np-access.md)
> - [Configurer des stratégies de réseau](nps-np-configure.md)

Stratégies de réseau sont des ensembles de conditions, contraintes et paramètres qui vous permettent de désigner les personnes autorisées à se connecter au réseau et les circonstances dans lesquelles ils peuvent ou ne peut pas se connecter.

Lors du traitement des demandes de connexion en tant qu’un serveur d’authentification Dial-In Service RADIUS (Remote User), NPS effectue l’authentification et autorisation pour la demande de connexion. Pendant le processus d’authentification, NPS vérifie l’identité de l’utilisateur ou un ordinateur qui se connecte au réseau. Pendant le processus d’autorisation, NPS détermine si l’utilisateur ou l’ordinateur est autorisé à accéder au réseau.

Pour rendre ces déterminations, NPS utilise des stratégies de réseau qui sont configurés dans la console NPS. NPS examine également les propriétés d’accès à distance du compte d’utilisateur dans Active Directory&reg; les Services de domaine \(AD DS\) octroyer des autorisations.

## <a name="network-policies---an-ordered-set-of-rules"></a>Stratégies de réseau - un ensemble ordonné de règles

Stratégies de réseau peuvent être considérés comme règles. Chaque règle possède un ensemble de conditions et de paramètres. NPS compare les conditions de la règle pour les propriétés de demandes de connexion. Si une correspondance est trouvée entre la règle et la demande de connexion, les paramètres définis dans la règle sont appliqués à la connexion.

Lorsque plusieurs stratégies réseau sont configurées dans NPS, ils sont un ensemble ordonné de règles. NPS vérifie chaque demande de connexion par rapport à la première règle dans la liste, puis le deuxième, etc., jusqu'à ce qu’une correspondance est trouvée.

Chaque stratégie de réseau a un **état de la stratégie** paramètre qui vous permet d’activer ou désactiver la stratégie. Lorsque vous désactivez une stratégie réseau, NPS n’évalue pas la stratégie lors de l’autorisation des demandes de connexion.

>[!NOTE]
>Si vous souhaitez que le serveur NPS pour évaluer une stratégie de réseau lors de l’autorisation des demandes de connexion, vous devez configurer le **état de la stratégie** paramètre en sélectionnant la stratégie activé la case à cocher.

## <a name="network-policy-properties"></a>Propriétés de la stratégie réseau

Il existe quatre catégories de propriétés pour chaque stratégie réseau :

### <a name="overview"></a>Vue d'ensemble

 Ces propriétés permettent de spécifier si la stratégie est activée, indique si la stratégie accorde ou refuse l’accès, et si une méthode de connexion de réseau spécifique, ou type de serveur d’accès réseau (NAS), est requis pour les demandes de connexion. Propriétés de la vue d’ensemble vous permettent également de spécifier si les propriétés de numérotation des comptes d’utilisateur dans AD DS sont ignorées. Si vous sélectionnez cette option, seuls les paramètres dans la stratégie de réseau sont utilisés par le serveur NPS pour déterminer si la connexion est autorisée.


### <a name="conditions"></a>Conditions

 Ces propriétés permettent de spécifier les conditions de la demande de connexion doit avoir pour correspondre à la stratégie réseau ; Si les conditions configurées dans la stratégie correspond à la demande de connexion, NPS applique les paramètres désignés dans la stratégie de réseau pour la connexion. Par exemple, si vous spécifiez l’adresse IPv4 du NAS en tant que condition de la stratégie de réseau et NPS reçoit une demande de connexion à partir d’un NAS qui a l’adresse IP spécifiée, la condition dans la stratégie correspond à la demande de connexion. 


### <a name="constraints"></a>Contraintes

 Les contraintes sont des paramètres supplémentaires de la stratégie réseau qui sont nécessaires pour faire correspondre la demande de connexion. Si une contrainte n’est pas mis en correspondance par la demande de connexion, NPS rejette automatiquement la demande. Contrairement à la réponse de serveur NPS aux conditions sans correspondance dans la stratégie de réseau, si une contrainte n’est pas mis en correspondance, NPS refuse la demande de connexion sans l’évaluation des stratégies de réseau supplémentaire.

### <a name="settings"></a>Paramètres

 Ces propriétés permettent de spécifier les paramètres NPS s’applique à la demande de connexion si toutes les conditions de stratégie réseau pour la stratégie sont mises en correspondance.

Lorsque vous ajoutez une nouvelle stratégie de réseau à l’aide de la console NPS, vous devez utiliser l’Assistant Nouvelle stratégie de réseau. Une fois que vous avez créé une stratégie de réseau à l’aide de l’Assistant, vous pouvez personnaliser la stratégie en double-cliquant sur la stratégie dans la console NPS pour obtenir les propriétés de la stratégie.

Pour obtenir des exemples de syntaxe des critères spéciaux pour spécifier les attributs de stratégie du réseau, consultez [utiliser des Expressions régulières dans NPS](nps-crp-reg-expressions.md).

Pour plus d’informations sur le serveur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).
