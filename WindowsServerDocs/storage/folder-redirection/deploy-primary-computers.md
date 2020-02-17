---
title: Déployer des ordinateurs principaux pour la redirection de dossiers et les profils utilisateur itinérants
description: Découvrez comment activer la prise en charge des ordinateurs principaux et désigner des ordinateurs principaux pour les utilisateurs avec la redirection de dossiers et les profils utilisateur itinérants.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: be2b41cf32e2020422c32415e2d8f4273eb09859
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394439"
---
# <a name="deploy-primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>Déployer des ordinateurs principaux pour la redirection de dossiers et les profils utilisateur itinérants

>S'applique à : Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2

Cette rubrique explique comment activer la charge des ordinateurs principaux et désigner des ordinateurs principaux pour les utilisateurs. En procédant ainsi, vous pouvez contrôler quels ordinateurs utilisent la redirection de dossiers et les profils utilisateur itinérants.

> [!IMPORTANT]
> Quand vous activez la prise en charge des ordinateurs principaux pour les profils utilisateur itinérants, veillez également à toujours activer la prise en charge des ordinateurs principaux pour la redirection de dossiers. Les documents et autres fichiers utilisateur sont ainsi conservés hors des profils utilisateur, ce qui permet de réduire la taille des profils et la durée de l’authentification.

## <a name="prerequisites"></a>Prérequis

## <a name="software-requirements"></a>Configuration logicielle requise

Les exigences relatives à la prise en charge des ordinateurs principaux sont les suivantes :

