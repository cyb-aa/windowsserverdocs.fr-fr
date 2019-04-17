---
title: Autorisation d’accès
description: Cette rubrique fournit une vue d’ensemble de l’autorisation d’accès de stratégie réseau pour le serveur NPS dans Windows Server2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d6d1ca5e-bde0-4509-9e14-dc3fa9ff447e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: aeacfaeb159598d2e53bac29fb09ffc3e7739476
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="access-permission"></a>Autorisation d’accès

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

L’autorisation d’accès est configurée sur le **vue d’ensemble** onglet de chaque stratégie réseau dans serveur NPS (Network Policy). 

Ce paramètre vous permet de configurer la stratégie pour accorder ou refuser l’accès aux utilisateurs si les conditions et les contraintes de la stratégie de réseau sont conformes à la demande de connexion. 

Paramètres d’autorisation d’accès ont les conséquences suivantes:

- **Accorder l’accès**. Accès est accordé si la demande de connexion correspond aux conditions et des contraintes qui sont configurés dans la stratégie.
- **Refuser l’accès**. L’accès est refusé si la demande de connexion correspond aux conditions et des contraintes qui sont configurés dans la stratégie.

L’autorisation d’accès est également accordée ou refusée en fonction de votre configuration des propriétés d’accès à chaque compte d’utilisateur.

>[!NOTE]
>Comptes d’utilisateurs et leurs propriétés, telles que les propriétés de numérotation, sont configurées dans deux utilisateurs ActiveDirectory et les ordinateurs ou les utilisateurs locaux et groupes de MicrosoftManagement Console \(MMC\) enfichable, selon que vous disposez d’ActiveDirectory&reg; Services de domaine (ADDS) installé.

Le paramètre de compte d’utilisateur **autorisation d’accès réseau**substitutions paramètre autorisation accéder à la stratégie de réseau, qui est configuré dans les propriétés d’accès à des comptes d’utilisateur. Lorsque l’autorisation d’accès réseau sur un compte d’utilisateur est définie sur le **contrôler l’accès via la stratégie de réseau NPS** option, le paramètre d’autorisation accès réseau stratégie détermine si l’utilisateur est accordé ou refusé.

>[!NOTE]
>Dans Windows Server2016, la valeur par défaut **autorisation d’accès réseau** dans les services ADDS utilisateur à distance dans Propriétés du compte est **contrôler l’accès via la stratégie de réseau NPS**.

Lorsqu’il évalue les demandes de connexion par rapport aux stratégies réseau configurées, il effectue les actions suivantes:

- Si les conditions de la première stratégie ne correspondent pas, NPS évalue la stratégie suivante et continue ce processus qu’une correspondance est trouvée ou que toutes les stratégies ont été évaluées pour une correspondance.
- Si les conditions et les contraintes de la stratégie sont mis en correspondance, NPS accorde ou refuse l’accès, en fonction de la valeur du paramètre de la stratégie d’autorisation d’accès.
- Si les conditions d’une correspondance de stratégie, mais les contraintes dans la stratégie ne correspondent pas, NPS rejette la demande de connexion.
- Si les conditions de toutes les stratégies ne correspondent pas, NPS rejette la demande de connexion.

## <a name="ignore-user-account-dial-in-properties"></a>Ignorer les propriétés d’accès du compte d’utilisateur

Vous pouvez configurer la stratégie de réseau NPS pour ignorer les propriétés de numérotation des comptes d’utilisateur en sélectionnant ou en désactivant le **ignorer les propriétés d’accès du compte d’utilisateur** case à cocher sur le **vue d’ensemble** onglet d’une stratégie réseau. 

Normalement lorsque le serveur NPS effectue l’autorisation d’une demande de connexion, il vérifie les propriétés d’accès du compte d’utilisateur, où l’autorisation d’accès réseau valeur peut affecter si l’utilisateur est autorisé à se connecter au réseau. Lorsque vous configurez le serveur NPS pour ignorer les propriétés de numérotation des comptes d’utilisateur lors de l’autorisation, les paramètres de stratégie réseau déterminent si l’utilisateur a accès au réseau.

Les propriétés de numérotation des comptes d’utilisateur contient les éléments suivants:

- Autorisation d’accès réseau
- ID d’appelant
- Options de rappel
- Adresse IP statique
- Itinéraires statiques

Pour prendre en charge plusieurs types de connexions pour lequel NPS fournit une authentification et autorisation, il peut être nécessaire de désactiver le traitement des propriétés d’accès du compte d’utilisateur. Cela est possible pour prendre en charge les scénarios dans lesquels les propriétés de numérotation spécifiques ne sont pas requises.

Par exemple, l’ID d’appelant, rappel, adresse IP statique et les itinéraires statiques propriétés sont conçues pour les clients qui se connectent à un serveur d’accès réseau \(NAS\), pas pour les clients qui sont connectent à l’accès sans fil points. Un point d’accès sans fil qui reçoit ces paramètres dans un message RADIUS dans NPS ne peut pas être en mesure de les traiter, qui peut entraîner la déconnexion du client sans fil.

Lorsque le serveur NPS fournit une authentification et autorisation pour les utilisateurs se connectent à distance et l’accès à votre réseau d’entreprise par le biais de points d’accès sans fil, vous devez configurer les propriétés d’accès à distance pour prendre en charge des connexions à distance dans \ (par le paramètre-in properties\) ou les connexions sans fil \ (en ne définissant ne pas accès à distance properties\).

Vous pouvez utiliser le serveur NPS pour activer les propriétés d’accès à distance pour le compte d’utilisateur dans certains scénarios de traitement \ (par exemple, ou à distance) et désactiver les propriétés de numérotation dans d’autres scénarios de traitement \ (par exemple, 802. 1 X sans fil et d’authentification switch\).

Vous pouvez également utiliser **ignorer les propriétés d’accès du compte d’utilisateur** pour gérer le contrôle d’accès réseau par le biais des groupes et le paramètre d’autorisation d’accès sur la stratégie de réseau. Lorsque vous sélectionnez le **ignorer les propriétés d’accès du compte d’utilisateur** case à cocher, autorisation d’accès réseau sur le compte d’utilisateur est ignorée.

Le seul inconvénient de cette configuration est que vous ne pouvez pas utiliser les utilisateurs supplémentaires à distance dans Propriétés du compte de l’ID d’appelant, rappel, adresse IP statique et itinéraires statiques.

Pour plus d’informations sur le serveur NPS, voir [serveur NPS (Network Policy)](nps-top.md).
