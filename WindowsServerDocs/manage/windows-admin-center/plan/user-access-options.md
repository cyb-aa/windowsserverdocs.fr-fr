---
title: Options d’accès utilisateur avec Windows Admin Center
description: Options d’accès utilisateur et les fournisseurs d’identité avec Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 9adea736d6e7ae181bdfe50289564083146f5b30
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825890"
---
# <a name="user-access-options-with-windows-admin-center"></a>Options d’accès utilisateur avec Windows Admin Center

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

Lors du déploiement sur Windows Server, Windows Admin Center fournit un point de gestion pour votre environnement de serveur centralisé. En contrôlant l’accès à Windows Admin Center, vous pouvez améliorer la sécurité de votre paysage de la gestion.

## <a name="gateway-access-roles"></a>Rôles d’accès de passerelle

Windows Admin Center définit deux rôles pour l’accès au service de passerelle : utilisateurs de la passerelle et les administrateurs de passerelle.

> [!NOTE]
> Accès à la passerelle n’implique pas l’accès aux serveurs cibles visibles par la passerelle. Pour gérer un serveur cible, un utilisateur doit se connecter avec les informations d’identification disposant des privilèges d’administrateur sur le serveur cible.

**Utilisateurs de la passerelle** peut se connecter au service de passerelle Windows Admin Center afin de gérer des serveurs via cette passerelle, mais ils ne peuvent pas modifier les autorisations d’accès ni le mécanisme d’authentification utilisé pour authentifier à la passerelle.

**Administrateurs de passerelles** configurables qui obtient l’accès, ainsi que la manière dont les utilisateurs seront authentifier à la passerelle.

>[!NOTE]
> S’il n’y a aucun groupe d’accès définies dans Windows Admin Center, les rôles reflète le compte d’accès Windows au serveur de passerelle. 

[Configurer l’accès utilisateur et administrateur de passerelle dans Windows Admin Center.](../configure/user-access-control.md)

## <a name="identity-provider-options"></a>Options de fournisseur d’identité

