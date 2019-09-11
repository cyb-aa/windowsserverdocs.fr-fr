---
title: Déployer des ordinateurs principaux pour la redirection de dossiers et les profils utilisateur itinérants
description: Comment activer la prise en charge d’ordinateur principal et désigner les ordinateurs principaux pour les utilisateurs ayant une redirection de dossiers et des profils utilisateur itinérants.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: fe026b97f15b4094303c8162c5363cc6205dedd1
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867277"
---
# <a name="deploy-primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>Déployer des ordinateurs principaux pour la redirection de dossiers et les profils utilisateur itinérants

>S’applique à : Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2

Cette rubrique explique comment activer la prise en charge de l’ordinateur principal et désigner les ordinateurs principaux pour les utilisateurs. Cela vous permet de contrôler quels ordinateurs utilisent la redirection de dossiers et les profils utilisateur itinérants.

> [!IMPORTANT]
> Lorsque vous activez la prise en charge des ordinateurs principaux pour les profils utilisateur itinérants, activez toujours la prise en charge des ordinateurs principaux pour la redirection de dossiers. Cela permet de conserver les documents et autres fichiers utilisateur hors des profils utilisateur, ce qui permet aux profils de rester petits et de s’authentifier rapidement.

## <a name="prerequisites"></a>Prérequis

## <a name="software-requirements"></a>Configuration logicielle requise

La prise en charge de l’ordinateur principal présente les exigences suivantes :

