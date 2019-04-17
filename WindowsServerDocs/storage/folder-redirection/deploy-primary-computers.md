---
title: Déployer des ordinateurs principales pour la Redirection de dossiers et les profils utilisateur itinérants
description: Comment activer la prise en charge de l’ordinateur principal et désigner des ordinateurs principales pour les utilisateurs avec la Redirection de dossiers et les profils utilisateur itinérants.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 7b3c87597e07102d00fc068b7ecd5744e4ba366f
ms.sourcegitcommit: 505505b7ec99506e7ff50eddbdd6aa94a602abe6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/11/2018
ms.locfileid: "3831399"
---
# Déployer des ordinateurs principales pour la Redirection de dossiers et les profils utilisateur itinérants

>S’applique à: Windows 10, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

Cette rubrique décrit comment activer la prise en charge de l’ordinateur principal et désigner des ordinateurs principales pour les utilisateurs. Cela vous permet de contrôler les ordinateurs qui utilisent la Redirection de dossiers et les profils utilisateur itinérants.

>[!IMPORTANT]
>Lorsque vous activez la prise en charge de l’ordinateur principal pour les profils utilisateur itinérants, toujours activer l’ordinateur principal prise en charge de la Redirection de dossiers également. Cela maintient les documents et autres fichiers de l’utilisateur hors les profils utilisateur, ce qui permet de profils restent petit et vous connecter sur le temps de reste rapide.

## Conditions préalables

## Configuration logicielle requise

Prise en charge de l’ordinateur principal présente les conditions suivantes:

