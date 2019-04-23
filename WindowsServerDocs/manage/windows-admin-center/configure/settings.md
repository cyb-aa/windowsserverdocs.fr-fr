---
title: Paramètres
description: En savoir plus sur les paramètres dans Windows Admin Center (projet Honolulu). Paramètres utilisateur permettent aux utilisateurs de modifier leur langue/région et autres préférences. Paramètres de la passerelle permettent aux administrateurs de configurer la passerelle.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/19/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 1e1231500733f70ddfcbd4f8a847047b73f24a00
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882380"
---
# <a name="settings"></a>Paramètres

> S'applique à : Windows Admin Center

Les paramètres Windows Admin Center se composent des paramètres au niveau de l’utilisateur et au niveau de la passerelle. Une modification apportée à un paramètre au niveau de l’utilisateur affecte uniquement le profil de l’utilisateur actuel, tandis que la modification d’un paramètre au niveau de la passerelle affecte tous les utilisateurs sur la passerelle Windows Admin Center.

## <a name="user-settings"></a>Paramètres utilisateur

Paramètres au niveau de l’utilisateur se composent des sections suivantes :

- Compte
- Langue/région
- Suggestions

Dans le **compte** onglet, les utilisateurs peuvent consulter les informations d’identification à laquelle ils ont utilisés pour l’authentification à Windows Admin Center. Si Azure AD est configuré pour être le fournisseur d’identité, l’utilisateur peut se déconnecter de leur compte Azure AD à partir de cet onglet.

Dans le **langue/région** onglet, les utilisateurs peuvent modifier les formats de langue et région affichés par Windows Admin Center.

Dans le **Suggestions** onglet, les utilisateurs peuvent activer/désactiver les suggestions sur les nouvelles fonctionnalités et les services Azure.

### <a name="dark-theme"></a>Thème foncé

> S'applique à : Windows Admin Center Preview

Dans la version préliminaire de Windows Admin Center, vous pouvez déverrouiller un autre **personnalisation** section qui contient l’option pour activer/désactiver un thème sombre de l’interface utilisateur. Pour activer la **personnalisation** section, entrez ```msft.sme.shell.personalization``` comme une clé de l’expérience.

>[!IMPORTANT]
> Thème foncé est un travail en cours, veuillez le faire pas signaler des bogues pour l’instant.

## <a name="gateway-settings"></a>Paramètres de la passerelle

Paramètres au niveau de la passerelle se composent des sections suivantes :

- Extensions
- Accès
- Azure

Seuls les administrateurs de passerelle sont en mesure de voir et modifier ces paramètres. Modifications apportées à ces paramètres modifier la configuration de la passerelle et affectent tous les utilisateurs de la passerelle Windows Admin Center.

Dans le **Extensions** onglet, les administrateurs peuvent installer, désinstaller ou mettre à jour les extensions de passerelle. [En savoir plus sur les extensions.](using-extensions.md)

Le **accès** onglet permet aux administrateurs de configurer l’accès à la passerelle Windows Admin Center, ainsi que le fournisseur d’identité utilisé pour authentifier les utilisateurs. [En savoir plus sur le contrôle d’accès à la passerelle.](user-access-control.md)

À partir de la **Azure** onglet, les administrateurs peuvent inscrire la passerelle avec Azure pour activer [fonctionnalités d’intégration Azure](azure-integration.md) dans Windows Admin Center.