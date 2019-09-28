---
title: Options d’accès utilisateur avec le centre d’administration Windows
description: Options d’accès utilisateur et fournisseurs d’identité avec le centre d’administration Windows (projet Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 084cdae0bf8ca0eb3aff1f4679d30978b860efef
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356919"
---
# <a name="user-access-options-with-windows-admin-center"></a>Options d’accès utilisateur avec le centre d’administration Windows

>S'applique à : Windows Admin Center, Windows Admin Center Preview

Une fois déployé sur Windows Server, le centre d’administration Windows fournit un point de gestion centralisé pour votre environnement serveur. En contrôlant l’accès au centre d’administration Windows, vous pouvez améliorer la sécurité de votre environnement de gestion.

## <a name="gateway-access-roles"></a>Rôles d’accès à la passerelle

Le centre d’administration Windows définit deux rôles pour l’accès au service de passerelle : les utilisateurs de la passerelle et les administrateurs de la passerelle.

> [!NOTE]
> L’accès à la passerelle n’implique pas l’accès aux serveurs cibles visibles par la passerelle. Pour gérer un serveur cible, un utilisateur doit se connecter avec des informations d’identification disposant de privilèges d’administrateur sur le serveur cible.

**Les utilisateurs** de la passerelle peuvent se connecter au service de passerelle du centre d’administration Windows afin de gérer les serveurs par le biais de cette passerelle, mais ils ne peuvent pas modifier les autorisations d’accès ni le mécanisme d’authentification utilisé pour s’authentifier auprès de la passerelle.

**Les administrateurs de passerelle** peuvent configurer les utilisateurs qui accèdent à la passerelle, ainsi que la façon dont ils s’authentifient.

>[!NOTE]
> Si aucun groupe d’accès n’est défini dans le centre d’administration Windows, les rôles reflètent l’accès du compte Windows au serveur de passerelle. 

[Configurez l’accès de l’utilisateur et de l’administrateur de la passerelle dans le centre d’administration Windows.](../configure/user-access-control.md)

## <a name="identity-provider-options"></a>Options du fournisseur d’identité

Les administrateurs de passerelle peuvent choisir l’un des éléments suivants :

 - [Groupes de machines Active Directory/locales](../configure/user-access-control.md#active-directory-or-local-machine-groups)
 - [Azure Active Directory en tant que fournisseur d’identité pour le centre d’administration Windows](../configure/user-access-control.md#azure-active-directory)


### <a name="smartcard-authentication"></a>Authentification par carte à puce

Lorsque vous utilisez Active Directory ou des groupes d’ordinateurs locaux comme fournisseur d’identité, vous pouvez appliquer l’authentification par carte à puce en demandant aux utilisateurs qui accèdent au centre d’administration Windows d’être membres d’autres groupes de sécurité basés sur une carte à puce. [Configurez l’authentification par carte à puce dans le centre d’administration Windows.](../configure/user-access-control.md#active-directory-or-local-machine-groups)

### <a name="conditional-access-and-multi-factor-authentication"></a>Accès conditionnel et multi-Factor Authentication

En exigeant une authentification Azure AD pour la passerelle, vous pouvez tirer parti de fonctionnalités de sécurité supplémentaires telles que l’accès conditionnel et l’authentification multifacteur fournies par Azure AD. [En savoir plus sur la configuration de l’accès conditionnel avec Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="role-based-access-control"></a>Contrôle d'accès basé sur les rôles

Par défaut, les utilisateurs ont besoin de privilèges d’administrateur local complets sur les ordinateurs qu’ils souhaitent gérer à l’aide du centre d’administration Windows.
Cela leur permet de se connecter à distance à l’ordinateur et de s’assurer qu’ils disposent d’autorisations suffisantes pour afficher et modifier les paramètres système.
Toutefois, certains utilisateurs n’ont peut-être pas besoin d’un accès illimité à l’ordinateur pour effectuer leurs tâches.
Vous pouvez utiliser le **contrôle d’accès en fonction du rôle** dans le centre d’administration Windows pour fournir à ces utilisateurs un accès limité à l’ordinateur au lieu d’en faire des administrateurs locaux complets.

Le contrôle d’accès en fonction du rôle dans le centre d’administration Windows fonctionne en configurant chaque serveur géré avec un point de terminaison d' [administration juste assez](https://aka.ms/jeadocs) PowerShell.
Ce point de terminaison définit les rôles, y compris les aspects du système que chaque rôle est autorisé à gérer et les utilisateurs qui sont affectés au rôle.
Lorsqu’un utilisateur se connecte au point de terminaison restreint, un compte d’administrateur local temporaire est créé pour gérer le système en son nom.
Cela permet de s’assurer que même les outils qui n’ont pas leur propre modèle de délégation peuvent toujours être gérés avec le centre d’administration Windows.
Le compte temporaire est automatiquement supprimé lorsque l’utilisateur cesse de gérer l’ordinateur via le centre d’administration Windows.

Lorsqu’un utilisateur se connecte à un ordinateur configuré avec le contrôle d’accès en fonction du rôle, le centre d’administration Windows vérifie d’abord s’il s’agit d’un administrateur local.
Si c’est le cas, ils recevront l’expérience complète du centre d’administration Windows sans aucune restriction.
Dans le cas contraire, le centre d’administration Windows vérifie si l’utilisateur appartient à l’un des rôles prédéfinis.
Un utilisateur est considéré comme ayant un *accès limité* s’il appartient à un rôle du centre d’administration Windows, mais qu’il ne s’agit pas d’un administrateur complet.
Enfin, si l’utilisateur n’est ni un administrateur ni un membre d’un rôle, il se voit refuser l’accès pour gérer l’ordinateur.

Le contrôle d’accès en fonction du rôle est disponible pour les solutions de cluster de basculement et de Gestionnaire de serveur.

### <a name="available-roles"></a>Rôles disponibles

Le centre d’administration Windows prend en charge les rôles d’utilisateur final suivants :

Nom de rôle | Usage prévu
----------|-------------
Administrateurs | Permet aux utilisateurs d’utiliser la plupart des fonctionnalités du centre d’administration Windows sans leur accorder l’accès à Bureau à distance ou PowerShell. Ce rôle convient aux scénarios « Jump Server » dans lesquels vous souhaitez limiter les points d’entrée de gestion sur un ordinateur.
Lecteurs | Permet aux utilisateurs d’afficher des informations et des paramètres sur le serveur, mais pas d’apporter des modifications.
Administrateurs Hyper-V | Permet aux utilisateurs d’apporter des modifications aux machines virtuelles Hyper-V et aux commutateurs, mais limite les accès en lecture seule aux autres fonctionnalités.

Les extensions intégrées suivantes ont des fonctionnalités réduites lorsqu’un utilisateur se connecte avec un accès limité :

- Fichiers (pas de chargement ou de téléchargement de fichiers)
- PowerShell (non disponible)
- Bureau à distance (non disponible)
- Réplica de stockage (non disponible)

À ce stade, vous ne pouvez pas créer de rôles personnalisés pour votre organisation, mais vous pouvez choisir les utilisateurs qui disposent d’un accès à chaque rôle.

### <a name="preparing-for-role-based-access-control"></a>Préparation du contrôle d’accès en fonction du rôle

Pour tirer parti des comptes locaux temporaires, chaque ordinateur cible doit être configuré pour prendre en charge le contrôle d’accès en fonction du rôle dans le centre d’administration Windows.
Le processus de configuration implique l’installation de scripts PowerShell et d’un point de terminaison d’administration juste suffisant sur l’ordinateur à l’aide de la configuration d’état souhaité.

Si vous ne disposez que de quelques ordinateurs, vous pouvez facilement appliquer la configuration individuellement à chaque ordinateur à l’aide de la page contrôle d’accès en fonction du rôle dans le centre d’administration Windows.
Quand vous configurez le contrôle d’accès en fonction du rôle sur un ordinateur individuel, les groupes de sécurité locaux sont créés pour contrôler l’accès à chaque rôle.
Vous pouvez accorder l’accès à des utilisateurs ou à d’autres groupes de sécurité en les ajoutant en tant que membres des groupes de sécurité de rôle.

Pour un déploiement à l’ensemble de l’entreprise sur plusieurs ordinateurs, vous pouvez télécharger le script de configuration à partir de la passerelle et le distribuer à vos ordinateurs à l’aide d’un serveur collecteur de configuration d’état souhaité, d’Azure Automation ou de vos outils de gestion préférés.
