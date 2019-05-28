---
title: Déployer des ordinateurs principaux pour la Redirection de dossiers et profils utilisateur itinérants
description: Comment activer la prise en charge de l’ordinateur principal et désigner des ordinateurs principaux pour les utilisateurs disposant de la Redirection de dossiers et profils utilisateur itinérants.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 39b790f39a2bf9c6334eb2176aa2e5f2e0196c0c
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65475965"
---
# <a name="deploy-primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>Déployer des ordinateurs principaux pour la Redirection de dossiers et profils utilisateur itinérants

>S’applique à : Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2

Cette rubrique décrit comment activer la prise en charge de l’ordinateur principal et de désigner les ordinateurs principaux pour les utilisateurs. Cela vous permet de contrôler quels ordinateurs utilisent la Redirection de dossiers et profils utilisateur itinérants.

>[!IMPORTANT]
>Lorsque vous activez la prise en charge de l’ordinateur principal pour les profils utilisateur itinérants, toujours activer des ordinateurs principaux également en charge la Redirection de dossiers. Cela permet de conserver des documents et autres fichiers d’utilisateur hors les profils utilisateur, qui vous aide à profils restent petits et connectent fois rester rapide.

## <a name="prerequisites"></a>Prérequis

## <a name="software-requirements"></a>Configuration logicielle requise

Prise en charge de l’ordinateur principal nécessite la configuration suivante :

