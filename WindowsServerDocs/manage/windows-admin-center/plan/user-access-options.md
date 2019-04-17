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
ms.sourcegitcommit: 4961576f2891600ef9a760ca7df650d14332e057
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2019
ms.locfileid: "9151994"
---
# Options d’accès utilisateur avec Windows Admin Center

>S’applique à: Windows Admin Center, Windows Admin Center Preview

Lorsqu’elles sont déployées sur Windows Server, Windows Admin Center fournit un point de gestion pour votre environnement de serveur centralisé. En contrôlant l’accès au centre d’administration de Windows, vous pouvez améliorer la sécurité de votre paysage de gestion.

## Rôles d’accès de passerelle

Windows Admin Center définit deux rôles pour l’accès au service de passerelle: les utilisateurs de passerelle et les administrateurs de passerelle.

> [!NOTE]
> Accéder à la passerelle n’implique pas l’accès aux serveurs cible visibles par la passerelle. Pour gérer un serveur cible, un utilisateur doit se connecter avec les informations d’identification qui ont des privilèges d’administration sur le serveur cible.

**Les utilisateurs de passerelle** peut se connecter au service de passerelle Windows Admin Center pour pouvoir gérer les serveurs par le biais de cette passerelle, mais ils ne peuvent pas modifier les autorisations d’accès, ni le mécanisme d’authentification utilisée pour s’authentifier auprès de la passerelle.

**Les administrateurs de passerelle** peuvent configurer qui obtient l’accès, ainsi que la manière dont les utilisateurs seront authentifient auprès de la passerelle.

>[!NOTE]
> S’il n’existe aucun groupe d’accès définies dans Windows Admin Center, les rôles correspondent au compte d’accès Windows au serveur de passerelle. 

[Configurer l’accès utilisateur et l’administrateur de passerelle dans Windows Admin Center.](../configure/user-access-control.md)

## Options de fournisseur d’identité

