---
title: Déployer la redirection de dossiers avec Fichiers hors connexion
description: Découvrez comment utiliser Windows Server pour déployer la redirection de dossiers avec Fichiers hors connexion sur des ordinateurs clients Windows.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: 6d8f6bf0df67b76028945403352bd135e6641a5a
ms.sourcegitcommit: ab3967d71dcbb962079af194875de58e7c32c4e2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/03/2020
ms.locfileid: "76967420"
---
# <a name="deploy-folder-redirection-with-offline-files"></a>Déployer la redirection de dossiers avec Fichiers hors connexion

>S'applique à : Windows 10, Windows 7, Windows 8, Windows 8.1, Windows Vista, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2, Windows Server 2008 R2, Windows Server (canal semi-annuel)

Cette rubrique décrit comment utiliser Windows Server pour déployer la redirection de dossiers avec Fichiers hors connexion sur des ordinateurs clients Windows.

Pour obtenir la liste des modifications récentes apportées à cette rubrique, consultez l’[historique des modifications](#change-history).

> [!IMPORTANT]
> En raison des modifications de sécurité apportées dans [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), nous avons mis à jour la section [Étape 3 : Créer un objet de stratégie de groupe pour la redirection de dossiers](#step-3-create-a-gpo-for-folder-redirection) de cette rubrique pour que Windows puisse appliquer correctement la stratégie de redirection de dossiers (et ne pas rétablir les dossiers redirigés sur les PC affectés).

## <a name="prerequisites"></a>Prérequis

### <a name="hardware-requirements"></a>Configuration matérielle requise

La redirection de dossiers nécessite un ordinateur x64 ou x86. Elle n’est pas prise en charge par Windows® RT.

### <a name="software-requirements"></a>Configuration logicielle requise

La redirection de dossiers nécessite la configuration logicielle suivante :

- Pour administrer la redirection de dossiers, vous devez vous connecter en tant que membre du groupe de sécurité Administrateurs de domaine, Administrateurs d’entreprise ou Propriétaires créateurs de la stratégie de groupe.
- Les ordinateurs clients doivent exécuter Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server (canal semi-annuel), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008.
- Les ordinateurs clients doivent être joints aux services de domaine Active Directory (AD DS) que vous gérez.
- Un ordinateur sur lequel la gestion des stratégies de groupe et le Centre d'administration Active Directory sont installés doit être disponible.
- Un serveur de fichiers doit être disponible pour héberger les dossiers redirigés.
    - Si le partage de fichiers utilise des espaces de noms DFS, les dossiers DFS (liens) doivent avoir une seule cible pour éviter que les utilisateurs effectuent des modifications en conflit sur différents serveurs.
    - Si le partage de fichiers utilise la réplication DFS pour répliquer le contenu avec un autre serveur, les utilisateurs doivent pouvoir accéder uniquement au serveur source pour éviter que les utilisateurs effectuent des modifications en conflit sur différents serveurs.
    - Quand vous utilisez un partage de fichiers en cluster, désactivez la disponibilité continue sur le partage de fichiers afin d’éviter les problèmes de performances liés à la redirection de dossiers et aux fichiers hors connexion. En outre, la fonctionnalité Fichiers hors connexion peut ne pas passer en mode hors connexion pendant 3 à 6 minutes après qu’un utilisateur perd l’accès à un partage de fichiers disponible en continu, ce qui peut frustrer les utilisateurs qui n’utilisent pas le mode Toujours hors connexion de la fonctionnalité Fichiers hors connexion.

> [!NOTE]
> Certaines fonctionnalités plus récentes de la redirection de dossiers imposent des exigences supplémentaires en ce qui concerne les ordinateurs clients et les schémas Active Directory. Pour plus d’informations, consultez [Déployer des ordinateurs principaux](deploy-primary-computers.md), [Désactiver la fonctionnalité Fichiers hors connexion sur les dossiers](disable-offline-files-on-folders.md), [Activer le mode Toujours hors connexion](enable-always-offline.md) et [Activer le déplacement de dossier optimisé](enable-optimized-moving.md).

## <a name="step-1-create-a-folder-redirection-security-group"></a>Étape 1 : Créer un groupe de sécurité de redirection de dossiers

Si votre environnement n’est pas déjà configuré avec la redirection de dossiers, la première étape consiste à créer un groupe de sécurité qui contient tous les utilisateurs auxquels vous voulez appliquer les paramètres de stratégie de la redirection de dossiers.

Voici comment créer un groupe de sécurité pour la redirection de dossiers :

1. Ouvrez le Gestionnaire de serveur sur un ordinateur sur lequel le Centre d’administration Active Directory est installé.
2. Dans le menu **Outils**, sélectionnez **Centre d’administration Active Directory**. Le Centre d’administration Active Directory s’affiche.
3. Cliquez avec le bouton droit sur l’unité d’organisation ou le domaine appropriés, sélectionnez **Nouveau**, puis **Groupe**.
4. Dans la fenêtre **Créer un groupe** , dans la section **Groupe** , indiquez les paramètres suivants :
    - Dans **Nom du groupe**, tapez le nom du groupe de sécurité, par exemple : **Utilisateurs de la redirection de dossiers**.
    - Dans **Étendue du groupe**, sélectionnez **Sécurité**, puis **Globale**.
5. Dans la section **Membres**, sélectionnez **Ajouter**. La boîte de dialogue Sélectionnez Utilisateurs, Contacts, Ordinateurs ou Groupes s’affiche.
6. Tapez les noms des utilisateurs ou des groupes sur lesquels vous voulez déployer la redirection de dossiers, sélectionnez **OK**, puis à nouveau **OK**.

## <a name="step-2-create-a-file-share-for-redirected-folders"></a>Étape 2 : Créer un partage de fichiers pour les dossiers redirigés

Si vous ne disposez pas déjà d’un partage de fichiers pour les dossiers redirigés, utilisez la procédure suivante pour créer un partage de fichiers sur un serveur Windows Server 2012.

> [!NOTE]
> Certaines fonctionnalités peuvent être différentes ou non disponibles si vous créez le partage de fichiers sur un serveur exécutant une autre version de Windows Server.

Voici comment créer un partage de fichiers sur Windows Server 2019, Windows Server 2016 et Windows Server 2012 :

1. Dans le volet de navigation du Gestionnaire de serveur, sélectionnez **Services de fichiers et de stockage**, puis **Partages** pour afficher la page Partages.
2. Dans la vignette **Partages**, sélectionnez **Tâches**, puis **Nouveau partage**. L'Assistant Nouveau partage s'affiche.
3. Dans la page **Sélectionner un profil**, sélectionnez **Partage SMB - Rapide**. Si les Outils de gestion de ressources pour serveur de fichiers sont installés et que vous utilisez des propriétés de gestion des dossiers, sélectionnez plutôt **Partage SMB - Avancé**.
4. Dans la page **Emplacement du partage** , sélectionnez le serveur et le volume sur lesquels vous voulez créer le partage.
5. Dans la page **Nom du partage**, donnez un nom au partage (par exemple, **Utilisateur$** ) dans la zone **Nom du partage**.
    >[!TIP]
    >Lors de la création du partage, masquez-le en plaçant un caractère ```$``` après le nom du partage. Cela masque le partage dans les navigateurs informels.
6. Dans la page **Autres paramètres**, décochez la case Activer la disponibilité continue, si celle-ci est présente. Cochez éventuellement les cases **Activer l’énumération basée sur l’accès** et **Chiffrer l’accès aux données**.
7. Dans la page **Autorisations**, sélectionnez **Personnaliser les autorisations**. La boîte de dialogue Paramètres de sécurité avancés s'affiche.
8. Sélectionnez **Désactiver l’héritage**, puis **Convertir les autorisations héritées en autorisations explicites sur cet objet**.
9. Définissez les autorisations décrites dans le Tableau 1 et illustrées dans la Figure 1. Pour cela, supprimez les autorisations pour les groupes et comptes non listés, puis ajoutez des autorisations spéciales au groupe Utilisateurs de la redirection de dossiers créé à l’étape 1.
    
    ![Définition des autorisations pour le partage de dossiers redirigés](media/deploy-folder-redirection/setting-the-permissions-for-the-redirected-folders-share.png)
    
    **Figure 1** Définition des autorisations pour le partage de dossiers redirigés
10. Si vous choisissez le profil **Partage SMB - Avancé** , dans la page **Propriétés de gestion** , sélectionnez la valeur Utilisation du dossier **Fichiers utilisateur** .
11. Si vous choisissez le profil **Partage SMB - Avancé** , dans la page **Quota** , vous pouvez sélectionner un quota à appliquer aux utilisateurs du partage.
12. Dans la page **Confirmation**, sélectionnez **Créer**.

### <a name="required-permissions-for-the-file-share-hosting-redirected-folders"></a>Autorisations nécessaires pour le partage de fichiers hébergeant les dossiers redirigés

| Compte d’utilisateur  | Accès  | S’applique à  |
| --------- | --------- | --------- |
| Compte d’utilisateur | Accès | S’applique à |
| System     | Contrôle total        |    Ce dossier, ses sous-dossiers et ses fichiers     |
| Administrateurs     | Contrôle total       | Ce dossier uniquement        |
| Propriétaire créateur     |   Contrôle total      |   Sous-dossiers et fichiers uniquement      |
| Groupe de sécurité des utilisateurs devant placer des données sur le partage (Utilisateurs de la redirection de dossiers)     |   Lister le dossier / Lire les données *(Autorisations avancées)* <br /><br />Créer les dossiers / Ajouter les données *(Autorisations avancées)* <br /><br />Lire les attributs *(Autorisations avancées)* <br /><br />Lire les attributs étendus *(Autorisations avancées)* <br /><br />Autorisations de lecture *(Autorisations avancées)*      |  Ce dossier uniquement       |
| Autres groupes et comptes     |  Aucun (supprimer)       |         |

## <a name="step-3-create-a-gpo-for-folder-redirection"></a>Étape 3 : Créer un objet de stratégie de groupe pour la redirection de dossiers

Si vous ne disposez pas encore d’un objet de stratégie de groupe pour les paramètres de redirection de dossiers, utilisez la procédure suivante pour en créer un.

Voici comment créer un objet de stratégie de groupe pour la redirection de dossiers :

1. Ouvrez le Gestionnaire de serveur sur un ordinateur sur lequel la Gestion des stratégies de groupe est installée.
2. Dans le menu **Outils**, sélectionnez **Gestion des stratégies de groupe**.
3. Cliquez avec le bouton droit sur le domaine ou l’unité d’organisation où vous voulez configurer la redirection de dossiers, puis sélectionnez **Créer un objet GPO dans ce domaine, et le lier ici**.
4. Dans la boîte de dialogue **Nouvel objet GPO**, donnez un nom à l’objet de stratégie de groupe (par exemple, **Paramètres de redirection de dossiers**), puis sélectionnez **OK**.
5. Cliquez avec le bouton droit sur l'objet de stratégie de groupe nouvellement créé, puis décochez la case **Lien activé** . Cela évite que l'objet de stratégie de groupe soit appliqué tant que vous n'avez pas terminé de le configurer.
6. Sélectionnez l'objet de stratégie de groupe. Dans la section **Filtrage de sécurité** de l’onglet **Étendue**, sélectionnez **Utilisateurs authentifiés**, puis **Supprimer** pour que l’objet de stratégie de groupe ne soit pas appliqué à tout le monde.
7. Dans la section **Filtrage de sécurité**, sélectionnez **Ajouter**.
8. Dans la boîte de dialogue **Sélectionnez Utilisateurs, Ordinateurs ou Groupes**, tapez le nom du groupe de sécurité créé à l’étape 1 (par exemple, **Utilisateurs de la redirection de dossiers**), puis sélectionnez **OK**.
9. Sélectionnez l’onglet **Délégation**, sélectionnez **Ajouter**, tapez **Utilisateurs authentifiés**, puis sélectionnez **OK** et à nouveau **OK** pour accepter les autorisations de lecture par défaut.
    
    Cette étape est nécessaire en raison des modifications de sécurité apportées dans [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016).

> [!IMPORTANT]
> En raison des modifications de sécurité apportées dans [MS16-072A](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), vous devez maintenant accorder au groupe Utilisateurs authentifiés des autorisations de lecture déléguées sur l’objet de stratégie de groupe Redirection de dossiers pour l’appliquer aux utilisateurs. S’il est déjà appliqué, l’objet de stratégie de groupe est supprimé et les dossiers sont redirigés vers le PC local. Pour plus d’informations, consultez [Deploying Group Policy Security Update MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/).

## <a name="step-4-configure-folder-redirection-with-offline-files"></a>Étape 4 : Configurer la redirection de dossiers avec Fichiers hors connexion

Maintenant que vous disposez d’un objet de stratégie de groupe pour les paramètres de redirection de dossiers, effectuez la procédure suivante pour modifier les paramètres de stratégie de groupe afin d’activer et de configurer la redirection de dossiers.

> [!NOTE]
> La fonctionnalité Fichiers hors connexion est activée par défaut pour les dossiers redirigés sur les ordinateurs clients Windows et désactivée sur les ordinateurs Windows Server, sauf modification par l’utilisateur. Pour utiliser une stratégie de groupe afin de contrôler si la fonctionnalité Fichiers hors connexion est activée, utilisez le paramètre de stratégie **Autoriser ou interdire l’utilisation de la fonctionnalité de fichiers hors connexion**.
> Pour plus d’informations sur d’autres paramètres de stratégie de groupe pour Fichiers hors connexion, consultez [Activer des fonctionnalités Fichiers hors connexion avancées](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn270369(v%3dws.11)>) et [Configuration d’une stratégie de groupe pour Fichiers hors connexion](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc759721(v%3dws.10)>).

Voici comment configurer la redirection de dossiers dans une stratégie de groupe :

1. Dans Gestion des stratégies de groupe, cliquez avec le bouton droit sur l’objet de stratégie de groupe que vous avez créé (par exemple, **Paramètres de redirection de dossiers**), puis sélectionnez **Modifier**.
2. Dans la fenêtre de l’Éditeur de gestion des stratégies de groupe, accédez à **Configuration ordinateur**, **Stratégies**, **Paramètres Windows**, puis à **Redirection de dossiers**.
3. Cliquez avec le bouton droit sur un dossier que vous souhaitez rediriger (par exemple, **documents**), puis sélectionnez **Propriétés**.
4. Dans la boîte de dialogue **Propriétés**, dans la zone **Paramètre**, sélectionnez **De base - Rediriger les dossiers de tout le monde vers le même emplacement**.

    > [!NOTE]
    > Pour appliquer la redirection de dossiers à des ordinateurs clients Windows XP ou Windows Server 2003, sélectionnez l’onglet **Paramètres**, puis cochez la case **Appliquer également la stratégie de redirection aux systèmes d’exploitation Windows 2000, Windows 2000 Server, Windows XP et Windows Server 2003**.

5. Dans la section **Emplacement du dossier cible**, sélectionnez **Créer un dossier pour chaque utilisateur sous le chemin d’accès racine** puis, dans la zone **Chemin racine**, tapez le chemin au partage de fichiers stockant les dossiers redirigés (par exemple : **\\\\fs1.corp.contoso.com\\users$** ).
6. Sélectionnez l’onglet **Paramètres**, puis dans la section **Suppression de stratégie**, sélectionnez éventuellement **Rediriger le dossier vers l’emplacement du profil utilisateur local lorsque la stratégie sera supprimée** (ce paramètre peut rendre le comportement de la redirection des dossiers plus prévisible pour les administrateurs et les utilisateurs).
7. Sélectionnez **OK**, puis **Oui** dans la boîte de dialogue d’avertissement.

## <a name="step-5-enable-the-folder-redirection-gpo"></a>Étape 5 : Activer l’objet de stratégie de groupe de redirection de dossiers

Une fois la configuration des paramètres de stratégie de groupe pour la redirection de dossiers terminée, l’étape suivante consiste à activer l’objet de stratégie de groupe pour qu’il puisse être appliqué aux utilisateurs affectés.

> [!TIP]
> Si vous envisagez d'implémenter la prise en charge des ordinateurs principaux ou d'autres paramètres de stratégie, faites-le maintenant, avant d'activer l'objet de stratégie de groupe. Cela évite que des données utilisateur soient copiées sur des ordinateurs non principaux avant que la prise en charge des ordinateurs principaux soit activée.

Voici comment activer l’objet de stratégie de groupe de redirection de dossiers :

1. Ouvrez Gestion des stratégies de groupe.
2. Cliquez avec le bouton droit sur l’objet de stratégie de groupe que vous avez créé, puis sélectionnez **Lien activé**. Une coche doit apparaître en regard de l’élément de menu.

## <a name="step-6-test-folder-redirection"></a>Étape 6 : Tester la redirection de dossiers

Pour tester la redirection de dossiers, connectez-vous à un ordinateur avec un compte d’utilisateur configuré pour la redirection de dossiers. Vérifiez ensuite que les dossiers et les profils sont redirigés.

Voici comment tester la redirection de dossiers :

1. Connectez-vous à un ordinateur principal (si vous avez activé la prise en charge des ordinateurs principaux) avec un compte d’utilisateur pour lequel vous avez activé la redirection de dossiers.
2. Si l'utilisateur s'est précédemment connecté à l'ordinateur, ouvrez une invite de commandes avec élévation de privilèges, puis tapez la commande suivante pour vérifier que les derniers paramètres de stratégie de groupe sont appliqués à l'ordinateur client :
    
    ```PowerShell
    gpupdate /force
    ```
3. Ouvrez l’Explorateur de fichiers.
4. Cliquez avec le bouton droit sur un dossier redirigé (par exemple, le dossier Mes documents dans la bibliothèque Documents), puis sélectionnez **Propriétés**.
5. Sélectionnez l’onglet **Emplacement**, puis vérifiez que le chemin indique le partage de fichiers que vous avez spécifié au lieu d’un chemin local.

## <a name="appendix-a-checklist-for-deploying-folder-redirection"></a>Annexe A : Liste de vérification du déploiement de la redirection de dossiers

| État           | Action |
| ---              | ---    |
| ☐<br>☐<br>☐    | 1. Préparer le domaine<br>- Joindre les ordinateurs au domaine<br>- Créer des comptes d’utilisateur |
| ☐<br><br><br>   | 2. Créer un groupe de sécurité pour la redirection de dossiers<br>- Nom du groupe :<br>- Membres : |
| ☐<br><br>       | 3. Créer un partage de fichiers pour les dossiers redirigés<br>- Nom du partage de fichiers : |
| ☐<br><br>       | 4. Créer un objet de stratégie de groupe pour la redirection de dossiers<br>- Nom de l’objet de stratégie de groupe : |
| ☐<br><br>☐<br>☐<br>☐<br>☐<br>☐ | 5. Configurer des paramètres de stratégie pour la redirection de dossiers et les fichiers hors connexion<br>- Dossiers redirigés :<br>- Prise en charge de Windows 2000, Windows XP et Windows Server 2003 activée ?<br>- Fonctionnalité Fichiers hors connexion activée ? (activée par défaut sur les ordinateurs clients Windows)<br>- Mode Toujours hors connexion activé ?<br>- Synchronisation des fichiers en arrière-plan activée ?<br>- Déplacement optimisé des dossiers redirigés activé ? |
| ☐<br><br>☐<br><br>☐<br>☐ | 6. (Facultatif) Activer la prise en charge des ordinateurs principaux<br>- Basée sur les ordinateurs ou basée sur les utilisateurs ?<br>- Désigner les ordinateurs principaux pour les utilisateurs<br>- Emplacement des mappages des utilisateurs et des ordinateurs principaux :<br>- (Facultatif) Activer la prise en charge des ordinateurs principaux pour la redirection de dossiers<br>- (Facultatif) Activer la prise en charge des ordinateurs principaux pour les profils utilisateur itinérants |
| ☐         | 7. Activer l’objet de stratégie de groupe de redirection de dossiers |
| ☐         | 8. Tester la redirection de dossiers |

## <a name="change-history"></a>Historique des modifications

Le tableau suivant récapitule certaines des modifications les plus importantes apportées à cette rubrique.

| Date | Description | Raison|
| --- | --- | --- |
| 18 janvier 2017 | Ajout d’une étape à l’[Étape 3 : Créer un objet de stratégie de groupe pour la redirection de dossiers](#step-3-create-a-gpo-for-folder-redirection) permettant de déléguer des autorisations de lecture aux utilisateurs authentifiés, ce qui est maintenant obligatoire en raison d’une mise à jour de sécurité de la stratégie de groupe. | Feedback des clients |

## <a name="more-information"></a>Autres informations

* [Redirection de dossiers, fichiers hors connexion et profils utilisateur itinérants](folder-redirection-rup-overview.md)
* [Déployer des ordinateurs principaux pour la redirection de dossiers et les profils utilisateur itinérants](deploy-primary-computers.md)
* [Activer des fonctionnalités Fichiers hors connexion avancées](enable-always-offline.md)
* [Instruction de prise en charge de Microsoft concernant les données de profil utilisateur répliquées](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
* [Charger indépendamment des applications avec DISM](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
* [Résolution des problèmes de conditionnement, de déploiement et de requête des applications basées sur Windows Runtime](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)
