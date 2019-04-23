---
title: Autorisation d’accès
description: Cette rubrique fournit une vue d’ensemble de l’autorisation d’accès de stratégie réseau pour le serveur NPS dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d6d1ca5e-bde0-4509-9e14-dc3fa9ff447e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cdec41fb7925061bb8c8402634e1d9b1625bf301
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849490"
---
# <a name="access-permission"></a>Autorisation d’accès

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Autorisation d’accès est configurée sur le **vue d’ensemble** onglet de chaque stratégie de réseau de serveur NPS (Network Policy). 

Ce paramètre vous permet de configurer la stratégie pour accorder ou refuser l’accès aux utilisateurs si les conditions et les contraintes de la stratégie de réseau sont mises en correspondance par la demande de connexion. 

Paramètres d’autorisation accès ont l’effet suivant :

- **Accorder l’accès**. L’accès est accordé si la demande de connexion correspond aux conditions et des contraintes qui sont configurés dans la stratégie.
- **Refuser l’accès**. L’accès est refusé si la demande de connexion correspond aux conditions et des contraintes qui sont configurés dans la stratégie.

Autorisation d’accès est également accordée ou refusée en fonction de votre configuration des propriétés d’accès à distance de chaque compte d’utilisateur.

>[!NOTE]
>Comptes d’utilisateur et leurs propriétés, telles que les propriétés de l’accès à distance, sont configurées dans soit Active Directory Users et les ordinateurs ou les utilisateurs locaux et groupes Microsoft Management Console \(MMC\) -composant logiciel enfichable, selon que vous avez Active Directory&reg; installé les Services de domaine (AD DS).

Le paramètre de compte utilisateur **l’autorisation d’accès réseau**, qui est configuré dans les propriétés d’accès à distance des comptes d’utilisateur, les substitutions de paramètre d’autorisation d’accéder à la stratégie de réseau. Quand l’autorisation d’accès réseau sur un compte d’utilisateur est définie sur le **contrôler l’accès via la stratégie de réseau de NPS** option, le paramètre d’autorisation accès réseau stratégie détermine si l’utilisateur est accordé ou refusé l’accès.

>[!NOTE]
>Dans Windows Server 2016, la valeur par défaut **l’autorisation d’accès réseau** dans AD DS à distance dans Propriétés du compte utilisateur est **contrôler l’accès via la stratégie de réseau de NPS**.

Lorsque NPS évalue les demandes de connexion par rapport aux stratégies de réseau configuré, il effectue les actions suivantes :

- Si les conditions de la première stratégie ne sont pas mises en correspondance, NPS évalue la stratégie suivante et continue ce processus qu’une correspondance est trouvée, ou toutes les stratégies ont été évaluées pour une correspondance.
- Si les conditions et les contraintes d’une stratégie sont mises en correspondance, NPS accorde ou refuse l’accès, selon la valeur du paramètre d’autorisation d’accès dans la stratégie.
- Si les conditions d’une correspondance de stratégie, mais les contraintes dans la stratégie ne correspondent pas, NPS rejette la demande de connexion.
- Si les conditions de toutes les stratégies ne correspondent pas, NPS rejette la demande de connexion.

## <a name="ignore-user-account-dial-in-properties"></a>Ignorer les propriétés d’accès à distance du compte d’utilisateur

Vous pouvez configurer la stratégie de réseau NPS pour ignorer les propriétés de numérotation des comptes d’utilisateur en activant ou désactivant la **ignorer les propriétés d’accès à distance du compte d’utilisateur** case à cocher sur la **vue d’ensemble** onglet d’un réseau stratégie. 

Normalement, lorsque NPS effectue l’autorisation d’une demande de connexion, il vérifie les propriétés d’accès à distance du compte d’utilisateur, dans lequel l’autorisation d’accès réseau définissant la valeur peut affecter si l’utilisateur est autorisé à se connecter au réseau. Lorsque vous configurez le serveur NPS pour ignorer les propriétés de numérotation des comptes d’utilisateur lors de l’autorisation, les paramètres de stratégie de réseau déterminent si l’utilisateur a accès au réseau.

Les propriétés de numérotation des comptes d’utilisateur contient les éléments suivants :

- Autorisation d’accès réseau
- Caller-ID
- Options de rappel
- Adresse IP statique
- Itinéraires statiques

Pour prendre en charge plusieurs types de connexions pour lesquelles NPS fournit l’authentification et l’autorisation, il peut être nécessaire de désactiver le traitement des propriétés d’accès à distance du compte d’utilisateur. Cela est possible pour prendre en charge les scénarios dans lesquels les propriétés de numérotation spécifiques ne sont pas requises.

Par exemple, l’ID de l’appelant, de rappel, adresse IP statique et les propriétés des itinéraires statiques sont conçues pour les clients qui se connectent à un serveur d’accès réseau \(NAS\), mais pas pour les clients qui sont connectent aux points d’accès sans fil. Un point d’accès sans fil qui reçoit ces paramètres dans un message RADIUS de NPS ne peut pas être en mesure de les traiter, ce qui peut entraîner la déconnexion du client sans fil.

Lorsque NPS fournit une authentification et autorisation pour les utilisateurs qui sont à la fois dans la composition et l’accès à votre réseau d’entreprise via des points d’accès sans fil, vous devez configurer les propriétés d’accès à distance pour prendre en charge des connexions d’accès à distance \(par définition des propriétés de numérotation\) ou les connexions sans fil \(en ne définissant ne pas les propriétés de numérotation\).

Vous pouvez utiliser le serveur NPS pour activer les propriétés d’accès à distance pour le compte d’utilisateur dans certains scénarios de traitement \(, tels que dans l’accès à distance\) et désactiver les propriétés d’accès à distance dans d’autres scénarios de traitement \(telles que 802. 1 X sans fil et commutateur d’authentification\).

Vous pouvez également utiliser **ignorer les propriétés d’accès à distance du compte d’utilisateur** pour gérer le contrôle d’accès réseau via les groupes et le paramètre d’autorisation accès sur la stratégie de réseau. Lorsque vous sélectionnez le **ignorer les propriétés d’accès à distance du compte d’utilisateur** case à cocher, autorisation d’accès réseau sur le compte d’utilisateur est ignorée.

Le seul inconvénient de cette configuration est que vous ne pouvez pas utiliser les propriétés d’accès à distance du compte d’utilisateur supplémentaire de l’ID d’appelant, de rappel, adresse IP statique et les itinéraires statiques.

Pour plus d’informations sur le serveur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).
