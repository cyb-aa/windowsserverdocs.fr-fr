---
title: Options d’accès utilisateur avec Windows Admin Center
description: Options d’accès utilisateur et fournisseurs d’identité avec Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 084cdae0bf8ca0eb3aff1f4679d30978b860efef
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356919"
---
# <a name="user-access-options-with-windows-admin-center"></a>Options d’accès utilisateur avec Windows Admin Center

>S'applique à : Windows Admin Center, Windows Admin Center Preview

Une fois déployé sur Windows Server, Windows Admin Center fournit un point de gestion centralisé pour votre environnement serveur. En contrôlant l’accès à Windows Admin Center, vous pouvez améliorer la sécurité de votre environnement de gestion.

## <a name="gateway-access-roles"></a>Rôles d’accès à la passerelle

Windows Admin Center définit deux rôles pour l’accès au service de passerelle : utilisateurs de passerelle et administrateurs de passerelle.

> [!NOTE]
> L’accès à la passerelle n’implique pas l’accès aux serveurs cibles visibles par la passerelle. Pour gérer un serveur cible, un utilisateur doit se connecter avec des informations d’identification disposant de privilèges d’administrateur sur le serveur cible.

Les **utilisateurs de passerelle** peuvent se connecter au service de passerelle Windows Admin Center pour gérer les serveurs par le biais de cette passerelle, mais ils ne peuvent pas modifier les autorisations d’accès ni le mécanisme d’authentification utilisé pour l’authentification auprès de la passerelle.

Les **administrateurs de passerelle** peuvent configurer qui obtient l’accès à la passerelle, ainsi que la façon dont les utilisateurs s’authentifieront auprès d’elle.

>[!NOTE]
> Si aucun groupe d’accès n’est défini dans Windows Admin Center, les rôles reflètent l’accès du compte Windows au serveur de passerelle. 

[Configurer l’accès des utilisateurs et des administrateurs de passerelle dans Windows Admin Center.](../configure/user-access-control.md)

## <a name="identity-provider-options"></a>Options du fournisseur d’identité