- Le schéma Active Directory Domain Services (AD DS) doit être mis à jour pour inclure des ajouts de schéma Windows Server 2012 (l’installation d’un contrôleur de domaine Windows Server 2012 automatiquement les mises à jour le schéma). Pour plus d’informations sur la mise à jour le schéma AD DS, voir [l’intégration Adprep.exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh472161(v=ws.11)#adprepexe-integration>) et [Adprep.exe en cours d’exécution](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10)>).
- Les ordinateurs clients doivent exécuter Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.

>[!TIP]
>Bien que la prise en charge de l’ordinateur principal nécessite la Redirection de dossiers et/ou les profils utilisateur itinérants, si vous déployez ces technologies pour la première fois, il est préférable de configurer la prise en charge de l’ordinateur principal avant d’activer les objets de stratégie de groupe qui configurent la Redirection de dossiers et Les profils utilisateur itinérants. Cela empêche les données utilisateur d’être copiés sur les ordinateurs non principal avant d’activer la prise en charge de l’ordinateur principal. Pour plus d’informations de configuration, voir [Déployer la Redirection de dossiers](deploy-folder-redirection.md) et [Déployer les profils utilisateur itinérants](deploy-roaming-user-profiles.md).

## Étape 1: Désigner des ordinateurs principales pour les utilisateurs

La première étape du déploiement de prise en charge des ordinateurs principal est désignant les ordinateurs principales pour chaque utilisateur. Pour ce faire, utilisez le centre d’Administration Active Directory pour obtenir le nom unique des ordinateurs concernés, puis définissez l’attribut **msDs-PrimaryComputer** .

>[!TIP]
>Pour utiliser Windows PowerShell pour travailler avec des ordinateurs principales, consultez le blog [en profondeur un peu en ordinateur principal de Windows 8](<https://blogs.technet.microsoft.com/askds/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer/>).

Voici comment procéder spécifier les ordinateurs principales pour les utilisateurs:

1. Ouvrez le Gestionnaire de serveur sur un ordinateur sur lequel les outils d’administration Active Directory sont installés.
2. Dans le menu **Outils** , sélectionnez le **Centre d’Administration Active Directory**. Le Centre d’administration Active Directory s’affiche.
3. Accédez au conteneur **ordinateurs** dans le domaine approprié.
4. Avec le bouton droit sur un ordinateur que vous voulez désigner comme un ordinateur principal, puis sélectionnez **Propriétés**.
5. Dans le volet de Navigation, sélectionnez **les Extensions**.
6. Sélectionnez l’onglet de **l’Éditeur d’attributs** , faites défiler jusqu'à **distinguishedName**, sélectionnez **Afficher**, avec le bouton droit de la valeur indiquée, sélectionnez la **copie**, sélectionnez **OK**, puis **Annuler**.
7. Naviguez vers le conteneur **utilisateurs** dans le domaine approprié et avec le bouton droit de l’utilisateur auquel vous souhaitez attribuer à l’ordinateur, puis sélectionnez **Propriétés**.
8. Dans le volet de Navigation, sélectionnez **les Extensions**.
9. Sélectionnez l’onglet de **l’Éditeur de l’attribut** **msDs-PrimaryComputer** et sélectionnez puis **Modifier**. La boîte de dialogue Éditeur de chaînes à valeurs multiples s’affiche.
10. Avec le bouton droit de la zone de texte, sélectionnez l’option **Coller**, sélectionnez **Ajouter**, sélectionnez **OK**et sélectionnez **OK** à nouveau.

## Étape 2: Les ordinateurs principales éventuellement activer la Redirection de dossiers dans la stratégie de groupe

L’étape suivante consiste à éventuellement configurer la stratégie de groupe pour activer la prise en charge de l’ordinateur principal de la Redirection de dossiers. Cela permet de dossiers d’un utilisateur d’être redirigé sur les ordinateurs désignés comme des ordinateurs principal de l’utilisateur, mais pas sur d’autres ordinateurs. Vous pouvez contrôler les ordinateurs principales pour la Redirection de dossiers sur un ordinateur par ordinateur ou par utilisateur.

Voici comment procéder permettre aux ordinateurs principales pour la Redirection de dossiers:

1. Dans Gestion des stratégies de groupe, l’objet GPO que vous avez créé lorsque vous effectuez la configuration initiale de la Redirection de dossiers et/ou les profils utilisateur itinérants (par exemple, **Les paramètres de Redirection de dossiers** ou **Paramètres des profils utilisateur itinérants**), avec le bouton droit, puis Sélectionnez le **Modifier**.
2. Pour activer la prise en charge des ordinateurs principal à l’aide de la stratégie de groupe basé sur un ordinateur, accédez à la **Configuration de l’ordinateur**. Stratégie de groupe basé sur l’utilisateur, accédez à **Configuration de l’utilisateur**.
    - Stratégie de groupe basé sur l’ordinateur s’applique traitement de l’ordinateur principal à tous les ordinateurs auxquels l’objet de stratégie de groupe s’applique, qui affectent tous les utilisateurs des ordinateurs.
    - Stratégie de groupe basé sur l’utilisateur s’applique de traitement de l’ordinateur principal pour tous les comptes d’utilisateur à laquelle l’objet de stratégie de groupe s’applique, qui affectent tous les ordinateurs auxquels les utilisateurs se connectent.
3. Sous **Configuration de l’ordinateur** ou **Configuration utilisateur**, accédez à **La Redirection de dossiers**, **les stratégies**, puis **Modèles d’administration**, puis **système**.
4. Avec le bouton droit de **redirection de dossiers uniquement sur les principaux ordinateurs**et sélectionnez **Modifier**.
5. Sélectionnez **activé**et sélectionnez **OK**.

## Étape 3: Éventuellement activer des ordinateurs principales pour les profils utilisateur itinérants dans la stratégie de groupe

L’étape suivante consiste à éventuellement configurer la stratégie de groupe pour activer la prise en charge de l’ordinateur principal pour les profils utilisateur itinérants. Cela permet à un profil utilisateur d’assurer l’itinérance sur les ordinateurs désignés comme des ordinateurs principal de l’utilisateur, mais pas sur d’autres ordinateurs.

Voici comment procéder permettre aux ordinateurs principales pour les profils utilisateur itinérants:

1. Activer la prise en charge de l’ordinateur principal de la Redirection de dossiers, si vous n’avez pas déjà.
    * Cela maintient les documents et autres fichiers de l’utilisateur hors les profils utilisateur, ce qui permet de profils restent petit et vous connecter sur le temps de reste rapide.
2. Dans Gestion des stratégies de groupe avec le bouton droit de la stratégie de groupe que vous avez créé (par exemple, **la Redirection de dossiers et les paramètres de profils utilisateur itinérants**) et sélectionnez **Modifier**.
3. Accédez à la **Configuration de l’ordinateur**, puis **stratégies**, puis **modèles d’administration**, puis **système**et **les profils utilisateur**.
4. Avec le bouton droit à la **télécharger sur les ordinateurs principales uniquement, les profils itinérants** , puis sélectionnez **Modifier**.
5. Sélectionnez **activé**et sélectionnez **OK**.

## Étape 4: Activer l’objet de stratégie de groupe

Une fois vous avez terminé de configurer la Redirection de dossiers et les profils utilisateur itinérants, activer l’objet de stratégie de groupe, si vous n’avez pas déjà. Cela permet à être appliqués aux ordinateurs et des utilisateurs concernés.

Voici comment faire pour activer la Redirection de dossiers et/ou le GPO de profils utilisateur itinérants:

1. Ouvrir la gestion des stratégies
2. Avec le bouton droit de la stratégie de groupe que vous avez créé et sélectionnez **Lien activé**. Une case à cocher doit apparaître en regard de l’élément de menu.

## Étape 5: Tester la fonction de l’ordinateur principal

Pour tester la prise en charge de l’ordinateur principal, se connecter à un ordinateur principal, vérifiez que les dossiers et les profils sont redirigés, puis se connecter à un ordinateur non principal et à vérifier que les dossiers et les profils ne sont pas redirigées.

Voici comment procéder pour tester la fonctionnalité de l’ordinateur principal:

1. Connectez-vous à un ordinateur désigné principal avec un compte d’utilisateur pour lequel vous avez activé la Redirection de dossiers et/ou les profils utilisateur itinérants.
2. Si le compte d’utilisateur a connecté à l’ordinateur précédemment, ouvrez une session Windows PowerShell ou une fenêtre d’invite de commandes en tant qu’administrateur, tapez la commande suivante et puis valider lorsque vous êtes invité à vous assurer que les derniers paramètres de stratégie de groupe sont appliquées à la ordinateur client:
    ```PowerShell
    Gpupdate /force
    ```
3. Ouvrez l’Explorateur de fichiers.
4. Avec le bouton droit à un dossier redirigé (par exemple, le dossier Mes Documents dans la bibliothèque Documents) et sélectionnez **Propriétés**.
5. Sélectionnez l’onglet **emplacement** et vérifiez que le chemin d’accès au partage de fichiers que vous avez spécifié au lieu d’un chemin d’accès local s’affiche. Pour vérifier que le profil utilisateur est en itinérance, ouvrez le **Panneau** **système et sécurité**, sélectionnez le **système**, sélectionnez les **Paramètres système avancés**, sélectionnez les **paramètres** dans la section de profils utilisateur et sélectionnez, puis recherchez ** Itinérance** dans la colonne **Type** .
6. Connectez-vous avec le même compte d’utilisateur à un ordinateur qui n’est pas désigné en tant qu’ordinateur principal de l’utilisateur.
7. Répétez les étapes 2 à 5, au lieu de cela vous recherchez des chemins d’accès locales et un type de profil **Local** .

>[!NOTE]
>Si les dossiers ont été redirigés sur un ordinateur avant que vous avez activé la prise en charge de l’ordinateur principal, les dossiers reste redirigés, sauf si le paramètre suivant est configuré dans le paramètre de stratégie de redirection de dossiers de chaque dossier: **rediriger le dossier vers l’ordinateur local emplacement du profil utilisateur lorsque la stratégie est supprimée**. De même, les profils qui ont été précédemment itinérance sur un ordinateur spécifique seront affichent **Roaming** dans les colonnes de **Type** ; Toutefois, la colonne **état** affiche **Local**.

## Informations supplémentaires

- [Déployer la Redirection de dossiers avec des fichiers hors connexion](deploy-folder-redirection.md)
- [Déployer les profils utilisateur itinérants](deploy-roaming-user-profiles.md)
- [Vue d’ensemble de la Redirection de dossiers, fichiers hors connexion et les profils utilisateur itinérants](folder-redirection-rup-overview.md)
- [Examiner un peu plus en ordinateur principal de Windows 8](https://blogs.technet.com/b/askds/archive/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer.aspx)