- Le schéma AD DS (Active Directory Domain Services) doit être mis à jour de manière à inclure les ajouts de schéma Windows Server 2012 (l’installation d’un contrôleur de domaine Windows Server 2012 met automatiquement à jour le schéma). Pour obtenir des informations sur la mise à jour du schéma AD DS, consultez [Intégration d’Adprep.exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh472161(v=ws.11)#adprepexe-integration>) et [Exécution d’Adprep.exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10)>).
- Les ordinateurs clients doivent exécuter Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.

> [!TIP]
> Bien que la prise en charge des ordinateurs principaux nécessite la redirection de dossiers et/ou les profils utilisateur itinérants, il est préférable, si vous déployez ces technologies pour la première fois, de configurer la prise en charge des ordinateurs principaux avant d’activer les objets de stratégie de groupe qui configurent la redirection de dossiers et les profils utilisateur itinérants. Cela évite que des données utilisateur soient copiées sur des ordinateurs non principaux avant que la prise en charge des ordinateurs principaux soit activée. Pour obtenir des informations sur la configuration, consultez [Déployer la redirection de dossiers](deploy-folder-redirection.md) et [Déployer les profils utilisateur itinérants](deploy-roaming-user-profiles.md).

## <a name="step-1-designate-primary-computers-for-users"></a>Étape 1 : Désigner les ordinateurs principaux pour les utilisateurs

La première étape du déploiement de la prise en charge des ordinateurs principaux consiste à désigner les ordinateurs principaux pour chaque utilisateur. Pour cela, utilisez le Centre d’administration Active Directory pour obtenir le nom unique des ordinateurs concernés, puis définissez l’attribut **msDs-PrimaryComputer**.

> [!TIP]
> Pour utiliser Windows PowerShell avec des ordinateurs principaux, consultez le billet de blog [Digging a little deeper into Windows 8 Primary Computer](<https://blogs.technet.microsoft.com/askds/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer/>).

Voici comment spécifier les ordinateurs principaux pour les utilisateurs :

1. Ouvrez le Gestionnaire de serveur sur un ordinateur sur lequel les outils d’administration Active Directory sont installés.
2. Dans le menu **Outils**, sélectionnez **Centre d’administration Active Directory**. Le Centre d’administration Active Directory s’affiche.
3. Accédez au conteneur **Ordinateurs** dans le domaine approprié.
4. Cliquez avec le bouton droit sur un ordinateur que vous souhaitez désigner comme ordinateur principal, puis sélectionnez **Propriétés**.
5. Dans le volet de navigation, sélectionnez **Extensions**.
6. Sélectionnez l’onglet **Éditeur d’attributs**, faites défiler jusqu’à **distinguishedName**, sélectionnez **Afficher**, cliquez avec le bouton droit sur la valeur listée, puis sélectionnez **Copier**, **OK** et **Annuler**.
7. Accédez au conteneur **Utilisateurs** dans le domaine approprié, cliquez avec le bouton droit sur l’utilisateur auquel vous voulez affecter l’ordinateur, puis sélectionnez **Propriétés**.
8. Dans le volet de navigation, sélectionnez **Extensions**.
9. Sélectionnez l’onglet **Éditeur d’attributs**, **msDs-PrimaryComputer**, puis **Modifier**. La boîte de dialogue Éditeur de chaînes à valeurs multiples s’affiche.
10. Cliquez avec le bouton droit sur la zone de texte, puis sélectionnez **Coller**, **Ajouter**, **OK** et à nouveau **OK**.

## <a name="step-2-optionally-enable-primary-computers-for-folder-redirection-in-group-policy"></a>Étape 2 : Activer éventuellement les ordinateurs principaux pour la redirection de dossiers dans la stratégie de groupe

L’étape suivante est facultative et consiste à configurer la stratégie de groupe pour activer la prise en charge des ordinateurs principaux pour la redirection de dossiers. Cela permet de rediriger les dossiers d’un utilisateur sur les ordinateurs désignés comme ordinateurs principaux de l’utilisateur, mais pas sur d’autres ordinateurs. Vous pouvez contrôler les ordinateurs principaux pour la redirection de dossiers par ordinateur ou par utilisateur.

Voici comment activer les ordinateurs principaux pour la redirection de dossiers :

1. Dans Gestion des stratégies de groupe, cliquez avec le bouton droit sur l’objet de stratégie de groupe que vous avez créé lors de la configuration initiale de la redirection de dossiers et/ou des profils utilisateur itinérants (par exemple, **Paramètres de redirection de dossiers** ou **Paramètres de profils utilisateur itinérants**), puis sélectionnez **Modifier**.
2. Pour activer la prise en charge des ordinateurs principaux à l’aide d’une stratégie de groupe basée sur l’ordinateur, accédez à **Configuration ordinateur**. Pour une stratégie de groupe basée sur l’utilisateur, accédez à **Configuration utilisateur**.
    - La stratégie de groupe basée sur l’ordinateur applique le traitement des ordinateurs principaux à tous les ordinateurs auxquels l’objet de stratégie de groupe s’applique, ce qui affecte tous les utilisateurs des ordinateurs.
    - La stratégie de groupe basée sur l’utilisateur applique le traitement des ordinateurs principaux à tous les comptes d’utilisateurs auxquels l’objet de stratégie de groupe s’applique, ce qui affecte tous les ordinateurs auxquels les utilisateurs se connectent.
3. Sous **Configuration ordinateur** ou **Configuration utilisateur**, accédez à **Stratégies**, **Modèles d’administration**, **Système**, puis à **Redirection de dossiers**.
4. Cliquez avec le bouton droit sur **Rediriger les dossiers uniquement sur les ordinateurs principaux**, puis sélectionnez **Modifier**.
5. Sélectionnez **Activé**, puis **OK**.

## <a name="step-3-optionally-enable-primary-computers-for-roaming-user-profiles-in-group-policy"></a>Étape 3 : Activer éventuellement les ordinateurs principaux pour les profils utilisateur itinérants dans la stratégie de groupe

L’étape suivante est facultative et consiste à configurer la stratégie de groupe pour activer la prise en charge des ordinateurs principaux pour les profils utilisateur itinérants. Cela permet de rendre le profil d’un utilisateur itinérant sur les ordinateurs désignés comme ordinateurs principaux de l’utilisateur, mais pas sur d’autres ordinateurs.

Voici comment activer les ordinateurs principaux pour les profils utilisateur itinérants :

1. Activez la prise en charge des ordinateurs principaux pour les profils utilisateur itinérants si ce n’est pas déjà fait.<br>Les documents et autres fichiers utilisateur sont ainsi conservés hors des profils utilisateur, ce qui permet de réduire la taille des profils et la durée de l’authentification.
2. Dans Gestion des stratégies de groupe, cliquez avec le bouton droit sur l’objet de stratégie de groupe que vous avez créé (par exemple, **Paramètres de redirection de dossiers et de profils utilisateur itinérants**), puis sélectionnez **Modifier**.
3. Accédez à **Configuration ordinateur**, **Stratégies**, **Modèles d’administration**, **Système**, puis à **Profils utilisateur**.
4. Cliquez avec le bouton droit sur **Télécharger les profils itinérants sur les ordinateurs principaux uniquement**, puis sélectionnez **Modifier**.
5. Sélectionnez **Activé**, puis **OK**.

## <a name="step-4-enable-the-gpo"></a>Étape 4 : Activer l’objet de stratégie de groupe

Une fois que vous avez terminé de configurer la redirection de dossiers et les profils utilisateur itinérants, activez l’objet de stratégie de groupe si ce n’est pas déjà fait. Il peut ainsi être appliqué aux utilisateurs et ordinateurs affectés.

Voici comment activer les objets de stratégie de groupe de redirection de dossiers et/ou de profils utilisateur itinérants :

1. Ouvrez Gestion des stratégies de groupe.
2. Cliquez avec le bouton droit sur les objets de stratégie de groupe que vous avez créés, puis sélectionnez **Lien activé**. Une coche doit apparaître en regard de l’élément de menu.

## <a name="step-5-test-primary-computer-function"></a>Étape 5 : Tester le fonctionnement des ordinateurs principaux

Pour tester la prise en charge des ordinateurs principaux, connectez-vous à un ordinateur principal, confirmez que les dossiers et profils sont redirigés, puis connectez-vous à un ordinateur non principal et vérifiez que les dossiers et profils ne sont pas redirigés.

Voici comment tester le fonctionnement des ordinateurs principaux :

1. Connectez-vous à un ordinateur principal désigné à l’aide d’un compte d’utilisateur pour lequel vous avez activé la redirection de dossiers et/ou les profils utilisateur itinérants.
2. Si vous vous êtes déjà connecté à l’ordinateur avec ce compte d’utilisateur, ouvrez une session Windows PowerShell ou une fenêtre d’invite de commandes en tant qu’administrateur, tapez la commande suivante, puis déconnectez-vous quand vous êtes invité à vérifier que les derniers paramètres de stratégie de groupe sont appliqués à l’ordinateur client :

    ```PowerShell
    Gpupdate /force
    ```

3. Ouvrez l’Explorateur de fichiers.
1. Cliquez avec le bouton droit sur un dossier redirigé (par exemple, le dossier Mes documents dans la bibliothèque Documents), puis sélectionnez **Propriétés**.
1. Sélectionnez l’onglet **Emplacement**, puis vérifiez que le chemin indique le partage de fichiers que vous avez spécifié au lieu d’un chemin local. Pour vérifier que le profil utilisateur est itinérant, ouvrez le **Panneau de configuration**, sélectionnez **Système et sécurité**, **Système**, **Paramètres système avancés** et **Paramètres** dans la section Profils utilisateur, puis recherchez **Itinérant** dans la colonne **Type**.
1. Connectez-vous avec le même compte d’utilisateur sur un ordinateur qui n’est pas désigné comme ordinateur principal de l’utilisateur.
1. Répétez les étapes 2 à 5 en recherchant cette fois-ci des chemins locaux et un type de profil **Local**.

> [!NOTE]
> Si les dossiers ont été redirigés sur un ordinateur avant l’activation de la prise en charge des ordinateurs principaux, les dossiers restent redirigés à moins que le paramètre suivant ne soit configuré dans le paramètre de stratégie de redirection de dossiers de chaque dossier : **Rediriger le dossier vers l’emplacement du profil utilisateur local lorsque la stratégie sera supprimée**. De même, les profils qui étaient précédemment itinérants sur un ordinateur donné indiquent **Itinérant** dans les colonnes **Type** ; toutefois, la colonne **État** indique **Local**.

## <a name="more-information"></a>Autres informations

- [Déployer la redirection de dossiers avec Fichiers hors connexion](deploy-folder-redirection.md)
- [Déployer des profils utilisateur itinérants](deploy-roaming-user-profiles.md)
- [Vue d’ensemble de la redirection de dossiers, des fichiers hors connexion et des profils utilisateur itinérants](folder-redirection-rup-overview.md)
- [Digging a little deeper into Windows 8 Primary Computer](https://blogs.technet.com/b/askds/archive/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer.aspx)