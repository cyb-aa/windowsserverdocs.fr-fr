---
title: Autorisation d’accès
description: Cette rubrique fournit une vue d’ensemble de l’autorisation d’accès à la stratégie réseau pour le serveur NPS (Network Policy Server) dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d6d1ca5e-bde0-4509-9e14-dc3fa9ff447e
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d0521b44eb2b2733ecd8a259c5f9ca25d8bfdef1
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315799"
---
# <a name="access-permission"></a>Autorisation d’accès

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

L’autorisation d’accès est configurée sous l’onglet **vue d’ensemble** de chaque stratégie réseau dans le serveur NPS (Network Policy Server). 

Ce paramètre vous permet de configurer la stratégie pour accorder ou refuser l’accès aux utilisateurs si les conditions et contraintes de la stratégie réseau sont mises en correspondance par la demande de connexion. 

Les paramètres d’autorisation d’accès ont les effets suivants :

- **Accorder l’accès**. L’accès est accordé si la demande de connexion correspond aux conditions et contraintes configurées dans la stratégie.
- **Refuser l’accès**. L’accès est refusé si la demande de connexion correspond aux conditions et contraintes configurées dans la stratégie.

L’autorisation d’accès est également accordée ou refusée en fonction de la configuration des propriétés de numérotation de chaque compte d’utilisateur.

>[!NOTE]
>Les comptes d’utilisateur et leurs propriétés, tels que les propriétés de connexion d’accès à distance, sont configurés dans le Active Directory utilisateurs et ordinateurs ou dans le composant logiciel enfichable utilisateurs et groupes locaux de Microsoft Management Console \(MMC\), selon que vous avez installé Active Directory&reg; services de domaine (AD DS).

Le paramètre compte d’utilisateur **autorisation d’accès réseau**, qui est configuré dans les propriétés de numérotation des comptes d’utilisateur, remplace le paramètre d’autorisation accès à la stratégie réseau. Lorsque l’autorisation d’accès réseau sur un compte d’utilisateur est définie sur l’option **contrôler l’accès via la stratégie de réseau NPS** , le paramètre d’autorisation accès à la stratégie réseau détermine si l’accès est accordé ou refusé à l’utilisateur.

>[!NOTE]
>Dans Windows Server 2016, la valeur par défaut **autorisation d’accès réseau** dans AD DS propriétés d’accès à distance du compte d’utilisateur est **contrôler l’accès via la stratégie de réseau NPS**.

Lorsque NPS évalue les demandes de connexion par rapport aux stratégies réseau configurées, il effectue les actions suivantes :

- Si les conditions de la première stratégie ne sont pas mises en correspondance, le serveur NPS évalue la stratégie suivante et poursuit ce processus jusqu’à ce qu’une correspondance soit trouvée ou que toutes les stratégies aient été évaluées pour une correspondance.
- Si les conditions et contraintes d’une stratégie sont mises en correspondance, le serveur NPS accorde ou refuse l’accès, selon la valeur du paramètre d’autorisation d’accès de la stratégie.
- Si les conditions d’une stratégie correspondent mais que les contraintes de la stratégie ne correspondent pas, le serveur NPS rejette la demande de connexion.
- Si les conditions de toutes les stratégies ne correspondent pas, le serveur NPS rejette la demande de connexion.

## <a name="ignore-user-account-dial-in-properties"></a>Ignorer les propriétés de numérotation du compte d’utilisateur

Vous pouvez configurer la stratégie de réseau NPS pour ignorer les propriétés de numérotation des comptes d’utilisateur en activant ou en désactivant la case à cocher **Ignorer les propriétés d’accès à distance du compte d’utilisateur** sous l’onglet vue d' **ensemble** d’une stratégie réseau. 

Normalement, lorsque NPS effectue l’autorisation d’une demande de connexion, il vérifie les propriétés de numérotation du compte d’utilisateur, où la valeur du paramètre d’autorisation d’accès réseau peut affecter si l’utilisateur est autorisé à se connecter au réseau. Quand vous configurez NPS pour qu’il ignore les propriétés de numérotation des comptes d’utilisateur lors de l’autorisation, les paramètres de stratégie réseau déterminent si l’accès au réseau est accordé à l’utilisateur.

Les propriétés de numérotation des comptes d’utilisateur contiennent les éléments suivants :

- Autorisation d’accès réseau
- ID appelant
- Options de rappel
- Adresse IP statique
- Itinéraires statiques

Pour prendre en charge plusieurs types de connexions pour lesquelles NPS fournit une authentification et une autorisation, il peut être nécessaire de désactiver le traitement des propriétés de numérotation des comptes d’utilisateur. Cela peut être effectué pour prendre en charge des scénarios dans lesquels des propriétés de numérotation spécifiques ne sont pas requises.

Par exemple, les propriétés de l’ID de l’appelant, du rappel, de l’adresse IP statique et des itinéraires statiques sont conçues pour un client qui se connecte à un serveur d’accès réseau \(\)NAS, pas pour les clients qui se connectent aux points d’accès sans fil. Un point d’accès sans fil qui reçoit ces paramètres dans un message RADIUS du serveur NPS peut ne pas être en mesure de les traiter, ce qui peut entraîner la déconnexion du client sans fil.

Lorsque NPS fournit une authentification et une autorisation pour les utilisateurs qui se connectent et accèdent au réseau de votre organisation par le biais de points d’accès sans fil, vous devez configurer les propriétés d’accès à distance pour prendre en charge les connexions d’accès à distance \(en définissant les propriétés de connexion\) ou les connexions sans fil \(en ne définissant pas les propriétés de connexion\).

Vous pouvez utiliser NPS pour activer le traitement des propriétés de l’accès à distance pour le compte d’utilisateur dans certains scénarios \(tels que les\) d’accès à distance et pour désactiver le traitement des propriétés de connexion dans d’autres scénarios \(tels que 802.1 X sans fil et\)d’authentification.

Vous pouvez également utiliser l’option **Ignorer les propriétés du compte d’utilisateur** pour gérer le contrôle d’accès réseau via les groupes et les autorisations d’accès sur la stratégie réseau. Lorsque vous activez la case à cocher **Ignorer les propriétés du compte d’utilisateur** , l’autorisation d’accès réseau sur le compte d’utilisateur est ignorée.

Le seul inconvénient de cette configuration est que vous ne pouvez pas utiliser les propriétés de numérotation du compte d’utilisateur supplémentaires de l’ID de l’appelant, du rappel, de l’adresse IP statique et des itinéraires statiques.

Pour plus d’informations sur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).
