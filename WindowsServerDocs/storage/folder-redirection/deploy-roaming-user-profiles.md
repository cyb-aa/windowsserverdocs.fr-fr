---
title: Déploiement de profils utilisateur itinérants
TOCTitle: Deploying Roaming User Profiles
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.date: 06/07/2019
ms.author: jgerend
ms.openlocfilehash: 8feed2adb606edfb6068d7fe10c18baf142077ac
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822342"
---
# <a name="deploying-roaming-user-profiles"></a>Déploiement de profils utilisateur itinérants

>S'applique à : Windows 10, Windows 8.1, Windows 8, Windows Server 7, Windows Server 2019, Windows Server 2016, Windows Server (canal semi-annuel), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Cette rubrique décrit comment utiliser Windows Server pour déployer des [profils utilisateur itinérants](folder-redirection-rup-overview.md) sur des ordinateurs clients Windows. Les profils utilisateur itinérants redirigent les profils utilisateur vers un partage de fichiers afin que les utilisateurs reçoivent les mêmes paramètres de système d’exploitation et d’application sur plusieurs ordinateurs.

Pour obtenir la liste des modifications récentes apportées à cette rubrique, consultez la section [Historique des modifications](#change-history) de cette rubrique.

> [!IMPORTANT]
> En raison des modifications de sécurité apportées dans [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016), nous avons mis à jour la section [Étape 4 : Créer éventuellement un objet de stratégie de groupe pour les profils utilisateur itinérants](#step-4-optionally-create-a-gpo-for-roaming-user-profiles) de cette rubrique pour que Windows puisse appliquer correctement la stratégie des profils utilisateur itinérants (et ne pas rétablir les stratégies locales sur les PC affectés).

> [!IMPORTANT]
>  Les personnalisations apportées par l’utilisateur au menu Démarrer sont perdues après une mise à niveau sur place du système d’exploitation dans la configuration suivante :
> - Les utilisateurs sont configurés pour un profil itinérant
> - Les utilisateurs sont autorisés à modifier le menu Démarrer
>
> Le menu Démarrer par défaut de la nouvelle version du système d’exploitation apparaît donc après la mise à niveau sur place du système d’exploitation. Pour obtenir des solutions de contournement, consultez l’[Annexe C : Contournement de la réinitialisation de la disposition du menu Démarrer après une mise à niveau](#appendix-c-working-around-reset-start-menu-layouts-after-upgrades).

## <a name="prerequisites"></a>Prérequis

### <a name="hardware-requirements"></a>Configuration matérielle requise

Les profils utilisateur itinérants nécessitent un ordinateur x64 ou x86. Ils ne sont pas pris en charge par Windows RT.

### <a name="software-requirements"></a>Configuration logicielle requise

Les profils utilisateur itinérants présentent la configuration logicielle requise suivante :

- Si vous déployez les profils utilisateur itinérants avec la redirection de dossiers dans un environnement comportant des profils utilisateur locaux existants, déployez la redirection de dossiers avant les profils d'utilisateur itinérants afin de réduire la taille des profils itinérants. Une fois les dossiers utilisateur existants redirigés avec succès, vous pouvez déployer les profils utilisateur itinérants.
- Pour administrer les profils utilisateur itinérants, vous devez vous connecter en tant que membre du groupe de sécurité Admins du domaine, du groupe de sécurité Administrateurs d'entreprise ou du groupe de sécurité Propriétaires créateurs de la stratégie de groupe.
- Les ordinateurs clients doivent exécuter Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008.
- Les ordinateurs clients doivent être joints aux services de domaine Active Directory (AD DS) que vous gérez.
- Un ordinateur sur lequel la gestion des stratégies de groupe et le Centre d'administration Active Directory sont installés doit être disponible.
- Un serveur de fichiers doit être disponible pour héberger les profils utilisateur itinérants.

    - Si le partage de fichiers utilise des espaces de noms DFS, les dossiers DFS (liens) doivent avoir une seule cible pour éviter que les utilisateurs effectuent des modifications en conflit sur différents serveurs.
    - Si le partage de fichiers utilise la réplication DFS pour répliquer le contenu avec un autre serveur, les utilisateurs doivent pouvoir accéder uniquement au serveur source pour éviter que les utilisateurs effectuent des modifications en conflit sur différents serveurs.
    - Si le partage de fichiers est en cluster, désactivez la disponibilité continue sur le partage de fichiers afin d’éviter les problèmes de performances.
- La prise en charge des ordinateurs principaux dans les profils utilisateur itinérants impose des exigences supplémentaires en ce qui concerne les ordinateurs clients et les schémas Active Directory. Pour plus d’informations, consultez [Déployer des ordinateurs principaux pour la redirection de dossiers et les profils utilisateur itinérants](deploy-primary-computers.md).
- La disposition du menu Démarrer d’un utilisateur n’est pas itinérante sur Windows 10, Windows Server 2019 ou Windows Server 2016 s’il utilise plusieurs PC, le service de rôle Hôte de session Bureau à distance ou un serveur VDI (Virtual Desktop Infrastructure). Pour contourner ce problème, vous pouvez spécifier une disposition de menu Démarrer, comme décrit dans cette rubrique. Vous pouvez également utiliser des disques de profil utilisateur pour rendre les paramètres du menu Démarrer itinérants quand ils sont utilisés avec des serveurs de type Hôte de session à distance ou VDI. Pour plus d’informations, consultez [Easier User Data Management with User Profile Disks in Windows Server 2012](https://blogs.technet.microsoft.com/enterprisemobility/2012/11/13/easier-user-data-management-with-user-profile-disks-in-windows-server-2012/).

### <a name="considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows"></a>Points à prendre en considération lors de l'utilisation des profils utilisateur itinérants sur plusieurs versions de Windows

Si vous décidez d'utiliser les profils utilisateur itinérants sur plusieurs versions de Windows, nous vous recommandons d'effectuer les actions suivantes :

- Configurez Windows pour tenir à jour des versions de profil distinctes pour chaque version de système d'exploitation. Cela permet d'éviter des problèmes indésirables et inattendus tels que l'endommagement de profils.
- Utilisez la redirection de dossiers pour stocker des fichiers utilisateur tels que des documents et des images hors des profils utilisateur. Cela permet que les mêmes fichiers soient disponibles pour les utilisateurs sur différentes versions de système d'exploitation. Cela garantit également des profils de petite taille et des connexions rapides.
- Allouez suffisamment d'espace de stockage pour les profils utilisateur itinérants. Si vous prenez en charge deux versions de système d'exploitation, le nombre de profils double (et par conséquent l'espace total utilisé), car un profil distinct est tenu à jour pour chaque version de système d'exploitation.
- N’utilisez pas de profils utilisateur itinérants sur des ordinateurs Windows Vista/Windows Server 2008 et Windows 7/Windows Server 2008 R2. L’itinérance entre ces versions de système d’exploitation n’est pas prise en charge en raison d’incompatibilités dans les versions de profil.
- Informez vos utilisateurs que les modifications apportées à une version de système d’exploitation ne seront pas conservées sur une autre version de système d’exploitation.
- Quand vous déplacez votre environnement vers une version de Windows qui utilise une version de profil différente (par exemple, de Windows 10 à Windows 10 version 1607 ; consultez l’[Annexe B : Informations de référence sur les versions de profil](#appendix-b-profile-version-reference-information) pour obtenir une liste), les utilisateurs reçoivent un nouveau profil utilisateur itinérant vide. Vous pouvez réduire l’impact de l’obtention d’un nouveau profil à l’aide de la redirection de dossiers pour rediriger les dossiers communs. Aucune méthode n’est prise en charge pour migrer des profils utilisateur itinérants d’une version de profil vers une autre.

## <a name="step-1-enable-the-use-of-separate-profile-versions"></a>Étape 1 : Activer l'utilisation de versions de profil distinctes

Si vous déployez des profils utilisateur itinérants sur des ordinateurs Windows 8.1, Windows 8, Windows Server 2012 R2 ou Windows Server 2012, nous vous recommandons d’apporter certaines modifications à votre environnement Windows avant de procéder au déploiement. Ces modifications permettent de garantir que les futures mises à niveau du système d'exploitation s'effectueront sans problème, et facilitent l'exécution simultanée de plusieurs versions de Windows avec des profils utilisateur itinérants.

Pour apporter ces modifications, procédez comme suit.

1. Téléchargez et installez les mises à jour logicielles appropriées sur tous les ordinateurs sur lesquels vous prévoyez d’utiliser des profils itinérants, obligatoires, super obligatoires ou de domaine par défaut :

    - Windows 8.1 ou Windows Server 2012 R2 : installez la mise à jour logicielle décrite dans l’article [2887595](https://support.microsoft.com/kb/2887595) de la Base de connaissances Microsoft (une fois disponible).
    - Windows 8 ou Windows Server 2012 : installez la mise à jour logicielle décrite dans l’article [2887239](https://support.microsoft.com/kb/2887239) de la Base de connaissances Microsoft.

2. Sur tous les ordinateurs Windows 8.1, Windows 8, Windows Server 2012 R2 ou Windows Server 2012 sur lesquels vous utiliserez des profils utilisateur itinérants, utilisez l’Éditeur du Registre ou une stratégie de groupe pour créer la valeur DWORD de clé de Registre suivante et affectez-lui la valeur `1`. Pour plus d’informations sur la création de clés de Registre à l’aide d’une stratégie de groupe, voir [Configurer un élément de Registre](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>).

    ```
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\ProfSvc\Parameters\UseProfilePathExtensionVersion
    ```

    > [!WARNING]
    > Une modification incorrecte du Registre peut endommager gravement votre système. Avant toute modification du registre, il est conseillé de sauvegarder toutes les données importantes de votre ordinateur.
3. Redémarrez les ordinateurs.

## <a name="step-2-create-a-roaming-user-profiles-security-group"></a>Étape 2 : Créer des groupes de sécurité de profils utilisateur itinérants

Si votre environnement n'est pas déjà configuré avec les profils utilisateur itinérants, la première étape consiste à créer un groupe de sécurité qui contient tous les utilisateurs et/ou ordinateurs auxquels vous voulez appliquer les paramètres de stratégie des profils utilisateur itinérants.

- Les administrateurs de déploiements de profils utilisateur itinérants à usage général créent généralement un groupe de sécurité pour les utilisateurs.
- Les administrateurs de services Bureau à distance ou de déploiements de bureaux virtuels utilisent généralement un groupe de sécurité pour les utilisateurs et les ordinateurs partagés.

Voici comment créer un groupe de sécurité pour les profils utilisateur itinérants :

1. Ouvrez le Gestionnaire de serveur sur un ordinateur sur lequel le Centre d’administration Active Directory est installé.
2. Dans le menu **Outils**, sélectionnez **Centre d’administration Active Directory**. Le Centre d’administration Active Directory s’affiche.
3. Cliquez avec le bouton droit sur l’unité d’organisation ou le domaine appropriés, sélectionnez **Nouveau**, puis **Groupe**.
4. Dans la fenêtre **Créer un groupe** , dans la section **Groupe** , indiquez les paramètres suivants :

    - Dans **Nom du groupe**, tapez le nom du groupe de sécurité, par exemple : **Profils utilisateur itinérants et ordinateurs**.
    - Dans **Étendue du groupe**, sélectionnez **Sécurité**, puis **Globale**.

5. Dans la section **Membres**, sélectionnez **Ajouter**. La boîte de dialogue Sélectionnez Utilisateurs, Contacts, Ordinateurs ou Groupes s’affiche.
6. Si vous voulez inclure des comptes d’ordinateurs dans le groupe de sécurité, sélectionnez **Types d’objets**, cochez la case **Ordinateurs**, puis sélectionnez **OK**.
7. Tapez les noms des utilisateurs, groupes et/ou ordinateurs sur lesquels vous voulez déployer des profils utilisateur itinérants, sélectionnez **OK**, puis à nouveau **OK**.

## <a name="step-3-create-a-file-share-for-roaming-user-profiles"></a>Étape 3 : Créer un partage de fichiers pour les profils utilisateur itinérants

Si vous ne disposez pas déjà d’un partage de fichiers distincts pour les profils utilisateur itinérants (indépendant de tous les partages des dossiers redirigés afin d’éviter la mise en cache involontaire du dossier des profils itinérants), utilisez la procédure suivante pour créer un partage de fichiers sur un serveur Windows Server.

> [!NOTE]
> Certaines fonctionnalités peuvent varier ou ne pas être disponibles selon la version de Windows Server que vous utilisez.

Voici comment créer un partage de fichiers sur Windows Server :

1. Dans le volet de navigation du Gestionnaire de serveur, sélectionnez **Services de fichiers et de stockage**, puis **Partages** pour afficher la page Partages.
2. Dans la vignette Partages, sélectionnez **Tâches**, puis **Nouveau partage**. L'Assistant Nouveau partage s'affiche.
3. Dans la page **Sélectionner un profil**, sélectionnez **Partage SMB - Rapide**. Si les Outils de gestion de ressources pour serveur de fichiers sont installés et que vous utilisez des propriétés de gestion des dossiers, sélectionnez plutôt **Partage SMB - Avancé**.
4. Dans la page **Emplacement du partage** , sélectionnez le serveur et le volume sur lesquels vous voulez créer le partage.
5. Dans la page **Nom du partage** , tapez un nom pour le partage (par exemple, **Profil utilisateur$** ) dans la zone **Nom du partage** .

    > [!TIP]
    > Lors de la création du partage, masquez-le en plaçant un caractère ```$``` après le nom du partage. Cela masque le partage dans les navigateurs informels.

6. Dans la page **Autres paramètres** , décochez la case **Activer la disponibilité continue** , si celle-ci est présente. Vous pouvez cocher les cases **Activer l’énumération basée sur l’accès** et **Chiffrer l’accès aux données** .
7. Dans la page **Autorisations**, sélectionnez **Personnaliser les autorisations**. La boîte de dialogue Paramètres de sécurité avancés s'affiche.
8. Sélectionnez **Désactiver l’héritage**, puis **Convertir les autorisations héritées en autorisations explicites sur cet objet**.
9. Définissez les autorisations comme décrit dans [Autorisations nécessaires pour le partage de fichiers hébergeant les profils utilisateur itinérants](#required-permissions-for-the-file-share-hosting-roaming-user-profiles) et illustré dans la capture d’écran suivante, en supprimant les autorisations pour les groupes et comptes non listés et en ajoutant des autorisations spéciales pour le groupe Profils utilisateur itinérants et ordinateurs créés à l’étape 1.
    
    ![Fenêtre Paramètres de sécurité avancés montrant les autorisations décrites dans le Tableau 1](media/advanced-security-user-profiles.jpg)
    
    **Figure 1** Définition des autorisations pour le partage de profils utilisateur itinérants
10. Si vous choisissez le profil **Partage SMB - Avancé** , dans la page **Propriétés de gestion** , sélectionnez la valeur Utilisation du dossier **Fichiers utilisateur** .
11. Si vous choisissez le profil **Partage SMB - Avancé** , dans la page **Quota** , vous pouvez sélectionner un quota à appliquer aux utilisateurs du partage.
12. Dans la page **Confirmation**, sélectionnez **Créer**.

### <a name="required-permissions-for-the-file-share-hosting-roaming-user-profiles"></a>Autorisations nécessaires pour le partage de fichiers hébergeant les profils utilisateur itinérants

| Compte d’utilisateur | Accès | S’applique à |
|   -   |   -   |   -   |
|   System    |  Contrôle total     |  Ce dossier, ses sous-dossiers et ses fichiers     |
|  Administrateurs     |  Contrôle total     |  Ce dossier uniquement     |
|  Propriétaire créateur     |  Contrôle total     |  Sous-dossiers et fichiers uniquement     |
| Groupe de sécurité des utilisateurs qui doivent placer des données sur le partage (Profils utilisateur itinérants et ordinateurs)      |  Lister le dossier / Lire les données *(Autorisations avancées)* <br />Créer les dossiers / Ajouter les données *(Autorisations avancées)* |  Ce dossier uniquement     |
| Autres groupes et comptes   |  Aucun (supprimer)     |       |

## <a name="step-4-optionally-create-a-gpo-for-roaming-user-profiles"></a>Étape 4 : Créer éventuellement un objet de stratégie de groupe pour les profils utilisateur itinérants

Si un objet de stratégie de groupe n'est pas déjà créé pour les paramètres des profils utilisateur itinérants, utilisez la procédure suivante pour en créer un vide avec les profils utilisateur itinérants. Cet objet de stratégie de groupe vous permet de configurer des paramètres des profils utilisateur itinérants (tels que la prise en charge des ordinateurs principaux, qui est traitée séparément), et peut également être utilisé pour activer les profils utilisateur itinérants sur des ordinateurs, comme c'est généralement le cas lors d'un déploiement dans des environnements de bureaux virtuels ou avec les services Bureau à distance.

Voici comment créer un objet de stratégie de groupe pour les profils utilisateur itinérants :

1. Ouvrez le Gestionnaire de serveur sur un ordinateur sur lequel la Gestion des stratégies de groupe est installée.
2. Dans le menu **Outils**, sélectionnez **Gestion des stratégies de groupe**. La Gestion des stratégies de groupe s'affiche.
3. Cliquez avec le bouton droit sur le domaine ou l’unité d’organisation où vous voulez configurer les profils utilisateur itinérants, puis sélectionnez **Créer un objet GPO dans ce domaine, et le lier ici**.
4. Dans la boîte de dialogue **Nouvel objet GPO**, donnez un nom à l’objet de stratégie de groupe (par exemple, **Paramètres du profil utilisateur itinérant**), puis sélectionnez **OK**.
5. Cliquez avec le bouton droit sur l'objet de stratégie de groupe nouvellement créé, puis décochez la case **Lien activé** . Cela évite que l'objet de stratégie de groupe soit appliqué tant que vous n'avez pas terminé de le configurer.
6. Sélectionnez l'objet de stratégie de groupe. Dans la section **Filtrage de sécurité** de l’onglet **Étendue**, sélectionnez **Utilisateurs authentifiés**, puis **Supprimer** pour que l’objet de stratégie de groupe ne soit pas appliqué à tout le monde.
7. Dans la section **Filtrage de sécurité**, sélectionnez **Ajouter**.
8. Dans la boîte de dialogue **Sélectionnez Utilisateurs, Ordinateurs ou Groupes**, tapez le nom du groupe de sécurité créé à l’étape 1 (par exemple, **Profils utilisateur itinérants et ordinateurs**), puis sélectionnez **OK**.
9. Sélectionnez l’onglet **Délégation**, sélectionnez **Ajouter**, tapez **Utilisateurs authentifiés**, puis sélectionnez **OK** et à nouveau **OK** pour accepter les autorisations de lecture par défaut.
    
    Cette étape est nécessaire en raison des modifications de sécurité apportées dans [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016).

>[!IMPORTANT]
>En raison des modifications de sécurité apportées dans [MS16-072A](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016), vous devez maintenant accorder au groupe Utilisateurs authentifiés des autorisations de lecture déléguées sur l’objet de stratégie de groupe pour l’appliquer aux utilisateurs. S’il est déjà appliqué, l’objet de stratégie de groupe est supprimé et les profils utilisateur sont redirigés vers le PC local. Pour plus d’informations, consultez [Deploying Group Policy Security Update MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/).

## <a name="step-5-optionally-set-up-roaming-user-profiles-on-user-accounts"></a>Étape 5 : Configurer éventuellement les profils utilisateur itinérants sur des comptes d'utilisateur

Si vous déployez les profils utilisateur itinérants sur des comptes d'utilisateur, procédez comme suit pour spécifier les profils utilisateur itinérants pour des comptes d'utilisateur dans les services de domaine Active Directory. Si vous déployez les profils utilisateur itinérants sur des ordinateurs, comme c’est généralement le cas pour les services Bureau à distance ou les déploiements de bureaux virtuels, utilisez plutôt la procédure décrite à l’[Étape 6 : Configurer éventuellement les profils utilisateur itinérants sur des ordinateurs](#step-6-optionally-set-up-roaming-user-profiles-on-computers).

> [!NOTE]
> Si vous configurez les profils utilisateur itinérants sur des comptes d'utilisateur à l'aide d'Active Directory et sur des ordinateurs à l'aide d'une stratégie de groupe, le paramètre de stratégie basé sur l'ordinateur est prioritaire.

Voici comment configurer des profils utilisateur itinérants sur des comptes d’utilisateur :

1. Dans le Centre d'administration Active Directory, accédez au conteneur (ou à l'unité d'organisation) **Utilisateurs** dans le domaine approprié.
2. Sélectionnez tous les utilisateurs auxquels vous voulez affecter un profil utilisateur itinérant, cliquez avec le bouton droit sur ces utilisateurs, puis sélectionnez **Propriétés**.
3. Dans la section **Profil**, cochez la case **Chemin d’accès au profil**, puis entrez le chemin au partage de fichiers où vous voulez stocker le profil utilisateur itinérant de l’utilisateur, suivi de `%username%` (qui est automatiquement remplacé par le nom de l’utilisateur la première fois qu’il se connecte). Par exemple :
    
    `\\fs1.corp.contoso.com\User Profiles$\%username%`
    
    Pour spécifier un profil utilisateur itinérant obligatoire, spécifiez le chemin au fichier NTuser.man précédemment créé ; par exemple, `fs1.corp.contoso.comUser Profiles$default`. Pour plus d’informations, consultez [Créer des profils utilisateur obligatoires](https://docs.microsoft.com/windows/client-management/mandatory-user-profile).
4. Sélectionnez **OK**.

> [!NOTE]
> Par défaut, le déploiement de toutes les applications Windows® Runtime (du Windows Store) est autorisé lors de l'utilisation des profils utilisateur itinérants. Toutefois, lorsque vous utilisez un profil spécial, les applications ne sont pas déployées par défaut. Les profils spéciaux sont des profils utilisateur où les modifications sont supprimées lorsque l'utilisateur se déconnecte :
> <br><br>Pour supprimer des restrictions sur le déploiement d’applications pour des profils spéciaux, activez le paramètre de stratégie **Allow deployment operations in special profiles** (situé dans Configuration de l’ordinateur\Stratégies\Modèles d’administration\Composants Windows\Déploiement de packages d’application). Toutefois, les applications déployées de ce scénario laisseront stockées sur l'ordinateur certaines données qui risquent de s'accumuler si, par exemple, il y a des centaines d'utilisateurs sur un seul et même ordinateur. Pour nettoyer les applications, recherchez ou développez un outil qui utilise l’API [CleanupPackageForUserAsync](https://msdn.microsoft.com/library/windows/apps/windows.management.deployment.packagemanager.cleanuppackageforuserasync.aspx) pour le nettoyage des packages d’applications destiné aux utilisateurs qui n’ont plus de profil sur l’ordinateur.
> <br><br>Pour obtenir des informations générales supplémentaires sur les applications du Windows Store, voir [Gestion de l’accès client au Windows Store](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh832040(v=ws.11)>).

## <a name="step-6-optionally-set-up-roaming-user-profiles-on-computers"></a>Étape 6 : Configurer éventuellement les profils utilisateur itinérants sur des ordinateurs

Si vous déployez les profils utilisateur itinérants sur des ordinateurs, comme c'est généralement le cas pour les services Bureau à distance ou les déploiements de bureaux virtuels, utilisez la procédure suivante. Si vous déployez les profils utilisateur itinérants sur des comptes d’utilisateur, utilisez plutôt la procédure décrite à l’[Étape 5 : Configurer éventuellement les profils utilisateur itinérants sur des comptes d’utilisateur](#step-5-optionally-set-up-roaming-user-profiles-on-user-accounts).

Vous pouvez utiliser une stratégie de groupe pour appliquer les profils utilisateur itinérants aux ordinateurs Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008.

> [!NOTE]
> Si vous configurez les profils utilisateur itinérants sur des ordinateurs à l'aide d'une stratégie de groupe et sur des comptes d'utilisateur à l'aide d'Active Directory, le paramètre de stratégie basé sur l'ordinateur est prioritaire.

Voici comment configurer les profils utilisateur itinérants sur des ordinateurs :

1. Ouvrez le Gestionnaire de serveur sur un ordinateur sur lequel la Gestion des stratégies de groupe est installée.
2. Dans le menu **Outils**, sélectionnez **Gestion des stratégies de groupe**. La Gestion des stratégies de groupe s’affiche.
3. Dans Gestion des stratégies de groupe, cliquez avec le bouton droit sur l’objet de stratégie de groupe créé à l’étape 3 (par exemple, **Paramètres des profils utilisateur itinérants**), puis sélectionnez **Modifier**.
4. Dans la fenêtre Éditeur de gestion des stratégies de groupe, accédez à **Configuration ordinateur**, puis à **Stratégies**, **Modèles d'administration**, **Système**et enfin **Profils utilisateur**.
5. Cliquez avec le bouton droit sur **Définir un chemin d’accès de profil itinérant pour tous les utilisateurs ouvrant une session sur cet ordinateur**, puis sélectionnez **Modifier**.
    > [!TIP]
    > S'il est configuré, le dossier de base d'un utilisateur est le dossier par défaut utilisé par certains programmes tels que Windows PowerShell. Vous pouvez configurer un autre emplacement local ou réseau par utilisateur à l'aide de la section **Dossier de base** des propriétés de compte d'utilisateur dans AD DS. Pour configurer l’emplacement du dossier de base pour tous les utilisateurs d’un ordinateur Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 dans un environnement de bureau virtuel, activez le paramètre de stratégie **Définir le dossier de base de l’utilisateur**, puis spécifiez le partage de fichiers et la lettre de lecteur à mapper (ou spécifiez un dossier local). N'utilisez pas de variables d'environnement ou de points de suspension. L’alias de l’utilisateur est ajouté à la fin du chemin spécifié durant l’authentification de l’utilisateur.
6. Dans la boîte de dialogue **Propriétés**, sélectionnez **Activé**.
7. Dans la zone **Il est recommandé que les utilisateurs ouvrant une session sur cet ordinateur utilisent ce chemin d’accès de profil utilisateur**, entrez le chemin au partage de fichiers où vous voulez stocker le profil utilisateur itinérant de l’utilisateur, suivi de `%username%` (qui est automatiquement remplacé par le nom de l’utilisateur la première fois qu’il se connecte). Par exemple :

    `\\fs1.corp.contoso.com\User Profiles$\%username%`

    Pour spécifier un profil utilisateur itinérant obligatoire, qui est un profil préconfiguré auquel les utilisateurs ne peuvent pas apporter de modifications permanentes (les modifications apportées sont réinitialisées quand l’utilisateur se déconnecte), spécifiez le chemin au fichier NTuser.man précédemment créé ; par exemple, `\\fs1.corp.contoso.com\User Profiles$\default`. Pour plus d’informations, voir [Création d’un profil utilisateur obligatoire](https://docs.microsoft.com/windows/client-management/mandatory-user-profile).
8. Sélectionnez **OK**.

## <a name="step-7-optionally-specify-a-start-layout-for-windows-10-pcs"></a>Étape 7 : Spécifier éventuellement une disposition du menu Démarrer pour les PC Windows 10

Vous pouvez utiliser une stratégie de groupe pour appliquer une disposition spécifique du menu Démarrer à tous les PC. Si les utilisateurs se connectent à plusieurs PC et que vous souhaitez que la disposition du menu Démarrer soit cohérente, vérifiez que l’objet de stratégie de groupe s’applique à tous ces PC.

Pour spécifier une disposition du menu Démarrer, effectuez les étapes suivantes :

1. Mettez à jour vos PC Windows 10 avec Windows 10 version 1607 (également appelée Mise à jour anniversaire) ou plus récente, puis installez la mise à jour cumulative du 14 mars 2017 ([KB4013429](https://support.microsoft.com/kb/4013429)) ou une version plus récente.
2. Créez un fichier XML de disposition complète ou partielle du menu Démarrer. Pour cela, consultez [Personnaliser et exporter la disposition de l’écran de démarrage](https://docs.microsoft.com/windows/configuration/customize-and-export-start-layout).
    * Si vous spécifiez une disposition *complète*, un utilisateur ne peut personnaliser aucune partie du menu Démarrer. Si vous spécifiez une disposition *partielle*, les utilisateurs peuvent personnaliser le menu Démarrer (sauf les groupes verrouillés de vignettes que vous spécifiez). Toutefois, avec une disposition partielle, les personnalisations apportées au menu Démarrer par l’utilisateur ne sont pas itinérantes sur d’autres PC.
3. Utilisez une stratégie de groupe pour appliquer la disposition personnalisée du menu Démarrer à l’objet de stratégie de groupe que vous avez créé pour les profils utilisateur itinérants. Pour cela, consultez [Utiliser une stratégie de groupe pour appliquer une disposition personnalisée de l’écran de démarrage dans un domaine](https://docs.microsoft.com/windows/configuration/customize-windows-10-start-screens-by-using-group-policy#bkmk-domaingpodeployment).
4. Utilisez une stratégie de groupe pour définir la valeur de Registre suivante sur vos PC Windows 10. Pour cela, consultez [Configurer un élément de Registre](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>).

| **Action**   | **Mise à jour**                  |
| ------------ | ------------                |
| Ruche         | **HKEY_LOCAL_MACHINE**      |
| Chemin de la clé     | **Software\Microsoft\Windows\CurrentVersion\Explorer** |
| Nom de valeur   | **SpecialRoamingOverrideAllowed** |
| Type de valeur   | **REG_DWORD**               |
| Données de la valeur   | **1** (ou **0** pour désactiver) |
| Base         | **Décimal**                 |

5. (Facultatif) Activez les optimisations de la première ouverture de session pour accélérer la connexion des utilisateurs. Pour cela, consultez [Appliquer des stratégies pour accélérer les connexions](https://docs.microsoft.com/windows/client-management/mandatory-user-profile#apply-policies-to-improve-sign-in-time).
6. (Facultatif) Réduisez davantage les temps de connexion en supprimant les applications inutiles de l’image de base Windows 10 que vous utilisez pour déployer des PC clients. Windows Server 2019 et Windows Server 2016 n’ayant pas d’applications préprovisionnées, vous pouvez ignorer cette étape sur les images serveur.
    - Pour supprimer des applications, utilisez l’applet de commande [Remove-AppxProvisionedPackage](https://docs.microsoft.com/powershell/module/dism/remove-appxprovisionedpackage?view=win10-ps) dans Windows PowerShell pour désinstaller les applications suivantes. Si vos PC sont déjà déployés, vous pouvez créer un script pour supprimer ces applications à l’aide de l’applet de commande [Remove-AppxPackage](https://docs.microsoft.com/powershell/module/appx/remove-appxpackage?view=win10-ps).
    
      - Microsoft.windowscommunicationsapps\_8wekyb3d8bbwe
      - Microsoft.BingWeather\_8wekyb3d8bbwe
      - Microsoft.DesktopAppInstaller\_8wekyb3d8bbwe
      - Microsoft.Getstarted\_8wekyb3d8bbwe
      - Microsoft.Windows.Photos\_8wekyb3d8bbwe
      - Microsoft.WindowsCamera\_8wekyb3d8bbwe
      - Microsoft.WindowsFeedbackHub\_8wekyb3d8bbwe
      - Microsoft.WindowsStore\_8wekyb3d8bbwe
      - Microsoft.XboxApp\_8wekyb3d8bbwe
      - Microsoft.XboxIdentityProvider\_8wekyb3d8bbwe
      - Microsoft.ZuneMusic\_8wekyb3d8bbwe

>[!NOTE]
>La désinstallation de ces applications permet de réduire les temps de connexion, mais vous pouvez les conserver si votre déploiement en a besoin.

## <a name="step-8-enable-the-roaming-user-profiles-gpo"></a>Étape 8 : Activer les objets de stratégie de groupe des profils utilisateur itinérants

Si vous configurez les profils utilisateur itinérants sur des ordinateurs à l'aide d'une stratégie de groupe, ou si vous personnalisez d'autres paramètres de profil utilisateur itinérant à l'aide d'une stratégie de groupe, l'étape suivante consiste à activer l'objet de stratégie de groupe, en autorisant son application aux utilisateurs affectés.

>[!TIP]
>Si vous envisagez d’implémenter la prise en charge des ordinateurs principaux, faites-le maintenant, avant d’activer l’objet de stratégie de groupe. Cela évite que des données utilisateur soient copiées sur des ordinateurs non principaux avant que la prise en charge des ordinateurs principaux soit activée. Pour obtenir les paramètres de stratégie spécifiques, consultez [Déployer des ordinateurs principaux pour la redirection de dossiers et les profils utilisateur itinérants](deploy-primary-computers.md).

Voici comment activer l’objet de stratégie de groupe des profils utilisateur itinérants :

1. Ouvrez Gestion des stratégies de groupe.
2. Cliquez avec le bouton droit sur l’objet de stratégie de groupe que vous avez créé, puis sélectionnez **Lien activé**. Une coche s'affiche en regard de l'élément de menu.

## <a name="step-9-test-roaming-user-profiles"></a>Étape 9 : Tester les profils utilisateur itinérants

Pour tester les profils utilisateur itinérants, connectez-vous à un ordinateur avec un compte d'utilisateur configuré pour les profils utilisateur itinérants, ou connectez-vous à un ordinateur configuré pour les profils utilisateur itinérants. Vérifiez ensuite que le profil est redirigé.

Voici comment tester les profils utilisateur itinérants :

1. Connectez-vous à un ordinateur principal (si vous avez activé la prise en charge des ordinateurs principaux) avec un compte d'utilisateur pour lequel vous avez activé les profils utilisateur itinérants. Si vous avez activé les profils utilisateur itinérants sur des ordinateurs spécifiques, connectez-vous à l'un de ces ordinateurs.
2. Si l'utilisateur s'est précédemment connecté à l'ordinateur, ouvrez une invite de commandes avec élévation de privilèges, puis tapez la commande suivante pour vérifier que les derniers paramètres de stratégie de groupe sont appliqués à l'ordinateur client :
    
    ```PowerShell
    GpUpdate /Force
    ```
3. Pour vérifier que le profil utilisateur est itinérant, ouvrez le **Panneau de configuration**, sélectionnez **Système et sécurité**, **Système**, **Paramètres système avancés** et **Paramètres** dans la section Profils utilisateur, puis recherchez **Itinérant** dans la colonne **Type**.

## <a name="appendix-a-checklist-for-deploying-roaming-user-profiles"></a>Annexe A : Liste de vérification du déploiement des profils utilisateur itinérants

| État                     | Action                                                |
| ---                        | ------                                                |
| ☐<br>☐<br>☐<br>☐<br>☐   | 1. Préparer le domaine<br>- Joindre les ordinateurs au domaine<br>- Activer l’utilisation de versions de profil distinctes<br>- Créer des comptes d’utilisateur<br>- (Facultatif) Déployer la redirection de dossiers |
| ☐<br><br><br>             | 2. Créer un groupe de sécurité pour les profils utilisateur itinérants<br>- Nom du groupe :<br>- Membres : |
| ☐<br><br>                 | 3. Créer un partage de fichiers pour les profils utilisateur itinérants<br>- Nom du partage de fichiers : |
| ☐<br><br>                 | 4. Créer un objet de stratégie de groupe pour les profils utilisateur itinérants<br>- Nom de l’objet de stratégie de groupe :|
| ☐                         | 5. Configurer les paramètres de stratégie des profils utilisateur itinérants    |
| ☐<br>☐<br>☐              | 6. Activer les profils utilisateur itinérants<br>- Activés dans AD DS sur des comptes d’utilisateur ?<br>- Activés dans une stratégie de groupe sur des comptes d’ordinateur ?<br> |
| ☐                         | 7. (Facultatif) Spécifier une disposition du menu Démarrer obligatoire pour les PC Windows 10 |
| ☐<br>☐<br><br>☐<br><br>☐ | 8. (Facultatif) Activer la prise en charge des ordinateurs principaux<br>- Désigner les ordinateurs principaux pour les utilisateurs<br>- Emplacement des mappages des utilisateurs et des ordinateurs principaux :<br>- (Facultatif) Activer la prise en charge des ordinateurs principaux pour la redirection de dossiers<br>- Basée sur les ordinateurs ou basée sur les utilisateurs ?<br>- (Facultatif) Activer la prise en charge des ordinateurs principaux pour les profils utilisateur itinérants |
| ☐                        | 9. Activer les objets de stratégie de groupe des profils utilisateur itinérants                |
| ☐                        | 10. Tester les profils utilisateur itinérants                         |

## <a name="appendix-b-profile-version-reference-information"></a>Annexe B : Informations de référence sur les versions de profil

Chaque profil a une version de profil qui correspond approximativement à la version de Windows sur laquelle le profil est utilisé. Par exemple, les versions 1703 et 1607 de Windows 10 utilisent la version .V6 du profil. Microsoft crée uniquement une version de profil lorsque cela est nécessaire pour assurer la compatibilité, ce qui explique pourquoi les versions de Windows n’incluent pas toutes une nouvelle version de profil.

Le tableau suivant répertorie l'emplacement des profils utilisateur itinérants sur différentes versions de Windows.

| Version du système d'exploitation | Emplacement des profils utilisateur itinérants |
| --- | --- |
| Windows XP et Windows Server 2003 | ```\\<servername>\<fileshare>\<username>``` |
| Windows Vista et Windows Server 2008 | ```\\<servername>\<fileshare>\<username>.V2``` |
| Windows 7 et Windows Server 2008 R2 | ```\\<servername>\<fileshare>\<username>.V2``` |
| Windows 8 et Windows Server 2012 | ```\\<servername>\<fileshare>\<username>.V3``` (après application de la mise à jour logicielle et de la clé de Registre)<br>```\\<servername>\<fileshare>\<username>.V2``` (avant application de la mise à jour logicielle et de la clé de Registre) |
| Windows 8.1 et Windows Server 2012 R2 | ```\\<servername>\<fileshare>\<username>.V4``` (après application de la mise à jour logicielle et de la clé de Registre)<br>```\\<servername>\<fileshare>\<username>.V2``` (avant application de la mise à jour logicielle et de la clé de Registre) |
| Windows 10 | ```\\<servername>\<fileshare>\<username>.V5``` |
| Windows 10 versions 1703 et 1607 | ```\\<servername>\<fileshare>\<username>.V6``` |

## <a name="appendix-c-working-around-reset-start-menu-layouts-after-upgrades"></a>Annexe C : Contournement de la réinitialisation de la disposition du menu Démarrer après une mise à niveau

Voici quelques façons de contourner la réinitialisation de la disposition du menu Démarrer après une mise à niveau sur place :

- Si un seul utilisateur utilise l’appareil et que l’administrateur informatique utilise une stratégie de déploiement de système d’exploitation managée telle que Configuration Manager, il peut effectuer les opérations suivantes :
    
  1. Exporter la disposition du menu Démarrer avec Export-Startlayout avant la mise à niveau 
  2. Importer la disposition du menu Démarrer avec Import-StartLayout après la phase OOBE mais avant que l’utilisateur ne se connecte  
 
     > [!NOTE] 
     > L’importation d’une disposition du menu Démarrer modifie le profil utilisateur par défaut. Tous les profils utilisateur créés après l’importation obtiennent la disposition du menu Démarrer importée.
 
- Les administrateurs informatiques peuvent choisir de gérer la disposition du menu Démarrer avec une stratégie de groupe. L’utilisation d’une stratégie de groupe fournit une solution de gestion centralisée pour appliquer une disposition standardisée du menu Démarrer aux utilisateurs. Si vous utilisez une stratégie de groupe pour gérer le menu Démarrer, vous avez le choix entre 2 modes : le verrouillage complet et le verrouillage partiel. Le scénario de verrouillage complet empêche l’utilisateur d’apporter des modifications à la disposition du menu Démarrer. Le scénario de verrouillage partiel permet à l’utilisateur d’apporter des modifications à une zone spécifique du menu Démarrer. Pour plus d’informations, consultez [Personnaliser et exporter la disposition de l’écran de démarrage](https://docs.microsoft.com/windows/configuration/customize-and-export-start-layout).
        
   > [!NOTE]
   > Les modifications apportées par l’utilisateur dans le scénario de verrouillage partiel sont toujours perdues durant la mise à niveau.

- Laissez la réinitialisation de la disposition du menu Démarrer se produire et autorisez les utilisateurs finaux à reconfigurer ce menu. Vous pouvez envoyer un e-mail ou toute autre notification aux utilisateurs finaux pour les prévenir que la disposition de leur menu Démarrer sera réinitialisée après la mise à niveau du système d’exploitation afin de minimiser l’impact. 

## <a name="change-history"></a>Historique des modifications

Le tableau suivant récapitule certaines des modifications les plus importantes apportées à cette rubrique.

| Date | Description |Raison|
| --- | ---         | ---   |
| 1er mai 2019 | Ajout de mises à jour pour Windows Server 2019. |
| 10 avril 2018 | Ajout d’une discussion pour comprendre quand les personnalisations apportées au menu Démarrer par l’utilisateur sont perdues après une mise à niveau sur place du système d’exploitation.|Problème connu lié aux légendes. |
| 13 mars 2018 | Mise à jour pour Windows Server 2016. | Abandon de la bibliothèque de versions précédentes et mise à jour pour la version actuelle de Windows Server. |
| 13 avril 2017 | Ajout d’informations sur les profils pour Windows 10 version 1703 et clarification du fonctionnement des versions de profils itinérants lors de la mise à niveau des systèmes d’exploitation. Consultez [Points à prendre en considération lors de l’utilisation des profils utilisateur itinérants sur plusieurs versions de Windows](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows). | Commentaires des clients. |
| 14 mars 2017 | Ajout d’une étape facultative pour spécifier une disposition obligatoire du menu Démarrer pour les PC Windows 10 dans l’[Annexe A : Liste de vérification du déploiement des profils utilisateur itinérants](#appendix-a-checklist-for-deploying-roaming-user-profiles). |Modifications des fonctionnalités dans la dernière mise à jour de Windows. |
| 23 janvier 2017 | Ajout d’une étape à l’[Étape 4 : Créer éventuellement un objet de stratégie de groupe pour les profils utilisateur itinérants](#step-4-optionally-create-a-gpo-for-roaming-user-profiles) permettant de déléguer des autorisations de lecture aux utilisateurs authentifiés, ce qui est maintenant obligatoire en raison d’une mise à jour de sécurité de la stratégie de groupe.|Modifications de sécurité apportées au traitement de la stratégie de groupe. |
| 29 décembre 2016 | Ajout d’un lien à l’[Étape 8 : Activer les objets de stratégie de groupe des profils utilisateur itinérants](#step-8-enable-the-roaming-user-profiles-gpo) pour faciliter l’obtention d’informations sur la façon de définir une stratégie de groupe pour les ordinateurs principaux. Correction également de quelques références aux étapes 5 et 6 avec des numéros incorrects.|Commentaires des clients. |
| 5 décembre 2016 | Ajout d’informations expliquant le problème d’itinérance des paramètres du menu Démarrer. | Commentaires des clients. |
| 6 juillet 2016 | Ajout des suffixes de version de profil Windows 10 à l’[Annexe B : Informations de référence sur les versions de profil](#appendix-b-profile-version-reference-information). Suppression également de Windows XP et de Windows Server 2003 de la liste des systèmes d’exploitation pris en charge. | Ajout de mises à jour pour les nouvelles versions de Windows et suppression d’informations sur les versions de Windows qui ne sont plus prises en charge. |
| 7 juillet 2015 | Ajout d’une exigence et d’une étape pour désactiver la disponibilité continue lors de l’utilisation d’un serveur de fichiers en cluster. | Les partages de fichiers en cluster offrent de meilleures performances pour les écritures de petite taille (dont l’utilisation est courante avec les profils utilisateurs itinérants) lorsque la disponibilité en continu est désactivée. |
| 19 mars 2014 | Mise en majuscules des suffixes de versions de profil (.V2, .V3, .V4) dans l’[Annexe B : Informations de référence sur les versions de profil](#appendix-b-profile-version-reference-information). | Bien que Windows ne soit pas sensible à la casse, si vous utilisez NFS avec le partage de fichiers, il est important que le suffixe du profil soit correctement mis en majuscule. |
| 9 octobre 2013 | Révision pour Windows Server 2012 R2 et Windows 8.1, clarification de certains points et ajout des sections [Points à prendre en considération lors de l’utilisation des profils utilisateur itinérants sur plusieurs versions de Windows](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows) et [Annexe B : Informations de référence sur les versions de profil](#appendix-b-profile-version-reference-information). | Mises à jour pour la nouvelle version ; commentaires des clients. |

## <a name="more-information"></a>Autres informations

- [Déployer la redirection de dossiers, les fichiers hors connexion et les profils utilisateur itinérants](deploy-folder-redirection.md)
- [Déployer des ordinateurs principaux pour la redirection de dossiers et les profils utilisateur itinérants](deploy-primary-computers.md)
- [Implémentation de la gestion des états utilisateur](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc784645(v=ws.10)>)
- [Instruction de prise en charge de Microsoft concernant les données de profil utilisateur répliquées](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
- [Charger indépendamment des applications avec DISM](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
- [Résolution des problèmes de conditionnement, de déploiement et de requête des applications basées sur Windows Runtime](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)