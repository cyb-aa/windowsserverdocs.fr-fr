---
title: Paramètres
description: En savoir plus sur les paramètres dans le centre d’administration Windows (projet Honolulu). Les paramètres utilisateur permettent aux utilisateurs de modifier leur langue/région et d’autres préférences. Les paramètres de la passerelle permettent aux administrateurs de configurer la passerelle.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 04/12/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: e184064aa913bf4fb18cadd8ddbb08b5b97c59c6
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865323"
---
# <a name="windows-admin-center-settings"></a>Paramètres du centre d’administration Windows

> S'applique à : Windows Admin Center

Les paramètres du centre d’administration Windows se composent de paramètres au niveau de l’utilisateur et de la passerelle. La modification d’un paramètre de niveau utilisateur affecte uniquement le profil de l’utilisateur actuel, tandis qu’une modification d’un paramètre de niveau passerelle affecte tous les utilisateurs de cette passerelle du centre d’administration Windows.

## <a name="user-settings"></a>Paramètres utilisateur

Les paramètres au niveau de l’utilisateur se composent des sections suivantes :

- Compte
- Personnalisation
- Langue/région
- Suggestions
- Avancé

Dans l’onglet **compte** , les utilisateurs peuvent consulter les informations d’identification qu’ils ont utilisées pour s’authentifier auprès du centre d’administration Windows. Si Azure AD est configuré comme fournisseur d’identité, l’utilisateur peut se déconnecter de son compte Azure AD à partir de cet onglet.

Dans l’onglet **personnalisation** , les utilisateurs peuvent basculer vers un thème d’interface utilisateur sombre.

Dans l’onglet **langue/région** , les utilisateurs peuvent modifier les formats de langue et de région affichés par le centre d’administration Windows.

Dans l’onglet **suggestions** , les utilisateurs peuvent activer ou désactiver les suggestions relatives aux services Azure et aux nouvelles fonctionnalités.

L’onglet **avancé** donne des fonctionnalités supplémentaires aux développeurs d’extensions du centre d’administration Windows.

## <a name="gateway-settings"></a>Paramètres de la passerelle

Les paramètres au niveau de la passerelle sont constitués des sections suivantes :

- Extensions
- Access
- Azure
- Connexions partagées

Seuls les administrateurs de passerelle sont en mesure de voir et de modifier ces paramètres. Les modifications apportées à ces paramètres modifient la configuration de la passerelle et affectent tous les utilisateurs de la passerelle du centre d’administration Windows.

Sous l’onglet **Extensions** , les administrateurs peuvent installer, désinstaller ou mettre à jour les extensions de passerelle. [En savoir plus sur les extensions.](using-extensions.md)

L’onglet **accès** permet aux administrateurs de configurer les utilisateurs autorisés à accéder à la passerelle du centre d’administration Windows, ainsi que le fournisseur d’identité utilisé pour authentifier les utilisateurs. [En savoir plus sur le contrôle de l’accès à la passerelle.](user-access-control.md)

Dans l’onglet **Azure** , les administrateurs peuvent inscrire la passerelle auprès d’Azure pour activer les [fonctionnalités d’intégration Azure](azure-integration.md) dans le centre d’administration Windows.

À l’aide de l’onglet **connexions partagées** , les administrateurs peuvent configurer une liste unique de connexions à partager entre tous les utilisateurs de la passerelle du centre d’administration Windows. [En savoir plus sur la configuration des connexions une seule fois pour tous les utilisateurs d’une passerelle.](shared-connections.md)