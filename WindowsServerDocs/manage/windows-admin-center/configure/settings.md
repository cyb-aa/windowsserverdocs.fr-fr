---
title: Paramètres
description: En savoir plus sur les paramètres de Windows Admin Center (projet Honolulu). Les paramètres utilisateur permettent aux utilisateurs de modifier leurs préférences de langue/région et autres. Les paramètres de passerelle permettent aux administrateurs de configurer la passerelle.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 04/12/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: e0fd6618f275058d4e22fe9abb9e484d4752ac9a
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "71407058"
---
# <a name="windows-admin-center-settings"></a>Paramètres de Windows Admin Center

> S'applique à : Windows Admin Center

Les paramètres de Windows Admin Center se composent de paramètres de niveau utilisateur et de niveau passerelle. La modification d’un paramètre de niveau utilisateur affecte uniquement le profil de l’utilisateur actuel, tandis que la modification d’un paramètre de niveau passerelle affecte tous les utilisateurs de cette passerelle Windows Admin Center.

## <a name="user-settings"></a>Paramètres utilisateur

Les paramètres de niveau utilisateur se composent des sections suivantes :

- Compte
- Personalization
- Langue/Région
- Suggestions
- Avancé

Dans l’onglet **Compte**, les utilisateurs peuvent consulter les informations d’identification qu’ils ont employées pour s’authentifier auprès de Windows Admin Center. Si Azure AD est configuré comme fournisseur d’identité, l’utilisateur peut se déconnecter de son compte Azure AD à partir de cet onglet.

Dans l’onglet **Personnalisation**, les utilisateurs peuvent basculer vers un thème d’interface utilisateur sombre.

Dans l’onglet **Langue/région**, les utilisateurs peuvent modifier les formats de langue et de région affichés par Windows Admin Center.

Dans l’onglet **Suggestions**, les utilisateurs peuvent activer ou désactiver les suggestions relatives aux services Azure et aux nouvelles fonctionnalités.

L’onglet **Avancé** offre des fonctionnalités supplémentaires aux développeurs d’extensions Windows Admin Center.

## <a name="gateway-settings"></a>Paramètres de la passerelle

Les paramètres de niveau passerelle se composent des sections suivantes :

- Extensions
- Accès
- Azure
- Connexions partagées

Seuls les administrateurs de passerelle sont en mesure de voir et de modifier ces paramètres. Les modifications apportées à ces paramètres modifient la configuration de la passerelle et concernent tous les utilisateurs de la passerelle Windows Admin Center.

Dans l’onglet **Extensions**, les administrateurs peuvent installer, désinstaller ou mettre à jour des extensions de passerelle. [En savoir plus sur les extensions.](using-extensions.md)

L’onglet **Accès** permet aux administrateurs de définir qui peut accéder à la passerelle Windows Admin Center, et de choisir le fournisseur d’identité utilisé pour authentifier les utilisateurs. [En savoir plus sur le contrôle des accès à la passerelle.](user-access-control.md)

Dans l’onglet **Azure**, les administrateurs peuvent inscrire la passerelle auprès d’Azure pour activer les [fonctionnalités d’intégration Azure](azure-integration.md) dans Windows Admin Center.

Dans l’onglet **Connexions partagées**, les administrateurs peuvent configurer une liste unique de connexions à partager entre tous les utilisateurs de la passerelle Windows Admin Center. [En savoir plus sur la configuration des connexions une fois pour toutes pour tous les utilisateurs d’une passerelle.](shared-connections.md)