Les administrateurs de passerelle peuvent choisir une des opérations suivantes:

 - [Groupes d’ordinateurs actives Directory/local](../configure/user-access-control.md#active-directory-or-local-machine-groups)
 - [Azure Active Directory en tant que le fournisseur d’identité pour Windows Admin Center](../configure/user-access-control.md#azure-active-directory)


### Authentification par carte à puce

Lorsque vous utilisez Active Directory ou des groupes de l’ordinateur local en tant que le fournisseur d’identité, vous pouvez appliquer l’authentification par carte à puce à demander aux utilisateurs qui accèdent à Windows Admin Center pour être membre de groupes de sécurité basée sur une carte à puce supplémentaires. [Configurer l’authentification de carte à puce dans Windows Admin Center.](../configure/user-access-control.md#active-directory-or-local-machine-groups)

### Accès conditionnel et l’authentification multifacteur

Si vous exigez qu’authentification Azure AD pour la passerelle, vous pouvez exploiter les fonctionnalités de sécurité supplémentaires telles que l’accès conditionnel et une authentification multifacteur fournie par Azure AD. [En savoir plus sur la configuration de l’accès conditionnel à Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## Contrôle d'accès basé sur les rôles

Par défaut, les utilisateurs nécessitent des privilèges d’administrateur local complet sur les ordinateurs qu’ils souhaitent gérer à l’aide de Windows Admin Center.
Cela leur permet de se connecter à distance à l’ordinateur et permet de s’assurer qu’ils ont des autorisations suffisantes pour afficher et modifier les paramètres système.
Toutefois, certains utilisateurs peut-être pas accès à l’ordinateur pour effectuer leurs tâches.
Vous pouvez utiliser **le contrôle d’accès en fonction du rôle** de Windows Admin Center pour fournir ces utilisateurs avec un accès limité à l’ordinateur au lieu de rendre les administrateurs locales complet.

Contrôle d’accès en fonction du rôle dans Windows Admin Center fonctionne en configurant chaque serveur géré avec un point de terminaison PowerShell [Just Enough Administration](https://aka.ms/jeadocs) .
Ce point de terminaison définit les rôles, y compris les aspects du système de chaque rôle est autorisé à gérer et les utilisateurs affectés au rôle.
Lorsqu’un utilisateur se connecte au point de terminaison restreinte, un compte d’administrateur local temporaire est créé pour gérer le système en son nom.
Cela garantit que même les outils qui n’ont pas leur propre modèle de délégation peuvent toujours être gérés avec Windows Admin Center.
Le compte temporaire est supprimé automatiquement lorsque l’utilisateur cesse de la gestion de l’ordinateur par le biais de Windows Admin Center.

Lorsqu’un utilisateur se connecte à un ordinateur configuré avec le contrôle d’accès en fonction du rôle, Windows Admin Center vérifiera en premier lieu si elles sont un administrateur local.
S’ils sont, ils recevront une expérience complète Windows Admin Center sans aucune restriction.
Dans le cas contraire, Windows Admin Center vérifie si l’utilisateur appartient à un des rôles prédéfinies.
Un utilisateur est considéré comme *un accès limité* s’ils appartiennent à un rôle de Windows Admin Center mais que vous n’êtes pas administrateur complet.
Enfin, si l’utilisateur est un membre d’un rôle ni un administrateur, ils seront refusées accès à gérer la machine.

Contrôle d’accès en fonction du rôle est disponible pour les solutions de gestionnaire de serveur et de Cluster de basculement.

### Rôles disponibles

Windows Admin Center prend en charge les rôles de l’utilisateur final suivants:

Nom de rôle | Usage prévu
----------|-------------
Administrateurs | Permet aux utilisateurs d’utiliser la plupart des fonctionnalités dans Windows Admin Center sans lui accorder l’accès aux services Bureau à distance ou PowerShell. Ce rôle est adapté aux scénarios «sauter server» dans lequel vous souhaitez limiter les points d’entrée de gestion sur un ordinateur.
Lecteurs | Permet aux utilisateurs d’afficher des informations et des paramètres sur le serveur, mais pas apporter de modifications.
Administrateurs Hyper-V | Permet aux utilisateurs d’apporter des modifications aux ordinateurs virtuels Hyper-V et des commutateurs, mais limite les autres fonctionnalités d’accès en lecture seule.

Les extensions intégrées suivantes possèdent une fonctionnalité réduite lorsqu’un utilisateur se connecte avec un accès limité:

- Fichiers (aucun fichier téléchargement ou transfert)
- PowerShell (non disponible)
- Services Bureau à distance (non disponible)
- Réplica de stockage (non disponible)

À ce stade, vous ne pouvez pas créer des rôles personnalisés pour votre organisation, mais vous pouvez choisir les utilisateurs autorisés à accéder à chaque rôle.

### Préparation pour le contrôle d’accès en fonction du rôle

Pour tirer les comptes locaux temporaires, chaque ordinateur cible doit être configuré pour prendre en charge le contrôle d’accès en fonction du rôle de Windows Admin Center.
Le processus de configuration consiste à installer les scripts PowerShell et un point de terminaison Just Enough Administration sur l’ordinateur à l’aide de la Configuration d’état souhaité.

Si vous n’avez que quelques ordinateurs, vous pouvez facilement appliquer individuellement la configuration sur chaque ordinateur à l’aide de la page de contrôle d’accès basé sur le rôle de Windows Admin Center.
Lorsque vous configurez le contrôle d’accès en fonction du rôle sur un ordinateur individuel, les groupes de sécurité locaux sont créés pour contrôler l’accès à chaque rôle.
Vous pouvez accorder l’accès à des utilisateurs ou autres groupes de sécurité en les ajoutant en tant que membres des groupes de sécurité rôle.

Pour un déploiement à l’échelle de l’entreprise sur plusieurs ordinateurs, vous pouvez télécharger le script de configuration à partir de la passerelle et distribuez-le à vos ordinateurs à l’aide d’un serveur d’extraction de Configuration d’état souhaité, Automation Azure ou vos outils de gestion par défaut.