- Le schéma des Services de domaine Active Directory (AD DS) doit être mis à jour pour inclure les ajouts de schéma Windows Server 2012 (installation d’un contrôleur de domaine de Windows Server 2012 automatiquement des mises à jour le schéma). Pour plus d’informations sur la mise à jour le schéma AD DS, consultez [intégration d’Adprep.exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh472161(v=ws.11)#adprepexe-integration>) et [exécution d’Adprep.exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10)>).
- Ordinateurs clients doivent exécuter Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.

>[!TIP]
>Bien que la prise en charge de l’ordinateur principal nécessite la Redirection de dossiers et/ou les profils utilisateur itinérants, si vous déployez ces technologies pour la première fois, il est préférable de configurer la prise en charge de l’ordinateur principal avant d’activer la stratégie de groupe qui configurent la Redirection de dossiers et Les profils utilisateur itinérants. Cela évite que des données utilisateur soient copiées sur des ordinateurs non principaux avant que la prise en charge des ordinateurs principaux soit activée. Pour plus d’informations de configuration, consultez [déployer la Redirection de dossiers](deploy-folder-redirection.md) et [déployer les profils utilisateur itinérants](deploy-roaming-user-profiles.md).

## <a name="step-1-designate-primary-computers-for-users"></a>Étape 1 : Désigner les ordinateurs principaux pour les utilisateurs

La première étape de déploiement prise en charge des ordinateurs principaux est désignation des ordinateurs principaux pour chaque utilisateur. Pour ce faire, utilisez le centre d’Administration Active Directory pour obtenir le nom unique des ordinateurs concernés, puis définissez le **msDs-PrimaryComputer** attribut.

>[!TIP]
>Pour utiliser Windows PowerShell pour travailler avec des ordinateurs principaux, consultez le blog [un peu plus loin à Explorer Windows 8 principal ordinateur](<https://blogs.technet.microsoft.com/askds/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer/>).

Voici comment spécifier les ordinateurs principaux pour les utilisateurs :

1. Ouvrez le Gestionnaire de serveur sur un ordinateur sur lequel les outils d’administration Active Directory sont installés.
2. Sur le **outils** menu, sélectionnez **centre d’Administration Active Directory**. Le Centre d’administration Active Directory s’affiche.
3. Accédez à la **ordinateurs** conteneur dans le domaine approprié.
4. Cliquez sur un ordinateur que vous souhaitez désigner comme ordinateur principal, puis sélectionnez **propriétés**.
5. Dans le volet de Navigation, sélectionnez **Extensions**.
6. Sélectionnez le **Éditeur d’attributs** onglet, faites défiler jusqu'à **distinguishedName**, sélectionnez **vue**, avec le bouton droit de la valeur répertoriée, sélectionnez **copie**, Sélectionnez **OK**, puis sélectionnez **Annuler**.
7. Accédez à la **utilisateurs** conteneur dans le domaine approprié, avec le bouton droit de l’utilisateur auquel vous souhaitez attribuer à l’ordinateur, puis sélectionnez **propriétés**.
8. Dans le volet de Navigation, sélectionnez **Extensions**.
9. Sélectionnez le **Éditeur d’attributs** onglet, sélectionnez **msDs-PrimaryComputer** , puis sélectionnez **modifier**. La boîte de dialogue Éditeur de chaînes à valeurs multiples s’affiche.
10. Cliquez sur la zone de texte, sélectionnez **coller**, sélectionnez **ajouter**, sélectionnez **OK**, puis sélectionnez **OK** à nouveau.

## <a name="step-2-optionally-enable-primary-computers-for-folder-redirection-in-group-policy"></a>Étape 2 : Si vous le souhaitez activer des ordinateurs principaux pour la Redirection de dossiers dans la stratégie de groupe

L’étape suivante consiste à configurer éventuellement des stratégie de groupe pour activer la prise en charge de l’ordinateur principal pour la Redirection de dossiers. Ainsi, les dossiers d’un utilisateur à être redirigé sur les ordinateurs désignés en tant qu’ordinateurs principal de l’utilisateur, mais pas sur tous les autres ordinateurs. Vous pouvez contrôler les ordinateurs principaux pour la Redirection de dossiers sur un ordinateur par ordinateur ou par utilisateur.

Voici comment activer des ordinateurs principaux pour la Redirection de dossiers :

1. Dans la gestion de stratégie de groupe, cliquez sur l’objet de stratégie de groupe que vous avez créé lors de la configuration initiale de la Redirection de dossiers et/ou les profils utilisateur itinérants (par exemple, **les paramètres de Redirection de dossier** ou **utilisateur itinérant Les paramètres des profils**), puis sélectionnez **modifier**.
2. Pour activer la prise en charge des ordinateurs principaux à l’aide de la stratégie de groupe basée sur l’ordinateur, accédez à **Configuration ordinateur**. Pour la stratégie de groupe basé sur utilisateur, accédez à **Configuration utilisateur**.
    - Stratégie de groupe basée sur l’ordinateur s’applique à traitement de l’ordinateur principal pour tous les ordinateurs auxquels l’objet de stratégie de groupe s’applique, ce qui affecte tous les utilisateurs des ordinateurs.
    - Utilisateur-stratégie de groupe s’applique de traitement de l’ordinateur principal pour tous les comptes d’utilisateur auquel l’objet de stratégie de groupe s’applique, qui affectent tous les ordinateurs auxquels les utilisateurs pour se connecter sur.
3. Sous **Configuration ordinateur** ou **Configuration utilisateur**, accédez à **stratégies**, puis **modèles d’administration**, puis  **Système**, puis **la Redirection de dossiers**.
4. Avec le bouton droit **redirection de dossiers sur des ordinateurs principaux uniquement**, puis sélectionnez **modifier**.
5. Sélectionnez **activé**, puis sélectionnez **OK**.

## <a name="step-3-optionally-enable-primary-computers-for-roaming-user-profiles-in-group-policy"></a>Étape 3 : Si vous le souhaitez activer des ordinateurs principaux pour les profils utilisateur itinérants dans Stratégie de groupe

L’étape suivante consiste à configurer éventuellement des stratégie de groupe pour activer la prise en charge de l’ordinateur principal pour les profils utilisateur itinérants. Cela permet à un profil utilisateur itinérant sur les ordinateurs désignés en tant qu’ordinateurs principal de l’utilisateur, mais pas sur tous les autres ordinateurs.

Voici comment activer des ordinateurs principaux pour les profils utilisateur itinérants :

1. Activer la prise en charge de l’ordinateur principal pour la Redirection de dossiers, si vous n’avez pas déjà.
    * Cela permet de conserver des documents et autres fichiers d’utilisateur hors les profils utilisateur, qui vous aide à profils restent petits et connectent fois rester rapide.
2. Dans la gestion de stratégie de groupe, cliquez sur l’objet de stratégie de groupe que vous avez créé (par exemple, **Redirection de dossiers et les paramètres du profil utilisateur itinérant**), puis sélectionnez **modifier**.
3. Accédez à **Configuration ordinateur**, puis **stratégies**, puis **modèles d’administration**, puis **système**, puis **Profils utilisateur**.
4. Avec le bouton droit **télécharger les profils itinérants sur des ordinateurs principaux uniquement,** , puis sélectionnez **modifier**.
5. Sélectionnez **activé**, puis sélectionnez **OK**.

## <a name="step-4-enable-the-gpo"></a>Étape 4 : Activer l’objet de stratégie de groupe

Une fois vous avez terminé de configurer la Redirection de dossiers et profils utilisateur itinérants, activez la stratégie de groupe si vous n’avez pas déjà. Cela permet de l’appliquer aux utilisateurs concernés et aux ordinateurs.

Voici comment activer la Redirection de dossiers et/ou les GPO de profils utilisateur itinérants :

1. Open Group Policy Management
2. Avec le bouton droit de la stratégie de groupe que vous avez créé, puis sélectionnez **lien activé**. Une case à cocher doit apparaître en regard de l’élément de menu.

## <a name="step-5-test-primary-computer-function"></a>Étape 5 : Tester la fonction de l’ordinateur principal

Pour tester la prise en charge de l’ordinateur principal, connectez-vous à un ordinateur principal, vérifiez que les dossiers et les profils sont redirigés, puis connectez-vous à un ordinateur non principal et de confirmer que les dossiers et les profils ne sont pas redirigés.

Voici comment tester les fonctionnalités de l’ordinateur principal :

1. Connectez-vous à un ordinateur désigné principal avec un compte d’utilisateur pour lequel vous avez activé la Redirection de dossiers et/ou les profils utilisateur itinérants.
2. Si le compte d’utilisateur s’est authentifié à l’ordinateur précédemment, ouvrez une session Windows PowerShell ou une fenêtre d’invite de commandes en tant qu’administrateur, tapez la commande suivante et puis déconnectez lorsque vous êtes invité à vous assurer que les derniers paramètres de stratégie de groupe sont appliquées à la ordinateur client :
    ```PowerShell
    Gpupdate /force
    ```
3. Ouvrez l’Explorateur de fichiers.
4. Cliquez sur un dossier redirigé (par exemple, le dossier Mes Documents dans la bibliothèque de Documents), puis sélectionnez **propriétés**.
5. Sélectionnez le **emplacement** onglet et confirmez que le chemin d’accès affiche le partage de fichiers que vous avez spécifié au lieu d’un chemin d’accès local. Pour vérifier que le profil utilisateur est itinérant, ouvrez **le panneau de configuration**, sélectionnez **système et sécurité**, sélectionnez **système**, sélectionnez **paramètres système avancés** , sélectionnez **paramètres** dans les profils utilisateur section, puis recherchez **itinérance** dans le **Type** colonne.
6. Connectez-vous avec le même compte d’utilisateur sur un ordinateur qui n’est pas désigné en tant qu’ordinateur principal de l’utilisateur.
7. Répétez les étapes 2 à 5, vous recherchez plutôt des chemins d’accès locales et un **Local** type de profil.

>[!NOTE]
>Si les dossiers redirigés sur un ordinateur avant l’activation de la prise en charge de l’ordinateur principal, les dossiers redirigés sauf si le paramètre suivant est configuré dans le paramètre de stratégie de redirection de dossier de chaque dossier restera : **Rediriger le dossier vers l’emplacement du profil utilisateur local lorsque la stratégie est supprimée**. De même, les profils qui étaient précédemment en itinérance sur un ordinateur particulier affichera **itinérance** dans le **Type** colonnes ; Toutefois, le **état** colonne affichera **Local**.

## <a name="more-information"></a>Informations supplémentaires

- [Déployer la Redirection de dossiers des fichiers hors connexion](deploy-folder-redirection.md)
- [Déployer des profils utilisateur itinérants](deploy-roaming-user-profiles.md)
- [Vue d’ensemble de la Redirection de dossiers, fichiers hors connexion et les profils utilisateur itinérants](folder-redirection-rup-overview.md)
- [Examiner un peu plus en détail ordinateurs principaux de Windows 8](https://blogs.technet.com/b/askds/archive/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer.aspx)