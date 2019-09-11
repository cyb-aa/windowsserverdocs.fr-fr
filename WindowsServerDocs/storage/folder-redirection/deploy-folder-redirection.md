---
title: Déployer la redirection de dossiers avec Fichiers hors connexion
description: Comment utiliser Windows Server pour déployer la redirection de dossiers avec Fichiers hors connexion sur des ordinateurs clients Windows.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: 90b3e3d0b5030f8c0140e54c8b0bf55317437427
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867307"
---
# <a name="deploy-folder-redirection-with-offline-files"></a>Déployer la redirection de dossiers avec Fichiers hors connexion

>S’applique à : Windows 10, Windows 7, Windows 8, Windows 8.1, Windows Vista, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2, Windows Server 2008 R2, Windows Server (canal semi-annuel)

Cette rubrique explique comment utiliser Windows Server pour déployer la redirection de dossiers avec Fichiers hors connexion sur des ordinateurs clients Windows.

Pour obtenir la liste des modifications récentes apportées à cette rubrique, consultez [l’historique des modifications](#change-history).

> [!IMPORTANT]
> En raison des modifications de sécurité apportées à [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), nous avons mis à jour [l’étape 3 : Créez un objet de stratégie de groupe pour](#step-3-create-a-gpo-for-folder-redirection) la redirection de dossiers de cette rubrique afin que Windows puisse appliquer correctement la stratégie de redirection de dossiers (et ne pas restaurer les dossiers redirigés sur les PC affectés).

## <a name="prerequisites"></a>Prérequis

### <a name="hardware-requirements"></a>Configuration matérielle requise

La redirection de dossiers requiert un ordinateur x64 ou x86. Il n’est pas pris en charge par Windows® RT.

### <a name="software-requirements"></a>Configuration logicielle requise

La redirection de dossiers présente la configuration logicielle suivante :

- Pour administrer la redirection de dossiers, vous devez être connecté en tant que membre du groupe de sécurité administrateurs de domaine, du groupe de sécurité administrateurs de l’entreprise ou du groupe de sécurité propriétaires créateurs de stratégie de groupe.
- Les ordinateurs clients doivent exécuter Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server (canal semi-annuel), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008.
- Les ordinateurs clients doivent être joints aux services de domaine Active Directory (AD DS) que vous gérez.
- Un ordinateur sur lequel la gestion des stratégies de groupe et le Centre d'administration Active Directory sont installés doit être disponible.
- Un serveur de fichiers doit être disponible pour héberger les dossiers redirigés.
    - Si le partage de fichiers utilise des espaces de noms DFS, les dossiers DFS (liens) doivent avoir une seule cible pour éviter que les utilisateurs effectuent des modifications en conflit sur différents serveurs.
    - Si le partage de fichiers utilise la réplication DFS pour répliquer le contenu avec un autre serveur, les utilisateurs doivent pouvoir accéder uniquement au serveur source pour éviter que les utilisateurs effectuent des modifications en conflit sur différents serveurs.
    - Lorsque vous utilisez un partage de fichiers en cluster, désactivez la disponibilité continue sur le partage de fichiers afin d’éviter les problèmes de performances liés à la redirection de dossiers et Fichiers hors connexion. En outre, Fichiers hors connexion peut ne pas passer en mode hors connexion pendant 3-6 minutes après qu’un utilisateur a perdu l’accès à un partage de fichiers disponible en continu, ce qui peut entraîner des frustrations pour les utilisateurs qui n’utilisent pas encore le mode toujours hors connexion de Fichiers hors connexion.

> [!NOTE]
> Certaines fonctionnalités plus récentes de la redirection de dossiers ont des exigences supplémentaires en matière d’ordinateur client et de schéma Active Directory. Pour plus d’informations, consultez [déployer des ordinateurs principaux](deploy-primary-computers.md), [désactiver des fichiers hors connexion sur les dossiers](disable-offline-files-on-folders.md), activer le [mode toujours hors connexion](enable-always-offline.md)et [activer le déplacement de dossiers optimisés](enable-optimized-moving.md).

## <a name="step-1-create-a-folder-redirection-security-group"></a>Étape 1 : Créer un groupe de sécurité de redirection de dossiers

Si votre environnement n’est pas déjà configuré avec la redirection de dossiers, la première étape consiste à créer un groupe de sécurité qui contient tous les utilisateurs auxquels vous souhaitez appliquer les paramètres de stratégie de redirection de dossiers.

Voici comment créer un groupe de sécurité pour la redirection de dossiers :

1. Ouvrez Gestionnaire de serveur sur un ordinateur sur lequel Active Directory Administration Center est installé.
2. Dans le menu **Outils** , sélectionnez **Active Directory Centre d’administration**. Le Centre d’administration Active Directory s’affiche.
3. Cliquez avec le bouton droit sur le domaine ou l’unité d’organisation approprié, sélectionnez **nouveau**, puis sélectionnez **groupe**.
4. Dans la fenêtre **Créer un groupe**, dans la section **Groupe**, indiquez les paramètres suivants :
    - Dans **Nom du groupe**, tapez le nom du groupe de sécurité, par exemple : **Utilisateurs de redirection de dossiers**.
    - Dans **étendue du groupe**, sélectionnez **sécurité**, puis sélectionnez **Global**.
5. Dans la section **membres** , sélectionnez **Ajouter**. La boîte de dialogue Sélectionnez Utilisateurs, Contacts, Ordinateurs ou Groupes s’affiche.
6. Tapez les noms des utilisateurs ou des groupes dans lesquels vous souhaitez déployer la redirection de dossiers, sélectionnez **OK**, puis cliquez de nouveau sur **OK** .

## <a name="step-2-create-a-file-share-for-redirected-folders"></a>Étape 2 : Créer un partage de fichiers pour les dossiers redirigés

Si vous ne disposez pas déjà d’un partage de fichiers pour les dossiers redirigés, utilisez la procédure suivante pour créer un partage de fichiers sur un serveur exécutant Windows Server 2012.

> [!NOTE]
> Certaines fonctionnalités peuvent être différentes ou non disponibles si vous créez le partage de fichiers sur un serveur exécutant une autre version de Windows Server.

Voici comment créer un partage de fichiers sur Windows Server 2019, Windows Server 2016 et Windows Server 2012 :

1. Dans le volet de navigation Gestionnaire de serveur, sélectionnez **services de fichiers et de stockage**, puis sélectionnez **partages** pour afficher la page partages.
2. Dans la vignette **partages** , sélectionnez **tâches**, puis sélectionnez **nouveau partage**. L'Assistant Nouveau partage s'affiche.
3. Dans la page **Sélectionner un profil** , sélectionnez **partage SMB – rapide**. Si le serveur de fichiers Gestionnaire des ressources est installé et que vous utilisez les propriétés de gestion des dossiers, sélectionnez **partage SMB-avancé**.
4. Dans la page **Emplacement du partage** , sélectionnez le serveur et le volume sur lesquels vous voulez créer le partage.
5. Dans la page **nom du partage** , tapez un nom pour le partage (par exemple, **utilisateurs $** ) dans la zone **nom du partage** .
    >[!TIP]
    >Lors de la création du partage, masquez-le en plaçant un caractère ```$``` après le nom du partage. Cela masque le partage dans les navigateurs informels.
6. Dans la page **autres paramètres** , désactivez la case à cocher Activer la disponibilité continue, si elle est présente, et activez éventuellement les cases à cocher **activer l’énumération basée sur l’accès** et **chiffrer l’accès aux données** .
7. Sur la page **autorisations** , sélectionnez **personnaliser les autorisations.** . La boîte de dialogue Paramètres de sécurité avancés s'affiche.
8. Sélectionnez **désactiver l’héritage**, puis sélectionnez **convertir les autorisations héritées en autorisations explicites sur cet objet**.
9. Définissez les autorisations comme décrit dans le tableau 1 et illustré à la figure 1, en supprimant les autorisations pour les groupes et comptes non répertoriés, et en ajoutant des autorisations spéciales au groupe utilisateurs de redirection de dossiers que vous avez créé à l’étape 1.
    
    ![Définition des autorisations pour le partage dossiers redirigés](media/deploy-folder-redirection/setting-the-permissions-for-the-redirected-folders-share.png)
    
    **Figure 1** Définition des autorisations pour le partage dossiers redirigés
10. Si vous choisissez le profil **Partage SMB - Avancé** , dans la page **Propriétés de gestion** , sélectionnez la valeur Utilisation du dossier **Fichiers utilisateur** .
11. Si vous choisissez le profil **Partage SMB - Avancé** , dans la page **Quota** , vous pouvez sélectionner un quota à appliquer aux utilisateurs du partage.
12. Dans la page **confirmation** , sélectionnez **créer.**

### <a name="required-permissions-for-the-file-share-hosting-redirected-folders"></a>Autorisations requises pour le partage de fichiers hébergeant des dossiers redirigés

| Compte d’utilisateur  | Access  | S'applique à  |
| --------- | --------- | --------- |
| Compte d’utilisateur | Access | S'applique à |
| System     | Contrôle total        |    Ce dossier, ses sous-dossiers et ses fichiers     |
| Administrateurs     | Contrôle total       | Ce dossier uniquement        |
| Propriétaire créateur     |   Contrôle total      |   Sous-dossiers et fichiers uniquement      |
| Groupe de sécurité des utilisateurs qui doivent placer des données sur le partage (utilisateurs redirection de dossiers)     |   Répertorier le dossier/lire les données *(autorisations avancées)* <br /><br />Créer des dossiers/ajouter des données *(autorisations avancées)* <br /><br />Attributs *de lecture (autorisations avancées)* <br /><br />Lire les attributs étendus *(autorisations avancées)* <br /><br />Autorisations *de lecture (autorisations avancées)*      |  Ce dossier uniquement       |
| Autres groupes et comptes     |  Aucun (supprimer)       |         |

## <a name="step-3-create-a-gpo-for-folder-redirection"></a>Étape 3 : Créer un objet de stratégie de groupe pour la redirection de dossiers

Si vous ne disposez pas déjà d’un objet de stratégie de groupe créé pour les paramètres de redirection de dossiers, utilisez la procédure suivante pour en créer un.

Voici comment créer un objet de stratégie de groupe pour la redirection de dossiers :

1. Ouvrez le Gestionnaire de serveur sur un ordinateur sur lequel la Gestion des stratégies de groupe est installée.
2. Dans le menu **Outils** , sélectionnez **gestion des stratégie de groupe**.
3. Cliquez avec le bouton droit sur le domaine ou l’unité d’organisation dans lequel vous souhaitez configurer la redirection de dossiers, puis sélectionnez **créer un objet de stratégie de groupe dans ce domaine, et le lier ici**.
4. Dans la boîte de dialogue **nouvel objet GPO** , tapez un nom pour l’objet de stratégie de groupe (par exemple, **paramètres de redirection de dossiers**), puis sélectionnez **OK**.
5. Cliquez avec le bouton droit sur l'objet de stratégie de groupe nouvellement créé, puis décochez la case **Lien activé**. Cela évite que l'objet de stratégie de groupe soit appliqué tant que vous n'avez pas terminé de le configurer.
6. Sélectionnez l'objet de stratégie de groupe. Dans la section **filtrage de sécurité** de l’onglet **étendue** , sélectionnez **utilisateurs authentifiés**, puis sélectionnez **supprimer** pour empêcher l’application de l’objet de stratégie de groupe à tout le monde.
7. Dans la section **filtrage de sécurité** , sélectionnez **Ajouter**.
8. Dans la boîte de dialogue **Sélectionner un utilisateur, un ordinateur ou un groupe** , tapez le nom du groupe de sécurité que vous avez créé à l’étape 1 (par exemple, **utilisateurs de redirection de dossiers**), puis sélectionnez **OK**.
9. Sélectionnez l’onglet **délégation** , sélectionnez **Ajouter**, tapez **utilisateurs authentifiés**, sélectionnez **OK**, puis cliquez à nouveau sur **OK** pour accepter les autorisations de lecture par défaut.
    
    Cette étape est nécessaire en raison des modifications de sécurité apportées à [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016).

> [!IMPORTANT]
> En raison des modifications de sécurité apportées à [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), vous devez maintenant accorder au groupe utilisateurs authentifiés des autorisations de lecture déléguées sur l’objet de stratégie de groupe de redirection de dossiers. sinon, l’objet de stratégie de groupe n’est pas appliqué aux utilisateurs, ou s’il est déjà appliqué, l’objet de stratégie de groupe est supprimé, redirection dossiers sur l’ordinateur local. Pour plus d’informations, consultez [déploiement d’stratégie de groupe mise à jour de sécurité MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/).

## <a name="step-4-configure-folder-redirection-with-offline-files"></a>Étape 4 : Configurer la redirection de dossiers avec Fichiers hors connexion

Après avoir créé un objet de stratégie de groupe pour les paramètres de redirection de dossiers, modifiez les paramètres de stratégie de groupe pour activer et configurer la redirection de dossiers, comme indiqué dans la procédure suivante.

> [!NOTE]
> Fichiers hors connexion est activé par défaut pour les dossiers redirigés sur les ordinateurs clients Windows et désactivé sur les ordinateurs exécutant Windows Server, sauf s’ils ont été modifiés par l’utilisateur. Pour utiliser stratégie de groupe pour contrôler si Fichiers hors connexion est activé, utilisez le paramètre **de stratégie de fonctionnalité autoriser ou interdire l’utilisation du fichiers hors connexion** .
> Pour plus d’informations sur les autres Fichiers hors connexion paramètres de stratégie de groupe, consultez [activer la fonctionnalité de fichiers hors connexion avancée](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn270369(v%3dws.11)>)et [Configuration des stratégie de groupe pour fichiers hors connexion](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc759721(v%3dws.10)>).

Voici comment configurer la redirection de dossiers dans stratégie de groupe :

1. Dans stratégie de groupe gestion, cliquez avec le bouton droit sur l’objet de stratégie de groupe que vous avez créé (par exemple, **paramètres de redirection de dossiers**), puis sélectionnez **modifier**.
2. Dans la fenêtre Éditeur de gestion des stratégies de groupe, accédez à **Configuration utilisateur**, puis **stratégies**, **Paramètres Windows**, puis **redirection de dossiers**.
3. Cliquez avec le bouton droit sur un dossier que vous souhaitez rediriger (par exemple, **documents**), puis sélectionnez **Propriétés**.
4. Dans la boîte de dialogue **Propriétés** , dans la zone **paramètre** , sélectionnez de **base-rediriger le dossier de tout le monde vers le même emplacement**.

    > [!NOTE]
    > Pour appliquer la redirection de dossiers aux ordinateurs clients exécutant Windows XP ou Windows Server 2003, sélectionnez l’onglet **paramètres** , puis sélectionnez l’option **appliquer également la stratégie de redirection aux systèmes d’exploitation Windows 2000, Windows 2000 Server, Windows XP et Windows Server 2003** cases à cocher systèmes.

5. Dans la section **emplacement du dossier cible** , sélectionnez **créer un dossier pour chaque utilisateur sous le chemin d’accès racine** , puis dans la zone **chemin d’accès racine** , tapez le chemin d’accès au partage de fichiers qui stocke les dossiers redirigés, par exemple :  **\\ \\ utilisateurs\\FS1.Corp.contoso.com $**
6. Sélectionnez l’onglet **paramètres** , et dans la section **Suppression de stratégie** , sélectionnez éventuellement **Rediriger le dossier vers l’emplacement du profil local local lorsque la stratégie est supprimée** (ce paramètre peut aider à faire en sorte que la redirection de dossiers se comporte davantage prévisible pour les adminisitrators et les utilisateurs).
7. Sélectionnez **OK**, puis cliquez sur **Oui** dans la boîte de dialogue d’avertissement.

## <a name="step-5-enable-the-folder-redirection-gpo"></a>Étape 5 : Activer l’objet de stratégie de groupe de redirection de dossiers

Une fois que vous avez terminé la configuration de la redirection de dossiers stratégie de groupe les paramètres, l’étape suivante consiste à activer l’objet de stratégie de groupe, en lui permettant de l’appliquer aux utilisateurs affectés.

> [!TIP]
> Si vous envisagez d'implémenter la prise en charge des ordinateurs principaux ou d'autres paramètres de stratégie, faites-le maintenant, avant d'activer l'objet de stratégie de groupe. Cela évite que des données utilisateur soient copiées sur des ordinateurs non principaux avant que la prise en charge des ordinateurs principaux soit activée.

Voici comment activer l’objet de stratégie de groupe de redirection de dossiers :

1. Ouvrez Gestion des stratégies de groupe.
2. Cliquez avec le bouton droit sur l’objet de stratégie de groupe que vous avez créé, puis sélectionnez **lien activé**. Une case à cocher s’affiche en regard de l’élément de menu.

## <a name="step-6-test-folder-redirection"></a>Étape 6 : Redirection de dossiers de test

Pour tester la redirection de dossiers, connectez-vous à un ordinateur avec un compte d’utilisateur configuré pour la redirection de dossiers. Vérifiez ensuite que les dossiers et les profils sont redirigés.

Voici comment tester la redirection de dossiers :

1. Connectez-vous à un ordinateur principal (si vous avez activé la prise en charge des ordinateurs principaux) avec un compte d’utilisateur pour lequel vous avez activé la redirection de dossiers.
2. Si l'utilisateur s'est précédemment connecté à l'ordinateur, ouvrez une invite de commandes avec élévation de privilèges, puis tapez la commande suivante pour vérifier que les derniers paramètres de stratégie de groupe sont appliqués à l'ordinateur client :
    
    ```PowerShell
    gpupdate /force
    ```
3. Ouvrez l’Explorateur de fichiers.
4. Cliquez avec le bouton droit sur un dossier redirigé (par exemple, le dossier Mes documents dans la bibliothèque de documents), puis sélectionnez **Propriétés**.
5. Sélectionnez l’onglet **emplacement** , puis vérifiez que le chemin d’accès affiche le partage de fichiers que vous avez spécifié au lieu d’un chemin d’accès local.

## <a name="appendix-a-checklist-for-deploying-folder-redirection"></a>Annexe A : Liste de vérification pour le déploiement de la redirection de dossiers

| Statut           | Action |
| ---              | ---    |
| ☐<br>☐<br>☐    | 1. Préparer le domaine<br>-Joindre les ordinateurs au domaine<br>-Créer des comptes d’utilisateur |
| ☐<br><br><br>   | 2. Créer un groupe de sécurité pour la redirection de dossiers<br>-Nom du groupe :<br>Membres |
| ☐<br><br>       | 3. Créer un partage de fichiers pour les dossiers redirigés<br>-Nom du partage de fichiers : |
| ☐<br><br>       | 4. Créer un objet de stratégie de groupe pour la redirection de dossiers<br>-Nom de l’objet de stratégie de groupe : |
| ☐<br><br>☐<br>☐<br>☐<br>☐<br>☐ | 5. Configurer la redirection de dossiers et les paramètres de stratégie de Fichiers hors connexion<br>-Dossiers redirigés :<br>-Prise en charge de Windows 2000, Windows XP et Windows Server 2003 activée ?<br>-Fichiers hors connexion activé ? (activé par défaut sur les ordinateurs clients Windows)<br>-Le mode toujours hors connexion est-il activé ?<br>-La synchronisation de fichiers en arrière-plan est-elle activée ?<br>-Déplacement optimisé des dossiers redirigés activé ? |
| ☐<br><br>☐<br><br>☐<br>☐ | 6. Facultatif Activer la prise en charge des ordinateurs principaux<br>-Basé sur un ordinateur ou un utilisateur ?<br>-Désigner les ordinateurs principaux pour les utilisateurs<br>-Emplacement des mappages d’utilisateurs et d’ordinateurs principaux :<br>-(Facultatif) activer la prise en charge des ordinateurs principaux pour la redirection de dossiers<br>-(Facultatif) activer la prise en charge des ordinateurs principaux pour les profils utilisateur itinérants |
| ☐         | 7. Activer l’objet de stratégie de groupe de redirection de dossiers |
| ☐         | 8. Redirection de dossiers de test |

## <a name="change-history"></a>Historique des modifications

Le tableau suivant récapitule certaines des modifications les plus importantes apportées à cette rubrique.

| Date | Description | Reason|
| --- | --- | --- |
| 18 janvier 2017 | Ajout d’une étape [à l’étape 3 : Créez un objet de stratégie de groupe pour](#step-3-create-a-gpo-for-folder-redirection) la redirection de dossiers afin de déléguer les autorisations de lecture aux utilisateurs authentifiés, ce qui est maintenant requis en raison d’une stratégie de groupe mise à jour de sécurité. | Retour d'expérience du client |

## <a name="more-information"></a>Plus d’informations

* [Redirection de dossiers, Fichiers hors connexion et profils utilisateur itinérants](folder-redirection-rup-overview.md)
* [Déployer des ordinateurs principaux pour la redirection de dossiers et les profils utilisateur itinérants](deploy-primary-computers.md)
* [Activer la fonctionnalité de Fichiers hors connexion avancée](enable-always-offline.md)
* [Déclaration de support de Microsoft concernant les données de profil utilisateur répliquées](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
* [Applications chargement avec DISM](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
* [Résolution des problèmes d’empaquetage, de déploiement et d’interrogation d’applications basées sur des Windows Runtime](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)