- Le schéma Active Directory Domain Services (AD DS) doit être mis à jour pour inclure les ajouts de schéma Windows Server 2012 (l’installation d’un contrôleur de domaine Windows Server 2012 met automatiquement à jour le schéma). Pour plus d’informations sur la mise à jour du schéma AD DS, consultez la rubrique [intégration d’Adprep. exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh472161(v=ws.11)#adprepexe-integration>) et [exécution d’Adprep. exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10)>).
- Les ordinateurs clients doivent exécuter Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.

> [!TIP]
> Bien que la prise en charge des ordinateurs principaux nécessite une redirection de dossiers et/ou des profils utilisateur itinérants, si vous déployez ces technologies pour la première fois, il est préférable de configurer la prise en charge de l’ordinateur principal avant d’activer les objets de stratégie de groupe qui configurent la redirection de dossiers et Profils utilisateur itinérants. Cela évite que des données utilisateur soient copiées sur des ordinateurs non principaux avant que la prise en charge des ordinateurs principaux soit activée. Pour plus d’informations sur la configuration, consultez [déployer la redirection de dossiers](deploy-folder-redirection.md) et [déployer des profils utilisateur itinérants](deploy-roaming-user-profiles.md).

## <a name="step-1-designate-primary-computers-for-users"></a>Étape 1 : Désigner les ordinateurs principaux pour les utilisateurs

La première étape du déploiement de la prise en charge des ordinateurs principaux est la désignation des ordinateurs principaux pour chaque utilisateur. Pour ce faire, utilisez le centre d’administration Active Directory pour obtenir le nom unique des ordinateurs concernés, puis définissez l’attribut **MSDS-PrimaryComputer** .

> [!TIP]
> Pour utiliser Windows PowerShell pour travailler avec des ordinateurs principaux, consultez le billet de blog qui [plonge un peu plus profondément sur l’ordinateur principal Windows 8](<https://blogs.technet.microsoft.com/askds/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer/>).

Voici comment spécifier les ordinateurs principaux pour les utilisateurs :

1. Ouvrez le Gestionnaire de serveur sur un ordinateur sur lequel les outils d’administration Active Directory sont installés.
2. Dans le menu **Outils** , sélectionnez **Active Directory Centre d’administration**. Le Centre d’administration Active Directory s’affiche.
3. Accédez au conteneur **ordinateurs** dans le domaine approprié.
4. Cliquez avec le bouton droit sur un ordinateur que vous souhaitez désigner comme ordinateur principal, puis sélectionnez **Propriétés**.
5. Dans le volet de navigation, sélectionnez **Extensions**.
6. Sélectionnez l' **onglet Éditeur d’attributs** , faites défiler la liste jusqu’à **distinguishedName**, sélectionnez **affichage**, cliquez avec le bouton droit sur la valeur indiquée, sélectionnez **copier**, sélectionnez **OK**, puis sélectionnez **Annuler**.
7. Accédez au conteneur **utilisateurs** dans le domaine approprié, cliquez avec le bouton droit sur l’utilisateur auquel vous souhaitez affecter l’ordinateur, puis sélectionnez **Propriétés**.
8. Dans le volet de navigation, sélectionnez **Extensions**.
9. Sélectionnez l’onglet **éditeur d’attributs** , sélectionnez **MSDS-PrimaryComputer** , puis sélectionnez **modifier**. La boîte de dialogue Éditeur de chaînes à valeurs multiples s’affiche.
10. Cliquez avec le bouton droit sur la zone de texte, sélectionnez **coller**, sélectionnez **Ajouter**, sélectionnez **OK**, puis cliquez à nouveau sur **OK** .

## <a name="step-2-optionally-enable-primary-computers-for-folder-redirection-in-group-policy"></a>Étape 2 : Activez éventuellement les ordinateurs principaux pour la redirection de dossiers dans stratégie de groupe

L’étape suivante consiste à configurer éventuellement stratégie de groupe pour activer la prise en charge des ordinateurs principaux pour la redirection de dossiers. Cela permet de rediriger les dossiers d’un utilisateur sur les ordinateurs désignés comme ordinateurs principaux de l’utilisateur, mais pas sur les autres ordinateurs. Vous pouvez contrôler les ordinateurs principaux pour la redirection de dossiers sur une base par ordinateur ou par utilisateur.

Voici comment activer les ordinateurs principaux pour la redirection de dossiers :

1. Dans stratégie de groupe gestion, cliquez avec le bouton droit sur l’objet de stratégie de groupe que vous avez créé lors de la configuration initiale de la redirection de dossiers et/ou des profils utilisateur itinérants (par exemple, **paramètres de redirection de dossiers** ou **paramètres des profils utilisateur itinérants**), puis Sélectionnez **modifier**.
2. Pour activer la prise en charge des ordinateurs principaux à l’aide de stratégie de groupe basés sur l’ordinateur, accédez à **Configuration ordinateur**. Pour stratégie de groupe utilisateur, accédez à **Configuration utilisateur**.
    - Le stratégie de groupe basé sur l’ordinateur applique le traitement de l’ordinateur principal à tous les ordinateurs auxquels s’applique l’objet de stratégie de groupe, affectant tous les utilisateurs des ordinateurs.
    - Stratégie de groupe basée sur l’utilisateur pour appliquer le traitement de l’ordinateur principal à tous les comptes d’utilisateurs auxquels s’applique l’objet de stratégie de groupe, affectant tous les ordinateurs auxquels les utilisateurs se connectent.
3. Sous configuration **ordinateur** ou **Configuration utilisateur**, accédez à **stratégies**, **modèles d’administration**, puis **système**, puis **redirection de dossiers**.
4. Cliquez avec le bouton droit sur **Rediriger les dossiers sur les ordinateurs principaux uniquement**, puis sélectionnez **modifier**.
5. Sélectionnez **activé**, puis cliquez sur **OK**.

## <a name="step-3-optionally-enable-primary-computers-for-roaming-user-profiles-in-group-policy"></a>Étape 3 : Activez éventuellement les ordinateurs principaux pour les profils utilisateur itinérants dans stratégie de groupe

L’étape suivante consiste à configurer éventuellement stratégie de groupe pour activer la prise en charge des ordinateurs principaux pour les profils utilisateur itinérants. Cela permet au profil d’un utilisateur d’être itinérant sur des ordinateurs désignés comme ordinateurs principaux de l’utilisateur, mais pas sur d’autres ordinateurs.

Voici comment activer les ordinateurs principaux pour les profils utilisateur itinérants :

1. Activez la prise en charge des ordinateurs principaux pour la redirection de dossiers, si vous ne l’avez pas déjà fait.<br>Cela permet de conserver les documents et autres fichiers utilisateur hors des profils utilisateur, ce qui permet aux profils de rester petits et de s’authentifier rapidement.
2. Dans stratégie de groupe gestion, cliquez avec le bouton droit sur l’objet de stratégie de groupe que vous avez créé (par exemple, **paramètres de redirection de dossiers et de profils utilisateur itinérants**), puis sélectionnez **modifier**.
3. Accédez à **Configuration ordinateur**, puis **stratégies**, **modèles d’administration**, puis **système**et **profils utilisateur**.
4. Cliquez avec le bouton droit sur **Télécharger les profils itinérants sur les ordinateurs principaux uniquement,** puis sélectionnez **modifier**.
5. Sélectionnez **activé**, puis cliquez sur **OK**.

## <a name="step-4-enable-the-gpo"></a>Étape 4 : Activer l’objet de stratégie de groupe

Une fois que vous avez terminé la configuration de la redirection de dossiers et des profils utilisateur itinérants, activez l’objet de stratégie de groupe, si ce n’est déjà fait. Cela leur permet d’être appliqués aux utilisateurs et ordinateurs affectés.

Voici comment activer les objets de stratégie de groupe redirection de dossiers et/ou profils utilisateur itinérants :

1. Ouvrir la gestion des stratégie de groupe
2. Cliquez avec le bouton droit sur les objets de stratégie de groupe que vous avez créés, puis sélectionnez **lien activé**. Une case à cocher doit s’afficher en regard de l’élément de menu.

## <a name="step-5-test-primary-computer-function"></a>Étape 5 : Fonction de test de l’ordinateur principal

Pour tester la prise en charge des ordinateurs principaux, connectez-vous à un ordinateur principal, confirmez que les dossiers et les profils sont redirigés, puis connectez-vous à un ordinateur non principal et confirmez que les dossiers et les profils ne sont pas redirigés.

Voici comment tester les fonctionnalités des ordinateurs principaux :

1. Connectez-vous à un ordinateur principal désigné à l’aide d’un compte d’utilisateur pour lequel vous avez activé la redirection de dossiers et/ou les profils utilisateur itinérants.
2. Si le compte d’utilisateur s’est connecté à l’ordinateur précédemment, ouvrez une session Windows PowerShell ou une fenêtre d’invite de commandes en tant qu’administrateur, tapez la commande suivante, puis déconnectez-vous lorsque vous êtes invité à vérifier que les derniers paramètres de stratégie de groupe sont appliqués au ordinateur client :

    ```PowerShell
    Gpupdate /force
    ```

3. Ouvrez l’Explorateur de fichiers.
1. Cliquez avec le bouton droit sur un dossier redirigé (par exemple, le dossier Mes documents dans la bibliothèque de documents), puis sélectionnez **Propriétés**.
1. Sélectionnez l’onglet **emplacement** , puis vérifiez que le chemin d’accès affiche le partage de fichiers que vous avez spécifié au lieu d’un chemin d’accès local. Pour confirmer que le profil utilisateur est itinérant, ouvrez le **panneau**de configuration, sélectionnez **système et sécurité**, sélectionnez **système**, sélectionnez **paramètres système avancés**, sélectionnez **paramètres** dans la section profils utilisateur, puis recherchez  **Itinérance** dans la colonne **type** .
1. Connectez-vous avec le même compte d’utilisateur sur un ordinateur qui n’est pas désigné comme ordinateur principal de l’utilisateur.
1. Répétez les étapes 2 à 5, à la place pour rechercher des chemins d’accès locaux et un type de profil **local** .

> [!NOTE]
> Si les dossiers ont été redirigés sur un ordinateur avant que vous n’ayez activé la prise en charge de l’ordinateur principal, les dossiers restent redirigés à moins que le paramètre suivant soit configuré dans le paramètre de stratégie de redirection de dossiers de chaque dossier : **Rediriger le dossier vers l’emplacement du profil local local lorsque la stratégie est supprimée**. De même, les profils qui étaient précédemment itinérants sur un ordinateur particulier affichent l' **itinérance** dans les colonnes de **type** . Toutefois, la colonne **État** affiche **local**.

## <a name="more-information"></a>Plus d’informations

- [Déployer la redirection de dossiers avec Fichiers hors connexion](deploy-folder-redirection.md)
- [Déployer des profils utilisateur itinérants](deploy-roaming-user-profiles.md)
- [Vue d’ensemble de la redirection de dossiers, des Fichiers hors connexion et des profils utilisateur itinérants](folder-redirection-rup-overview.md)
- [Examen d’un peu plus profondément sur l’ordinateur principal Windows 8](https://blogs.technet.com/b/askds/archive/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer.aspx)