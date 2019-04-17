---
title: Paramètres
description: En savoir plus sur les paramètres dans Windows Admin Center (projet Honolulu). Paramètres utilisateur permettent aux utilisateurs de modifier leur langue/région et les autres préférences. Paramètres de la passerelle permettent aux administrateurs de configurer la passerelle.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 04/12/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: d67a2c743900792353141186112cd09dbf780309
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296621"
---
# Paramètres de Windows Admin Center

> S’applique à: Windows Admin Center

Paramètres de Windows Admin Center sont constituées des paramètres de niveau utilisateur et au niveau de passerelle. Une modification à un paramètre de niveau utilisateur affecte uniquement le profil de l’utilisateur actuel, tandis que la modification d’un paramètre au niveau de la passerelle affecte tous les utilisateurs sur la passerelle Windows Admin Center.

## Paramètres utilisateur

Paramètres au niveau de l’utilisateur comprennent les sections suivantes:

- Compte
- Personnalisation
- Langue ou une région
- Suggestions
- Avancé

Dans l’onglet **compte** , les utilisateurs peuvent passer en revue les informations d’identification qu’ils ont utilisé pour s’authentifier pour Windows Admin Center. Si Azure AD est configuré pour être le fournisseur d’identité, l’utilisateur peut se connecter en dehors de leur compte Azure AD à partir de cet onglet.

Dans l’onglet **personnalisation** , les utilisateurs peuvent basculer vers un thème foncé de l’interface utilisateur.

Dans l’onglet **Langue ou une région** , les utilisateurs peuvent modifier les formats de langue et la région affichés par Windows Admin Center.

Dans l’onglet de **Suggestions** , les utilisateurs peuvent activer/désactiver les suggestions sur les services Azure et des nouvelles fonctionnalités.

L’onglet **Options avancées** offre aux développeurs de Windows Admin Center extension des fonctionnalités supplémentaires.

## Paramètres de la passerelle

Paramètres au niveau de la passerelle comprennent les sections suivantes:

- Extensions
- Access
- Azure
- Connexions partagées

Seuls les administrateurs de passerelle sont en mesure de voir et modifier ces paramètres. Modifications apportées à ces paramètres de modifier la configuration de la passerelle et affectent tous les utilisateurs de la passerelle Windows Admin Center.

Dans l’onglet **Extensions** , les administrateurs peuvent installer, désinstaller ou mettre à jour les extensions de passerelle. [En savoir plus sur les extensions.](using-extensions.md)

L’onglet **accès** permet aux administrateurs de configurer qui peut accéder à la passerelle Windows Admin Center, ainsi que le fournisseur d’identité utilisé pour authentifier les utilisateurs. [En savoir plus sur le contrôle d’accès à la passerelle.](user-access-control.md)

Dans l’onglet **Azure** , les administrateurs peuvent s’inscrire à la passerelle avec Azure pour activer les [fonctionnalités d’intégration Azure](azure-integration.md) dans Windows Admin Center.

À l’aide de l’onglet **Connexions partagé** , les administrateurs peuvent configurer une seule liste de connexions à être partagées entre tous les utilisateurs de la passerelle Windows Admin Center. [En savoir plus sur la configuration de connexions qu’une seule fois pour tous les utilisateurs d’une passerelle.](shared-connections.md)