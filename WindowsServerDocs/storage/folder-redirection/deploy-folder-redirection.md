---
title: Déployer la Redirection de dossiers des fichiers hors connexion
description: Comment utiliser Windows Server pour déployer la Redirection de dossiers avec les fichiers hors connexion sur les ordinateurs clients Windows.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 2bb15d5ae29da6c9dbcd6b58af280026d06febc8
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222746"
---
# <a name="deploy-folder-redirection-with-offline-files"></a>Déployer la Redirection de dossiers des fichiers hors connexion

>S’applique à : Windows 10, Windows 7, Windows 8, Windows 8.1, Windows Vista, Windows Server 2019, Windows Server 2016, Windows Server (canal semi-annuel), Windows Server 2012, Windows Server 2012 R2, Windows Server 2008 R2

Cette rubrique décrit comment utiliser Windows Server pour déployer la Redirection de dossiers avec les fichiers hors connexion sur les ordinateurs clients Windows.

Pour obtenir la liste des modifications récentes apportées à ce sujet, consultez [l’historique des modifications](#change-history).

>[!IMPORTANT]
>En raison des modifications de sécurité apportées dans [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), nous avons mis à jour [étape 3 : Créer un objet de stratégie de groupe pour la Redirection de dossiers](#step-3-create-a-gpo-for-folder-redirection) de cette rubrique afin que Windows peuvent correctement appliquer la stratégie de Redirection de dossiers (et pas rétablir les dossiers redirigés sur PC affectés).

## <a name="prerequisites"></a>Prérequis

### <a name="hardware-requirements"></a>Configuration matérielle requise

La Redirection de dossiers nécessite un ordinateur x64 64 ou x86 ; Il n’est pas pris en charge par Windows,® RT.

### <a name="software-requirements"></a>Configuration logicielle requise

La Redirection de dossiers présente la configuration logicielle requise suivante :

- Pour administrer la Redirection de dossiers, vous devez être connecté en tant que membre du groupe de sécurité Administrateurs du domaine, le groupe de sécurité administrateurs d’entreprise ou le groupe de sécurité de groupe Propriétaires créateurs de la stratégie.
- Les ordinateurs clients doivent exécuter Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server (canal semi-annuel), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008.
- Les ordinateurs clients doivent être joints aux services de domaine Active Directory (AD DS) que vous gérez.
- Un ordinateur sur lequel la gestion des stratégies de groupe et le Centre d'administration Active Directory sont installés doit être disponible.
- Un serveur de fichiers doit être disponible pour héberger les dossiers redirigés.
    - Si le partage de fichiers utilise des espaces de noms DFS, les dossiers DFS (liens) doivent avoir une seule cible pour éviter que les utilisateurs effectuent des modifications en conflit sur différents serveurs.
    - Si le partage de fichiers utilise la réplication DFS pour répliquer le contenu avec un autre serveur, les utilisateurs doivent pouvoir accéder uniquement au serveur source pour éviter que les utilisateurs effectuent des modifications en conflit sur différents serveurs.
    - Lorsque vous utilisez un partage de fichiers en cluster, désactivez la disponibilité continue sur le partage de fichiers afin d’éviter les problèmes de performances avec la Redirection de dossiers et fichiers hors connexion. En outre, les fichiers hors connexion ne peut pas passer en mode hors connexion pour 3-6 minutes après qu’un utilisateur perd l’accès à un partage de fichiers disponibles en permanence, ce qui peut nuire aux utilisateurs qui n’utilisent pas le mode toujours hors connexion des fichiers hors connexion.

>[!NOTE]
>Certaines fonctionnalités plus récentes dans la Redirection de dossiers ont ordinateur client supplémentaires et des spécifications de schéma Active Directory. Pour plus d’informations, consultez [déployer des ordinateurs principaux](deploy-primary-computers.md), [désactiver la fonctionnalité fichiers hors connexion sur les dossiers](disable-offline-files-on-folders.md), [activer le mode toujours hors connexion](enable-always-offline.md), et [activer optimisé le déplacement du dossier ](enable-optimized-moving.md).

## <a name="step-1-create-a-folder-redirection-security-group"></a>Étape 1 : Créer un groupe de sécurité de la redirection de dossiers

Si votre environnement n’est pas déjà configuré avec la Redirection de dossiers, la première étape consiste à créer un groupe de sécurité qui contient tous les utilisateurs auxquels vous souhaitez appliquer les paramètres de stratégie de Redirection de dossiers.

Voici comment créer un groupe de sécurité pour la Redirection de dossiers :

1. Ouvrez le gestionnaire Server sur un ordinateur avec le centre d’Administration Active Directory est installé.
2. Sur le **outils** menu, sélectionnez **centre d’Administration Active Directory**. Le Centre d’administration Active Directory s’affiche.
3. Cliquez sur le domaine approprié ou l’unité d’organisation, sélectionnez **New**, puis sélectionnez **groupe**.
4. Dans la fenêtre **Créer un groupe**, dans la section **Groupe**, indiquez les paramètres suivants :
    - Dans **Nom du groupe**, tapez le nom du groupe de sécurité, par exemple : **Les utilisateurs de la Redirection de dossier**.
    - Dans **étendue du groupe**, sélectionnez **sécurité**, puis sélectionnez **Global**.
5. Dans le **membres** section, sélectionnez **ajouter**. La boîte de dialogue Sélectionnez Utilisateurs, Contacts, Ordinateurs ou Groupes s’affiche.
6. Tapez les noms des utilisateurs ou des groupes auxquels vous souhaitez déployer la Redirection de dossiers, sélectionnez **OK**, puis sélectionnez **OK** à nouveau.

## <a name="step-2-create-a-file-share-for-redirected-folders"></a>Étape 2 : Créer un partage de fichiers des dossiers redirigés

Si vous n’avez pas déjà d’un partage de fichiers des dossiers redirigés, procédez comme suit pour créer un partage de fichiers sur un serveur exécutant Windows Server 2012.

>[!NOTE]
>Certaines fonctionnalités peuvent être différentes ou non disponibles si vous créez le partage de fichiers sur un serveur exécutant une autre version de Windows Server.

Voici comment créer un partage de fichiers sur Windows Server 2019, Windows Server 2016 et Windows Server 2012 :

1. Dans le volet de navigation de gestionnaire de serveur, sélectionnez **File and Storage Services**, puis sélectionnez **partages** pour afficher la page partages.
2. Dans le **partages** vignette, sélectionnez **tâches**, puis sélectionnez **nouveau partage**. L'Assistant Nouveau partage s'affiche.
3. Sur le **sélectionner un profil** , sélectionnez **partage SMB – rapide**. Si vous avez File Server Resource Manager est installé et que vous utilisez les propriétés de gestion de dossier, sélectionnez à la place **partage SMB - avancé**.
4. Dans la page **Emplacement du partage** , sélectionnez le serveur et le volume sur lesquels vous voulez créer le partage.
5. Sur le **nom du partage** page, tapez un nom pour le partage (par exemple, **utilisateurs$** ) dans le **nom de partage** boîte.
    >[!TIP]
    >Lors de la création du partage, masquez-le en plaçant un caractère ```$``` après le nom du partage. Cela permet de masquer le partage dans les navigateurs informels.
6. Sur le **autres paramètres** page, désactivez la case à cocher de la disponibilité continue d’activer, le cas échéant et sélectionnez éventuellement le **activer l’énumération basée sur l’accès** et **chiffrer l’accès aux données** cases à cocher.
7. Sur le **autorisations** page, sélectionnez **personnaliser les autorisations...** . La boîte de dialogue Paramètres de sécurité avancés s'affiche.
8. Sélectionnez **désactiver l’héritage**, puis sélectionnez **convertir les autorisations héritées en autorisations explicites sur cet objet**.
9. Définissez les autorisations comme décrit le tableau 1 et illustré dans la Figure 1, supprimant les autorisations pour les groupes et comptes et l’ajout des autorisations spéciales pour le groupe d’utilisateurs de la Redirection de dossiers que vous avez créé à l’étape 1.
    
    ![Définition des autorisations pour le partage des dossiers redirigés](media/deploy-folder-redirection/setting-the-permissions-for-the-redirected-folders-share.png)
    
    **Figure 1** définition des autorisations pour les dossiers redirigés de partage
10. Si vous choisissez le profil **Partage SMB - Avancé** , dans la page **Propriétés de gestion** , sélectionnez la valeur Utilisation du dossier **Fichiers utilisateur** .
11. Si vous choisissez le profil **Partage SMB - Avancé** , dans la page **Quota** , vous pouvez sélectionner un quota à appliquer aux utilisateurs du partage.
12. Sur le **Confirmation** page, sélectionnez **créer.**

### <a name="required-permissions-for-the-file-share-hosting-redirected-folders"></a>Les autorisations requises pour le fichier de partagent les dossiers redirigés d’hébergement


|Compte d’utilisateur  |Accès  |S'applique à  |
|---------|---------|---------|
| Compte d’utilisateur | Accès | S'applique à |
|System     | Contrôle total        |    Ce dossier, ses sous-dossiers et ses fichiers     |
|Administrateurs     | Contrôle total       | Ce dossier uniquement        |
|Propriétaire créateur     |   Contrôle total      |   Sous-dossiers et fichiers uniquement      |
|Groupe de sécurité des utilisateurs qui doivent placer des données sur le partage (utilisateurs de la Redirection de dossier)     |   Liste du dossier / lecture de données *(les autorisations avancées)* <br /><br />Créer des dossiers / Ajout de données *(les autorisations avancées)* <br /><br />Lire les attributs *(les autorisations avancées)* <br /><br />Lecture des attributs étendus *(les autorisations avancées)* <br /><br />Autorisations de lecture *(les autorisations avancées)*      |  Ce dossier uniquement       |
|Autres groupes et comptes     |  Aucun (supprimer)       |         |

## <a name="step-3-create-a-gpo-for-folder-redirection"></a>Étape 3 : Créer un objet de stratégie de groupe pour la Redirection de dossiers

Si vous n’avez pas déjà un objet de stratégie de groupe créé pour les paramètres de la Redirection de dossiers, procédez comme suit pour en créer un.

Voici comment créer un objet de stratégie de groupe pour la Redirection de dossiers :

1. Ouvrez le Gestionnaire de serveur sur un ordinateur sur lequel la Gestion des stratégies de groupe est installée.
2. À partir de la **outils** menu, sélectionnez **Group Policy Management**.
3. Cliquez sur le domaine ou l’unité d’organisation dans laquelle vous souhaitez configurer la Redirection de dossiers, puis sélectionnez **créer un objet GPO dans ce domaine et le lier ici**.
4. Dans le **nouvel objet GPO** boîte de dialogue, tapez un nom pour l’objet de stratégie de groupe (par exemple, **les paramètres de Redirection de dossier**), puis sélectionnez **OK**.
5. Cliquez avec le bouton droit sur l'objet de stratégie de groupe nouvellement créé, puis décochez la case **Lien activé**. Cela évite que l'objet de stratégie de groupe soit appliqué tant que vous n'avez pas terminé de le configurer.
6. Sélectionnez l'objet de stratégie de groupe. Dans le **filtrage de sécurité** section de la **étendue** onglet, sélectionnez **utilisateurs authentifiés**, puis sélectionnez **supprimer** pour empêcher l’objet de stratégie de groupe d’être appliquée à tout le monde.
7. Dans le **filtrage de sécurité** section, sélectionnez **ajouter**.
8. Dans le **sélectionner un utilisateur, ordinateur ou groupe** boîte de dialogue, tapez le nom de la sécurité du groupe que vous avez créé à l’étape 1 (par exemple, **les utilisateurs de la Redirection de dossier**), puis sélectionnez **OK**.
9. Sélectionnez le **délégation** onglet, sélectionnez **ajouter**, type **utilisateurs authentifiés**, sélectionnez **OK**, puis sélectionnez **OK** à nouveau pour accepter la valeur par défaut des autorisations de lecture.
    
    Cette étape est nécessaire en raison des modifications de sécurité apportées dans [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016).

>[!IMPORTANT]
>En raison des modifications de sécurité apportées dans [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), vous devez maintenant donner les autorisations de lecture de délégué de groupe utilisateurs authentifiés pour le GPO de la Redirection de dossier : sinon l’objet de stratégie de groupe ne sont pas appliqué aux utilisateurs, ou si elle est déjà appliquée, l’objet de stratégie de groupe est supprimé, la redirection de dossiers sur le PC local. Pour plus d’informations, consultez [déploiement de groupe de stratégie de sécurité mise à jour MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/).

## <a name="step-4-configure-folder-redirection-with-offline-files"></a>Étape 4 : Configurer la redirection de dossiers avec les fichiers hors connexion

Après avoir créé un objet de stratégie de groupe pour les paramètres de la Redirection de dossiers, modifiez les paramètres de stratégie de groupe pour activer et configurer la Redirection de dossiers, comme indiqué dans la procédure suivante.

>[!NOTE]
>Fichiers hors connexion est activée par défaut pour les dossiers redirigés sur les ordinateurs clients Windows et est désactivée sur les ordinateurs exécutant Windows Server, sauf modification par l’utilisateur. Pour utiliser la stratégie de groupe pour contrôler si les fichiers hors connexion est activée, utilisez le **autoriser ou interdire l’utilisation de la fonctionnalité fichiers hors connexion** paramètre de stratégie.
> Pour plus d’informations sur certaines des autres paramètres de stratégie de groupe de fichiers hors connexion, consultez [activer Advanced hors connexion fichiers fonctionnalité](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn270369(v%3dws.11)>), et [configuration de la stratégie de groupe pour les fichiers hors connexion](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc759721(v%3dws.10)>).

Voici comment configurer la Redirection de dossiers dans la stratégie de groupe :

1. Dans la gestion de stratégie de groupe, cliquez sur l’objet de stratégie de groupe que vous avez créé (par exemple, **les paramètres de Redirection de dossier**), puis sélectionnez **modifier**.
2. Dans la fenêtre Éditeur de gestion de stratégie de groupe, accédez à **Configuration utilisateur**, puis **stratégies**, puis **Windows paramètres**, puis **dossier La redirection**.
3. Cliquez sur un dossier que vous souhaitez rediriger (par exemple, **Documents**), puis sélectionnez **propriétés**.
4. Dans le **propriétés** boîte de dialogue, à partir de la **paramètre** boîte, sélectionnez **base - Rediriger les dossiers de tout le monde vers le même emplacement**.

    > [!NOTE]
    > Pour appliquer la Redirection de dossiers pour les ordinateurs clients exécutant Windows XP ou Windows Server 2003, sélectionnez le **paramètres** onglet et sélectionnez le **également appliquer la stratégie de redirection à Windows 2000, Windows 2000 Server, Windows XP, et Systèmes d’exploitation Windows Server 2003** case à cocher.
5. Dans le **emplacement du dossier cible** section, sélectionnez **créer un dossier pour chaque utilisateur sous le chemin d’accès racine** puis, dans le **chemin d’accès racine** , tapez le chemin d’accès pour le stockage de partage de fichier redirection de dossiers, par exemple :  **\\ \\fs1.corp.contoso.com\\utilisateurs$**
6. Sélectionnez le **paramètres** onglet, puis, dans le **suppression de la stratégie** section, vous pouvez également sélectionner **rediriger le dossier vers l’emplacement du profil utilisateur local lorsque la stratégie est supprimée** (ce paramètre peut aider à rendre la Redirection de dossiers se comportent plus prévisible pour les utilisateurs et adminisitrators).
7. Sélectionnez **OK**, puis sélectionnez **Oui** dans la boîte de dialogue d’avertissement.

## <a name="step-5-enable-the-folder-redirection-gpo"></a>Étape 5 : Activer la Redirection de dossier GPO

Une fois que vous avez terminé la configuration des paramètres de stratégie de groupe de Redirection de dossiers, l’étape suivante consiste à activer l’objet de stratégie de groupe, autorisant à appliquer aux utilisateurs concernés.

>[!TIP]
>Si vous envisagez d'implémenter la prise en charge des ordinateurs principaux ou d'autres paramètres de stratégie, faites-le maintenant, avant d'activer l'objet de stratégie de groupe. Cela évite que des données utilisateur soient copiées sur des ordinateurs non principaux avant que la prise en charge des ordinateurs principaux soit activée.

Voici comment activer le GPO de la Redirection de dossier :

1. Ouvrez Gestion des stratégies de groupe.
2. Avec le bouton droit de l’objet de stratégie de groupe que vous avez créé, puis sélectionnez **lien activé**. Une case à cocher s’affiche en regard de l’élément de menu.

## <a name="step-6-test-folder-redirection"></a>Étape 6 : Tester la Redirection de dossiers

Pour tester la Redirection de dossiers, connectez-vous à un ordinateur avec un compte d’utilisateur configuré pour la Redirection de dossiers. Vérifiez que les dossiers et les profils sont redirigés.

Voici comment tester la Redirection de dossiers :

1. Connectez-vous à un ordinateur principal (si vous avez activé la prise en charge des ordinateurs principaux) avec un compte d’utilisateur pour lequel vous avez activé la Redirection de dossiers.
2. Si l'utilisateur s'est précédemment connecté à l'ordinateur, ouvrez une invite de commandes avec élévation de privilèges, puis tapez la commande suivante pour vérifier que les derniers paramètres de stratégie de groupe sont appliqués à l'ordinateur client :
    
    ```PowerShell
    gpupdate /force
    ```
3. Ouvrez l’Explorateur de fichiers.
4. Cliquez sur un dossier redirigé (par exemple, le dossier Mes Documents dans la bibliothèque de Documents), puis sélectionnez **propriétés**.
5. Sélectionnez le **emplacement** onglet et confirmez que le chemin d’accès affiche le partage de fichiers que vous avez spécifié au lieu d’un chemin d’accès local.

## <a name="appendix-a-checklist-for-deploying-folder-redirection"></a>Annexe A : Liste de vérification pour le déploiement de la Redirection de dossiers

|État|Action|
|:---:|---|
|☐<br>☐<br>☐|1. Préparer le domaine<br>-Joindre des ordinateurs au domaine<br>-Créer des comptes d’utilisateur|
|☐<br><br><br>|2. Créer un groupe de sécurité pour la Redirection de dossiers<br>-Nom du groupe :<br>-Membres :|
|☐<br><br>|3. Créer un partage de fichiers des dossiers redirigés<br>-Nom de partage de fichiers :|
|☐<br><br>|4. Créer un objet de stratégie de groupe pour la Redirection de dossiers<br>-Nom de l’objet stratégie de groupe :|
|☐<br><br>☐<br>☐<br>☐<br>☐<br>☐|5. Configurer les paramètres de stratégie de Redirection de dossiers et fichiers hors connexion<br>-Redirection de dossiers :<br>-Windows 2000, Windows XP et prenant en charge de Windows Server 2003 ?<br>-Fichiers hors connexion activées ? (activé par défaut sur les ordinateurs clients Windows)<br>-Mode toujours hors connexion est activé ?<br>-Synchronisation des fichiers en arrière-plan activée ?<br>-Optimisé le déplacement des dossiers redirigés activé ?|
|☐<br><br>☐<br><br>☐<br>☐|6. (Facultatif) Activer la prise en charge de l’ordinateur principal<br>-Basées sur ordinateur ou utilisateur ?<br>-Désigner les ordinateurs principaux pour les utilisateurs<br>-Location de mappages utilisateurs et ordinateurs principaux :<br>-Prise en charge des ordinateurs principaux (facultatif) activer la Redirection de dossiers<br>-(Facultatif) activer la prise en charge des ordinateurs principaux pour les profils utilisateur itinérants|
|☐|7. Activer la Redirection de dossier GPO|
|☐|8. Tester la Redirection de dossiers|

## <a name="change-history"></a>Historique des modifications

Le tableau suivant récapitule certaines des modifications les plus importantes apportées à cette rubrique.

|Date|Description|Reason|
|---|---|---|
|18 janvier 2017|Ajouté une étape pour [étape 3 : Créer un objet de stratégie de groupe pour la Redirection de dossiers](#step-3-create-a-gpo-for-folder-redirection) pour déléguer des autorisations en lecture aux utilisateurs authentifiés, qui est désormais nécessaire en raison d’une mise à jour de sécurité de stratégie de groupe.|Commentaires des clients.|

## <a name="more-information"></a>Informations supplémentaires

* [La Redirection de dossiers, les fichiers hors connexion et les profils utilisateur itinérants](folder-redirection-rup-overview.md)
* [Déployer des ordinateurs principaux pour la Redirection de dossiers et les profils utilisateur itinérants](deploy-primary-computers.md)
* [Activer la fonctionnalité de fichiers hors connexion avancée](enable-always-offline.md)
* [Instruction de prise en charge de Microsoft autour des données de profil utilisateur répliquées](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
* [Supprimer des applications avec DISM](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
* [Résolution des problèmes d’empaquetage, déploiement et interrogation des applications basées sur le Windows Runtime](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)