Les administrateurs de passerelle peuvent choisir l’un des éléments suivants :

 - [Groupes d’ordinateurs Active Directory/locaux](../configure/user-access-control.md#active-directory-or-local-machine-groups)
 - [Azure Active Directory en tant que fournisseur d’identité pour Windows Admin Center](../configure/user-access-control.md#azure-active-directory)


### <a name="smartcard-authentication"></a>Authentification par carte à puce

Quand vous utilisez Active Directory ou des groupes d’ordinateurs locaux comme fournisseur d’identité, vous pouvez appliquer l’authentification par carte à puce en imposant aux utilisateurs qui accèdent à Windows Admin Center d’être membres d’autres groupes de sécurité basés sur une carte à puce. [Configurer l’authentification par carte à puce dans Windows Admin Center.](../configure/user-access-control.md#active-directory-or-local-machine-groups)

### <a name="conditional-access-and-multi-factor-authentication"></a>Accès conditionnel et authentification multifacteur

En exigeant l’authentification Azure AD pour la passerelle, vous pouvez tirer parti de fonctionnalités de sécurité supplémentaires telles que l’accès conditionnel et l’authentification multifacteur fournies par Azure AD. [En savoir plus sur la configuration de l’accès conditionnel avec Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="role-based-access-control"></a>Contrôle d'accès basé sur les rôles

Par défaut, les utilisateurs ont besoin de privilèges d’administrateur local complets sur les ordinateurs qu’ils souhaitent gérer à l’aide de Windows Admin Center.
Cela leur permet de se connecter à distance à l’ordinateur, et garantit qu’ils disposent d’autorisations suffisantes pour afficher et modifier les paramètres système.
Toutefois, certains utilisateurs n’auront peut-être pas besoin d’un accès illimité à l’ordinateur pour effectuer leurs tâches.
Vous pouvez utiliser le **contrôle d’accès en fonction du rôle** dans Windows Admin Center pour fournir à ces utilisateurs un accès limité à l’ordinateur au lieu d’en faire des administrateurs locaux complets.

Le contrôle d’accès en fonction du rôle dans Windows Admin Center configure chaque serveur managé avec un point de terminaison PowerShell [Just Enough Administration](https://aka.ms/jeadocs).
Ce point de terminaison définit les rôles, notamment quels aspects du système chaque rôle est autorisé à gérer et quels utilisateurs sont affectés au rôle.
Quand un utilisateur se connecte au point de terminaison restreint, un compte d’administrateur local temporaire est créé pour gérer le système en son nom.
Cela permet de s’assurer que même les outils qui n’ont pas leur propre modèle de délégation peuvent toujours être gérés avec Windows Admin Center.
Le compte temporaire est supprimé automatiquement quand l’utilisateur cesse de gérer l’ordinateur par le biais de Windows Admin Center.

Quand un utilisateur se connecte à un ordinateur configuré avec le contrôle d’accès en fonction du rôle, Windows Admin Center vérifie d’abord s’il s’agit d’un administrateur local.
Si c’est le cas, il bénéficiera de l’expérience Windows Admin Center complète sans aucune restriction.
Sinon, Windows Admin Center vérifie si l’utilisateur appartient à l’un des rôles prédéfinis.
Un utilisateur est considéré comme ayant un *accès limité* s’il appartient à un rôle Windows Admin Center, mais qu’il ne s’agit pas d’un administrateur complet.
Pour finir, si l’utilisateur n’est ni administrateur ni membre d’un rôle, il se voit refuser l’accès à la gestion de l’ordinateur.

Le contrôle d’accès en fonction du rôle est disponible pour les solutions Cluster de basculement et Gestionnaire de serveur.

### <a name="available-roles"></a>Rôles disponibles

Windows Admin Center prend en charge les rôles d’utilisateur final suivants :

Nom de rôle | Usage prévu
----------|-------------
Administrateurs | Permet aux utilisateurs d’utiliser la plupart des fonctionnalités de Windows Admin Center sans que l’accès à Bureau à distance ou à PowerShell leur soit accordé. Ce rôle convient aux scénarios « Jump Server » dans lesquels vous souhaitez limiter les points d’entrée de gestion sur un ordinateur.
Lecteurs | Permet aux utilisateurs d’afficher des informations et des paramètres sur le serveur, mais pas d’apporter des modifications.
Administrateurs Hyper-V | Permet aux utilisateurs d’apporter des modifications aux commutateurs et aux machines virtuelles Hyper-V, mais limite les autres fonctionnalités à un accès en lecture seule.

Les extensions intégrées suivantes ont des fonctionnalités réduites quand un utilisateur se connecte avec un accès limité :

- Fichiers (pas de chargement ou de téléchargement de fichiers)
- PowerShell (non disponible)
- Bureau à distance (non disponible)
- Réplica de stockage (non disponible)

À l’heure actuelle, vous ne pouvez pas créer de rôles personnalisés pour votre organisation, mais vous pouvez choisir les utilisateurs qui disposent d’un accès à chaque rôle.

### <a name="preparing-for-role-based-access-control"></a>Préparation du contrôle d’accès en fonction du rôle

Pour tirer parti des comptes locaux temporaires, chaque ordinateur cible doit être configuré pour prendre en charge le contrôle d’accès en fonction du rôle dans Windows Admin Center.
Le processus de configuration implique l’installation de scripts PowerShell et d’un point de terminaison Just Enough Administration sur l’ordinateur à l’aide de Desired State Configuration.

Si vous n’avez que quelques ordinateurs, vous pouvez facilement appliquer la configuration individuellement à chaque ordinateur par le biais de la page de contrôle d’accès en fonction du rôle dans Windows Admin Center.
Quand vous configurez le contrôle d’accès en fonction du rôle sur un ordinateur, des groupes de sécurité locaux sont créés pour contrôler l’accès à chaque rôle.
Vous pouvez accorder l’accès à des utilisateurs ou à d’autres groupes de sécurité en les ajoutant en tant que membres des groupes de sécurité de rôle.

Pour un déploiement à l’échelle de l’entreprise sur plusieurs ordinateurs, vous pouvez télécharger le script de configuration à partir de la passerelle et le distribuer à vos ordinateurs à l’aide d’un serveur Pull Desired State Configuration, d’Azure Automation ou de vos outils de gestion préférés.