Administrateurs de passerelles peuvent choisir de le des manières suivantes :

 - [Groupes d’ordinateurs actives Directory/locale](../configure/user-access-control.md#active-directory-or-local-machine-groups)
 - [Azure Active Directory comme fournisseur d’identité pour Windows Admin Center](../configure/user-access-control.md#azure-active-directory)


### <a name="smartcard-authentication"></a>Authentification par carte à puce

Lorsque vous utilisez des groupes d’ordinateurs locaux ou Active Directory comme fournisseur d’identité, vous pouvez appliquer l’authentification par carte à puce en demandant aux utilisateurs qui accèdent à Windows Admin Center pour être un membre des groupes de renforcer la sécurité basée sur une carte à puce. [Configurer l’authentification par carte à puce dans Windows Admin Center.](../configure/user-access-control.md#active-directory-or-local-machine-groups)

### <a name="conditional-access-and-multi-factor-authentication"></a>Accès conditionnel et l’authentification multifacteur

En exigeant l’authentification Azure AD pour la passerelle, vous pouvez tirer parti des fonctionnalités de sécurité supplémentaires telles que l’accès conditionnel et multi-factor authentication fournis par Azure AD. [En savoir plus sur la configuration de l’accès conditionnel à Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="role-based-access-control"></a>Contrôle d'accès basé sur les rôles

Par défaut, les utilisateurs ont besoin de privilèges administratifs locaux complets sur les ordinateurs qu’ils souhaitent gérer à l’aide de Windows Admin Center.
Cela leur permet de se connecter à la machine à distance et garantit qu’ils ont des autorisations suffisantes pour afficher et modifier les paramètres système.
Toutefois, certains utilisateurs peut-être pas un accès illimité à l’ordinateur pour effectuer leur travail.
Vous pouvez utiliser **contrôle d’accès en fonction du rôle** dans Windows Admin Center pour fournir ces utilisateurs avec un accès limité à l’ordinateur au lieu de faire les administrateurs locales complet.

Le contrôle d’accès en fonction du rôle dans Windows Admin Center fonctionne en configurant chaque serveur géré avec un PowerShell [Just Enough Administration](https://aka.ms/jeadocs) point de terminaison.
Ce point de terminaison définit les rôles, y compris quels aspects du système de chaque rôle est autorisé à gérer et à quels utilisateurs sont affectés au rôle.
Lorsqu’un utilisateur se connecte au point de terminaison restreint, un compte d’administrateur local temporaire est créé pour gérer le système en leur nom.
Cela garantit que les mêmes outils qui n’ont pas leur propre modèle de délégation peuvent toujours être gérés avec Windows Admin Center.
Le compte temporaire est automatiquement supprimé lorsque l’utilisateur arrête la gestion de l’ordinateur par le biais de Windows Admin Center.

Lorsqu’un utilisateur se connecte à un ordinateur configuré avec le contrôle d’accès en fonction du rôle, Windows Admin Center vérifie d’abord si elles sont d’un administrateur local.
S’ils sont, ils recevront l’expérience Windows Admin Center complète sans aucune restriction.
Dans le cas contraire, Windows Admin Center vérifie si l’utilisateur appartient à aucun des rôles prédéfinis.
Un utilisateur est considéré comme *d’un accès limité* s’ils appartiennent à un rôle Windows Admin Center mais que vous n’êtes pas un administrateur complet.
Enfin, si l’utilisateur n’est ni un administrateur ni un membre d’un rôle, ils seront refuser l’accès pour gérer l’ordinateur.

Contrôle d’accès en fonction du rôle est disponible pour les solutions de gestionnaire de serveur et le Cluster de basculement.

### <a name="available-roles"></a>Rôles disponibles

Windows Admin Center prend en charge les rôles de l’utilisateur final suivants :

Nom de rôle | Usage prévu
----------|-------------
Administrateurs | Permet aux utilisateurs d’utiliser la plupart des fonctionnalités dans Windows Admin Center sans leur accorder l’accès au Bureau à distance ou de PowerShell. Ce rôle est intéressante pour les scénarios de « serveur de renvoi » où vous souhaitez limiter les points d’entrée de gestion sur un ordinateur.
Lecteurs | Permet aux utilisateurs d’afficher des informations et des paramètres sur le serveur, mais pas apporter de modifications.
Administrateurs Hyper-V | Permet aux utilisateurs d’apporter des modifications aux machines virtuelles Hyper-V et commutateurs, mais limite les autres fonctionnalités permettant l’accès en lecture seule.

Les extensions intégrées suivantes ont des fonctionnalités réduites quand un utilisateur se connecte avec un accès limité :

- Fichiers (sans téléchargement du fichier ou le téléchargement)
- PowerShell (non disponible)
- Bureau à distance (non disponible)
- Réplica de stockage (non disponible)

À ce stade, vous ne pouvez pas créer des rôles personnalisés pour votre organisation, mais vous pouvez choisir quels utilisateurs ont accès à chaque rôle.

### <a name="preparing-for-role-based-access-control"></a>Préparation pour le contrôle d’accès en fonction du rôle

Pour tirer parti des comptes locaux temporaires, chaque ordinateur cible doit être configuré pour prendre en charge le contrôle d’accès en fonction du rôle dans Windows Admin Center.
Le processus de configuration implique l’installation des scripts PowerShell et un point de terminaison Just Enough Administration sur l’ordinateur à l’aide de Desired State Configuration.

Si vous n’avez que quelques ordinateurs, vous pouvez facilement appliquer individuellement la configuration pour chaque ordinateur à l’aide de la page de contrôle d’accès en fonction du rôle dans Windows Admin Center.
Lorsque vous configurez le contrôle d’accès en fonction du rôle sur un ordinateur individuel, les groupes de sécurité locaux sont créés pour contrôler l’accès à chaque rôle.
Vous pouvez accorder l’accès à des utilisateurs ou d’autres groupes de sécurité en les ajoutant en tant que membres des groupes de sécurité de rôle.

Pour un déploiement à l’échelle de l’entreprise sur plusieurs ordinateurs, vous pouvez télécharger le script de configuration à partir de la passerelle et le distribuer à vos ordinateurs à l’aide d’un serveur du collecteur de Desired State Configuration, Azure Automation, ou vos outils de gestion préférés.
