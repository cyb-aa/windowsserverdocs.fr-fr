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
ms.openlocfilehash: b7a89ce8d72cf4f060e83b3653b3b2d93eed5cfd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402040"
---
# <a name="deploying-roaming-user-profiles"></a>Déploiement de profils utilisateur itinérants

>S’applique à : Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server (canal semi-annuel), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Cette rubrique explique comment utiliser Windows Server pour déployer des [profils utilisateur itinérants](folder-redirection-rup-overview.md) sur des ordinateurs clients Windows. Les profils utilisateur itinérant redirigent les profils utilisateur vers un partage de fichiers afin que les utilisateurs reçoivent les mêmes paramètres de système d’exploitation et d’application sur plusieurs ordinateurs.

Pour obtenir la liste des modifications récentes apportées à cette rubrique, consultez la section [historique des modifications](#change-history) de cette rubrique.

> [!IMPORTANT]
> En raison des modifications de sécurité apportées à [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016), nous avons mis à jour [l’étape 4 : Si vous le souhaitez, vous pouvez créer un objet](#step-4-optionally-create-a-gpo-for-roaming-user-profiles) de stratégie de groupe pour les profils utilisateur itinérants dans cette rubrique afin que Windows puisse appliquer correctement la stratégie des profils utilisateur itinérants (sans revenir aux stratégies locales sur les PC affectés).

> [!IMPORTANT]
>  Les personnalisations de l’utilisateur à démarrer sont perdues après une mise à niveau sur place du système d’exploitation dans la configuration suivante :
> - Les utilisateurs sont configurés pour un profil itinérant
> - Les utilisateurs sont autorisés à apporter des modifications au démarrage
>
> Par conséquent, le menu Démarrer est rétabli à la valeur par défaut de la nouvelle version du système d’exploitation après la mise à niveau sur place du système d’exploitation. Pour obtenir des solutions de [contournement, consultez l’annexe C : Contournement de la réinitialisation des mises en page du](#appendix-c-working-around-reset-start-menu-layouts-after-upgrades)menu démarrer après des mises à niveau.

## <a name="prerequisites"></a>Prérequis

### <a name="hardware-requirements"></a>Configuration matérielle requise

Les profils utilisateur itinérants requièrent un ordinateur x64 ou x86. Il n’est pas pris en charge par Windows RT.

### <a name="software-requirements"></a>Configuration logicielle requise

Les profils utilisateur itinérants présentent la configuration logicielle requise suivante :

- Si vous déployez les profils utilisateur itinérants avec la redirection de dossiers dans un environnement comportant des profils utilisateur locaux existants, déployez la redirection de dossiers avant les profils d'utilisateur itinérants afin de réduire la taille des profils itinérants. Une fois les dossiers utilisateur existants redirigés avec succès, vous pouvez déployer les profils utilisateur itinérants.
- Pour administrer les profils utilisateur itinérants, vous devez vous connecter en tant que membre du groupe de sécurité Admins du domaine, du groupe de sécurité Administrateurs d'entreprise ou du groupe de sécurité Propriétaires créateurs de la stratégie de groupe.
- Les ordinateurs clients doivent exécuter Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008.
- Les ordinateurs clients doivent être joints aux services de domaine Active Directory (AD DS) que vous gérez.
- Un ordinateur sur lequel la gestion des stratégies de groupe et le Centre d'administration Active Directory sont installés doit être disponible.
- Un serveur de fichiers doit être disponible pour héberger les profils utilisateur itinérants.

    - Si le partage de fichiers utilise des espaces de noms DFS, les dossiers DFS (liens) doivent avoir une seule cible pour éviter que les utilisateurs effectuent des modifications en conflit sur différents serveurs.
    - Si le partage de fichiers utilise la réplication DFS pour répliquer le contenu avec un autre serveur, les utilisateurs doivent pouvoir accéder uniquement au serveur source pour éviter que les utilisateurs effectuent des modifications en conflit sur différents serveurs.
    - Si le partage de fichiers est en cluster, désactivez la disponibilité continue sur le partage de fichiers afin d’éviter les problèmes de performances.
- Pour utiliser la prise en charge des ordinateurs principaux dans les profils utilisateur itinérants, il existe d’autres exigences en matière d’ordinateur client et de schéma Active Directory. Pour plus d’informations, consultez [déployer des ordinateurs principaux pour la redirection de dossiers et les profils utilisateur itinérants](deploy-primary-computers.md).
- La disposition du menu Démarrer d’un utilisateur n’est pas itinérante sur Windows 10, Windows Server 2019 ou Windows Server 2016 s’ils utilisent plusieurs PC, Bureau à distance hôte de session ou un serveur VDI (Virtual Desktop Infrastructure). Pour contourner ce problème, vous pouvez spécifier une disposition de démarrage comme décrit dans cette rubrique. Vous pouvez utiliser des disques de profil utilisateur, qui sont les paramètres du menu démarrer correctement itinérants lorsqu’ils sont utilisés avec des serveurs hôtes de session Bureau à distance ou des serveurs VDI. Pour plus d’informations, consultez [gestion des données d’utilisateur plus faciles avec des disques de profil utilisateur dans Windows Server 2012](https://blogs.technet.microsoft.com/enterprisemobility/2012/11/13/easier-user-data-management-with-user-profile-disks-in-windows-server-2012/).

### <a name="considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows"></a>Points à prendre en considération lors de l'utilisation des profils utilisateur itinérants sur plusieurs versions de Windows

Si vous décidez d'utiliser les profils utilisateur itinérants sur plusieurs versions de Windows, nous vous recommandons d'effectuer les actions suivantes :

- Configurez Windows pour tenir à jour des versions de profil distinctes pour chaque version de système d'exploitation. Cela permet d'éviter des problèmes indésirables et inattendus tels que l'endommagement de profils.
- Utilisez la redirection de dossiers pour stocker des fichiers utilisateur tels que des documents et des images hors des profils utilisateur. Cela permet que les mêmes fichiers soient disponibles pour les utilisateurs sur différentes versions de système d'exploitation. Cela garantit également des profils de petite taille et des connexions rapides.
- Allouez suffisamment d'espace de stockage pour les profils utilisateur itinérants. Si vous prenez en charge deux versions de système d'exploitation, le nombre de profils double (et par conséquent l'espace total utilisé), car un profil distinct est tenu à jour pour chaque version de système d'exploitation.
- N’utilisez pas de profils utilisateur itinérants sur des ordinateurs exécutant Windows Vista/Windows Server 2008 et Windows 7/Windows Server 2008 R2. L’itinérance entre ces versions de système d’exploitation n’est pas prise en charge en raison d’incompatibilités dans leurs versions de profil.
- Informez vos utilisateurs que les modifications apportées à une version du système d’exploitation ne seront pas itinérantes vers une autre version du système d’exploitation.
- Lorsque vous déplacez votre environnement vers une version de Windows qui utilise une autre version de profil (par exemple, de Windows 10 à Windows 10, version [1607), consultez l’annexe B : Informations de référence sur](#appendix-b-profile-version-reference-information) les versions de profil pour une liste), les utilisateurs reçoivent un nouveau profil utilisateur itinérant vide. Vous pouvez réduire l’impact de l’obtention d’un nouveau profil à l’aide de la redirection de dossiers pour rediriger les dossiers communs. Il n’existe aucune méthode prise en charge pour migrer des profils utilisateur itinérants d’une version de profil à une autre.

## <a name="step-1-enable-the-use-of-separate-profile-versions"></a>Étape 1 : Activer l'utilisation de versions de profil distinctes

Si vous déployez des profils utilisateur itinérants sur des ordinateurs exécutant Windows 8.1, Windows 8, Windows Server 2012 R2 ou Windows Server 2012, nous vous recommandons d’apporter quelques modifications à votre environnement Windows avant de procéder au déploiement. Ces modifications permettent de garantir que les futures mises à niveau du système d'exploitation s'effectueront sans problème, et facilitent l'exécution simultanée de plusieurs versions de Windows avec des profils utilisateur itinérants.

Pour apporter ces modifications, procédez comme suit.

1. Téléchargez et installez la mise à jour logicielle appropriée sur tous les ordinateurs sur lesquels vous envisagez d’utiliser des profils itinérants, obligatoires, Super obligatoires ou de domaine par défaut :

    - Windows 8.1 ou Windows Server 2012 R2 : Installez la mise à jour logicielle décrite dans l’article [2887595](http://support.microsoft.com/kb/2887595) de la base de connaissances Microsoft (une fois publiée).
    - Windows 8 ou Windows Server 2012 : installez la mise à jour logicielle décrite dans l’article [2887239](http://support.microsoft.com/kb/2887239) de la Base de connaissances Microsoft.

2. Sur tous les ordinateurs exécutant Windows 8.1, Windows 8, Windows Server 2012 R2 ou Windows Server 2012 sur lesquels vous allez utiliser des profils utilisateur itinérants, utilisez l’éditeur du registre ou stratégie de groupe pour créer la valeur DWORD de clé de Registre `1`suivante et affectez-lui la valeur. Pour plus d’informations sur la création de clés de Registre à l’aide d’une stratégie de groupe, voir [Configurer un élément de Registre](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>).

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

1. Ouvrez Gestionnaire de serveur sur un ordinateur sur lequel Active Directory Administration Center est installé.
2. Dans le menu **Outils** , sélectionnez **Active Directory Centre d’administration**. Le Centre d’administration Active Directory s’affiche.
3. Cliquez avec le bouton droit sur le domaine ou l’unité d’organisation approprié, sélectionnez **nouveau**, puis sélectionnez **groupe**.
4. Dans la fenêtre **Créer un groupe**, dans la section **Groupe**, indiquez les paramètres suivants :

    - Dans **Nom du groupe**, tapez le nom du groupe de sécurité, par exemple : **Profils utilisateur itinérants et ordinateurs**.
    - Dans **étendue du groupe**, sélectionnez **sécurité**, puis sélectionnez **Global**.

5. Dans la section **membres** , sélectionnez **Ajouter**. La boîte de dialogue Sélectionnez Utilisateurs, Contacts, Ordinateurs ou Groupes s’affiche.
6. Si vous souhaitez inclure des comptes d’ordinateur dans le groupe de sécurité, sélectionnez **types d’objets**, activez la case à cocher **ordinateurs** , puis sélectionnez **OK**.
7. Tapez les noms des utilisateurs, groupes et/ou ordinateurs sur lesquels vous souhaitez déployer des profils utilisateur itinérants, sélectionnez **OK**, puis cliquez à nouveau sur **OK** .

## <a name="step-3-create-a-file-share-for-roaming-user-profiles"></a>Étape 3 : Créer un partage de fichiers pour les profils utilisateur itinérants

Si vous ne disposez pas déjà d’un partage de fichiers distinct pour les profils utilisateur itinérants (indépendamment des partages des dossiers redirigés pour empêcher la mise en cache involontaire du dossier des profils itinérants), utilisez la procédure suivante pour créer un partage de fichiers sur un serveur exécutant Windows. Serveurs.

> [!NOTE]
> Certaines fonctionnalités peuvent varier ou ne pas être disponibles en fonction de la version de Windows Server que vous utilisez.

Voici comment créer un partage de fichiers sur Windows Server :

1. Dans le volet de navigation Gestionnaire de serveur, sélectionnez **services de fichiers et de stockage**, puis sélectionnez **partages** pour afficher la page partages.
2. Dans la vignette partages, sélectionnez **tâches**, puis sélectionnez **nouveau partage**. L'Assistant Nouveau partage s'affiche.
3. Dans la page **Sélectionner un profil** , sélectionnez **partage SMB – rapide**. Si le serveur de fichiers Gestionnaire des ressources est installé et que vous utilisez les propriétés de gestion des dossiers, sélectionnez **partage SMB-avancé**.
4. Dans la page **Emplacement du partage** , sélectionnez le serveur et le volume sur lesquels vous voulez créer le partage.
5. Dans la page **Nom du partage**, tapez un nom pour le partage (par exemple, **Profil utilisateur$** ) dans la zone **Nom du partage**.

    > [!TIP]
    > Lors de la création du partage, masquez-le en plaçant un caractère ```$``` après le nom du partage. Cela masque le partage dans les navigateurs informels.

6. Dans la page **Autres paramètres**, décochez la case **Activer la disponibilité continue**, si celle-ci est présente. Vous pouvez cocher les cases **Activer l’énumération basée sur l’accès** et **Chiffrer l’accès aux données**.
7. Sur la page **autorisations** , sélectionnez **personnaliser les autorisations.** . La boîte de dialogue Paramètres de sécurité avancés s'affiche.
8. Sélectionnez **désactiver l’héritage**, puis sélectionnez **convertir les autorisations héritées en autorisations explicites sur cet objet**.
9. Définissez les autorisations comme décrit dans [autorisations requises pour le partage de fichiers hébergeant les profils utilisateur itinérants](#required-permissions-for-the-file-share-hosting-roaming-user-profiles) et illustré dans la capture d’écran suivante, en supprimant les autorisations pour les groupes et comptes non répertoriés, et en ajoutant des autorisations spéciales à l’utilisateur itinérant Profil les utilisateurs et les groupes d’ordinateurs que vous avez créés à l’étape 1.
    
    ![Fenêtre Paramètres de sécurité avancés avec les autorisations, comme décrit dans le tableau 1](media/advanced-security-user-profiles.jpg)
    
    **Figure 1** Définition des autorisations pour le partage de profils utilisateur itinérants
10. Si vous choisissez le profil **Partage SMB - Avancé** , dans la page **Propriétés de gestion** , sélectionnez la valeur Utilisation du dossier **Fichiers utilisateur** .
11. Si vous choisissez le profil **Partage SMB - Avancé** , dans la page **Quota** , vous pouvez sélectionner un quota à appliquer aux utilisateurs du partage.
12. Dans la page **confirmation** , sélectionnez **créer.**

### <a name="required-permissions-for-the-file-share-hosting-roaming-user-profiles"></a>Autorisations requises pour le partage de fichiers hébergeant les profils utilisateur itinérants

| Compte d’utilisateur | Accès | S'applique à |
|   -   |   -   |   -   |
|   System    |  Contrôle total     |  Ce dossier, ses sous-dossiers et ses fichiers     |
|  Administrateurs     |  Contrôle total     |  Ce dossier uniquement     |
|  Propriétaire créateur     |  Contrôle total     |  Sous-dossiers et fichiers uniquement     |
| Groupe de sécurité des utilisateurs qui doivent placer des données sur le partage (Profils utilisateur itinérants et ordinateurs)      |  Répertorier le dossier/lire les données *(autorisations avancées)* <br />Créer des dossiers/ajouter des données *(autorisations avancées)* |  Ce dossier uniquement     |
| Autres groupes et comptes   |  Aucun (supprimer)     |       |

## <a name="step-4-optionally-create-a-gpo-for-roaming-user-profiles"></a>Étape 4 : Créer éventuellement un objet de stratégie de groupe pour les profils utilisateur itinérants

Si un objet de stratégie de groupe n'est pas déjà créé pour les paramètres des profils utilisateur itinérants, utilisez la procédure suivante pour en créer un vide avec les profils utilisateur itinérants. Cet objet de stratégie de groupe vous permet de configurer des paramètres des profils utilisateur itinérants (tels que la prise en charge des ordinateurs principaux, qui est traitée séparément), et peut également être utilisé pour activer les profils utilisateur itinérants sur des ordinateurs, comme c'est généralement le cas lors d'un déploiement dans des environnements de bureaux virtuels ou avec les services Bureau à distance.

Voici comment créer un objet de stratégie de groupe pour les profils utilisateur itinérants :

1. Ouvrez le Gestionnaire de serveur sur un ordinateur sur lequel la Gestion des stratégies de groupe est installée.
2. Dans le menu **Outils** , sélectionnez **gestion des stratégie de groupe**. La Gestion des stratégies de groupe s'affiche.
3. Cliquez avec le bouton droit sur le domaine ou l’unité d’organisation dans lequel vous souhaitez configurer des profils utilisateur itinérants, puis sélectionnez **créer un objet de stratégie de groupe dans ce domaine, et le lier ici**.
4. Dans la boîte de dialogue **nouvel objet GPO** , tapez un nom pour l’objet de stratégie de groupe (par exemple, **paramètres du profil utilisateur itinérant**), puis sélectionnez **OK**.
5. Cliquez avec le bouton droit sur l'objet de stratégie de groupe nouvellement créé, puis décochez la case **Lien activé**. Cela évite que l'objet de stratégie de groupe soit appliqué tant que vous n'avez pas terminé de le configurer.
6. Sélectionnez l'objet de stratégie de groupe. Dans la section **filtrage de sécurité** de l’onglet **étendue** , sélectionnez **utilisateurs authentifiés**, puis sélectionnez **supprimer** pour empêcher l’application de l’objet de stratégie de groupe à tout le monde.
7. Dans la section **filtrage de sécurité** , sélectionnez **Ajouter**.
8. Dans la boîte de dialogue **Sélectionner les utilisateurs, les ordinateurs ou les groupes** , tapez le nom du groupe de sécurité que vous avez créé à l’étape 1 (par exemple, **profils utilisateur itinérants et ordinateurs**), puis sélectionnez **OK**.
9. Sélectionnez l’onglet **délégation** , sélectionnez **Ajouter**, tapez **utilisateurs authentifiés**, sélectionnez **OK**, puis cliquez à nouveau sur **OK** pour accepter les autorisations de lecture par défaut.
    
    Cette étape est nécessaire en raison des modifications de sécurité apportées à [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016).

>[!IMPORTANT]
>En raison des modifications de sécurité apportées à [MS16-072A](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016), vous devez maintenant accorder au groupe utilisateurs authentifiés des autorisations de lecture déléguées sur l’objet de stratégie de groupe. sinon, l’objet de stratégie de groupe n’est pas appliqué aux utilisateurs, ou s’il est déjà appliqué, l’objet de stratégie de groupe est supprimé, redirection des profils utilisateur sur l’ordinateur local. Pour plus d’informations, consultez [déploiement d’stratégie de groupe mise à jour de sécurité MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/).

## <a name="step-5-optionally-set-up-roaming-user-profiles-on-user-accounts"></a>Étape 5 : Configurer éventuellement les profils utilisateur itinérants sur des comptes d'utilisateur

Si vous déployez les profils utilisateur itinérants sur des comptes d'utilisateur, procédez comme suit pour spécifier les profils utilisateur itinérants pour des comptes d'utilisateur dans les services de domaine Active Directory. Si vous déployez des profils utilisateur itinérants sur des ordinateurs, comme c’est généralement le cas pour les déploiements de postes de travail services Bureau à distance ou virtualisés [, utilisez plutôt la procédure décrite à l’étape 6 : Éventuellement, configurez les profils utilisateur itinérants](#step-6-optionally-set-up-roaming-user-profiles-on-computers)sur les ordinateurs.

> [!NOTE]
> Si vous configurez les profils utilisateur itinérants sur des comptes d'utilisateur à l'aide d'Active Directory et sur des ordinateurs à l'aide d'une stratégie de groupe, le paramètre de stratégie basé sur l'ordinateur est prioritaire.

Voici comment configurer des profils utilisateur itinérants sur des comptes d’utilisateur :

1. Dans le Centre d'administration Active Directory, accédez au conteneur (ou à l'unité d'organisation) **Utilisateurs** dans le domaine approprié.
2. Sélectionnez tous les utilisateurs auxquels vous souhaitez affecter un profil utilisateur itinérant, cliquez avec le bouton droit sur les utilisateurs, puis sélectionnez **Propriétés**.
3. Dans la section **Profil** , cochez la case **chemin d’accès au profil :** , puis entrez le chemin d’accès au partage de fichiers où vous voulez stocker le profil utilisateur itinérant de l' `%username%` utilisateur, suivi de (qui est automatiquement remplacé par le nom d’utilisateur le premier heure à laquelle l’utilisateur se connecte). Exemple :
    
    `\\fs1.corp.contoso.com\User Profiles$\%username%`
    
    Pour spécifier un profil utilisateur itinérant obligatoire, spécifiez le chemin d’accès au fichier NTuser. Man que vous avez créé précédemment, par `fs1.corp.contoso.comUser Profiles$default`exemple,. Pour plus d’informations, consultez [Create Mandatory User Profiles](https://docs.microsoft.com/windows/client-management/mandatory-user-profile).
4. Sélectionnez **OK**.

> [!NOTE]
> Par défaut, le déploiement de toutes les applications Windows® Runtime (du Windows Store) est autorisé lors de l'utilisation des profils utilisateur itinérants. Toutefois, lorsque vous utilisez un profil spécial, les applications ne sont pas déployées par défaut. Les profils spéciaux sont des profils utilisateur où les modifications sont supprimées lorsque l'utilisateur se déconnecte :
> <br><br>Pour supprimer des restrictions sur le déploiement d’applications pour des profils spéciaux, activez le paramètre de stratégie **Allow deployment operations in special profiles** (situé dans Configuration de l’ordinateur\Stratégies\Modèles d’administration\Composants Windows\Déploiement de packages d’application). Toutefois, les applications déployées de ce scénario laisseront stockées sur l'ordinateur certaines données qui risquent de s'accumuler si, par exemple, il y a des centaines d'utilisateurs sur un seul et même ordinateur. Pour nettoyer les applications, recherchez ou développez un outil qui utilise l’API [CleanupPackageForUserAsync](https://msdn.microsoft.com/library/windows/apps/windows.management.deployment.packagemanager.cleanuppackageforuserasync.aspx) pour nettoyer les packages d’application pour les utilisateurs qui n’ont plus de profil sur l’ordinateur.
> <br><br>Pour obtenir des informations générales supplémentaires sur les applications du Windows Store, voir [Gestion de l’accès client au Windows Store](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh832040(v=ws.11)>).

## <a name="step-6-optionally-set-up-roaming-user-profiles-on-computers"></a>Étape 6 : Configurer éventuellement les profils utilisateur itinérants sur des ordinateurs

Si vous déployez les profils utilisateur itinérants sur des ordinateurs, comme c'est généralement le cas pour les services Bureau à distance ou les déploiements de bureaux virtuels, utilisez la procédure suivante. Si vous déployez des profils utilisateur itinérants sur des comptes d’utilisateur, utilisez plutôt la procédure [décrite à l’étape 5 : Vous pouvez éventuellement configurer des profils utilisateur itinérants sur des](#step-5-optionally-set-up-roaming-user-profiles-on-user-accounts)comptes d’utilisateur.

Vous pouvez utiliser stratégie de groupe pour appliquer des profils utilisateur itinérants aux ordinateurs exécutant Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008.

> [!NOTE]
> Si vous configurez les profils utilisateur itinérants sur des ordinateurs à l'aide d'une stratégie de groupe et sur des comptes d'utilisateur à l'aide d'Active Directory, le paramètre de stratégie basé sur l'ordinateur est prioritaire.

Voici comment configurer des profils utilisateur itinérants sur des ordinateurs :

1. Ouvrez le Gestionnaire de serveur sur un ordinateur sur lequel la Gestion des stratégies de groupe est installée.
2. Dans le menu **Outils** , sélectionnez **gestion des stratégie de groupe**. La gestion des stratégie de groupe s’affiche.
3. Dans stratégie de groupe gestion, cliquez avec le bouton droit sur l’objet de stratégie de groupe que vous avez créé à l’étape 3 (par exemple, **paramètres des profils utilisateur itinérants**), puis sélectionnez **modifier**.
4. Dans la fenêtre Éditeur de gestion des stratégies de groupe, accédez à **Configuration ordinateur**, puis à **Stratégies**, **Modèles d'administration**, **Système** et enfin **Profils utilisateur**.
5. Cliquez avec le bouton droit sur **définir le chemin d’accès du profil itinérant pour tous les utilisateurs ouvrant une session sur cet ordinateur** , puis sélectionnez **modifier**.
    > [!TIP]
    > S'il est configuré, le dossier de base d'un utilisateur est le dossier par défaut utilisé par certains programmes tels que Windows PowerShell. Vous pouvez configurer un autre emplacement local ou réseau par utilisateur à l'aide de la section **Dossier de base** des propriétés de compte d'utilisateur dans AD DS. Pour configurer l’emplacement du dossier de démarrage pour tous les utilisateurs d’un ordinateur exécutant Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 dans un environnement de bureau virtuel, activez **définir le dossier de démarrage** de l’utilisateur. paramètre de stratégie, puis spécifiez le partage de fichiers et la lettre de lecteur à mapper (ou spécifiez un dossier local). N'utilisez pas de variables d'environnement ou de points de suspension. L’alias de l’utilisateur est ajouté à la fin du chemin d’accès spécifié lors de l’authentification de l’utilisateur.
6. Dans la boîte de dialogue **Propriétés** , sélectionnez **activé** .
7. Dans la zone les **utilisateurs ouvrant une session sur cet ordinateur doivent utiliser ce chemin d’accès de profil itinérant** , entrez le chemin d’accès au partage de fichiers où vous voulez stocker le profil utilisateur itinérant `%username%` de l’utilisateur, suivi de (qui est automatiquement remplacé par le nom d’utilisateur la première fois que l’utilisateur se connecte. Exemple :

    `\\fs1.corp.contoso.com\User Profiles$\%username%`

    Pour spécifier un profil utilisateur itinérant obligatoire, qui est un profil préconfiguré auquel les utilisateurs ne peuvent pas apporter de modifications permanentes (les modifications sont réinitialisées lorsque l’utilisateur se déconnecte), spécifiez le chemin d’accès au fichier NTuser. Man `\\fs1.corp.contoso.com\User Profiles$\default`que vous avez créé précédemment, par exemple,. Pour plus d’informations, voir [Création d’un profil utilisateur obligatoire](https://docs.microsoft.com/windows/client-management/mandatory-user-profile).
8. Sélectionnez **OK**.

## <a name="step-7-optionally-specify-a-start-layout-for-windows-10-pcs"></a>Étape 7 : Éventuellement, vous pouvez spécifier une disposition de démarrage pour les PC Windows 10

Vous pouvez utiliser stratégie de groupe pour appliquer une disposition de menu Démarrer spécifique afin que les utilisateurs voient la même disposition de démarrage sur tous les PC. Si les utilisateurs se connectent à plusieurs PC et que vous souhaitez qu’ils aient une disposition de démarrage cohérente sur les PC, assurez-vous que l’objet de stratégie de groupe s’applique à tous les PC.

Pour spécifier une disposition de démarrage, procédez comme suit :

1. Mettez à jour vos PC Windows 10 vers Windows 10 version 1607 (également appelée mise à jour anniversaire) ou plus récent, et installez la mise à jour cumulative du 14 mars, 2017 ([KB4013429](https://support.microsoft.com/kb/4013429)) ou une version plus récente.
2. Créez un fichier XML de disposition de menu de démarrage complet ou partiel. Pour ce faire, consultez [personnaliser et exporter la disposition de démarrage](https://docs.microsoft.com/windows/configuration/customize-and-export-start-layout).
    * Si vous spécifiez une disposition de démarrage *complète* , un utilisateur ne peut pas personnaliser une partie du menu Démarrer. Si vous spécifiez une disposition de démarrage *partielle* , les utilisateurs peuvent personnaliser tout sauf les groupes verrouillés de vignettes que vous spécifiez. Toutefois, avec une disposition de démarrage partielle, les personnalisations de l’utilisateur dans le menu Démarrer ne sont pas itinérantes sur les autres PC.
3. Utilisez stratégie de groupe pour appliquer la disposition de démarrage personnalisée à l’objet de stratégie de groupe que vous avez créé pour les profils utilisateur itinérants. Pour ce faire, consultez [utiliser stratégie de groupe pour appliquer une disposition de démarrage personnalisée dans un domaine](https://docs.microsoft.com/windows/configuration/customize-windows-10-start-screens-by-using-group-policy#bkmk-domaingpodeployment).
4. Utilisez stratégie de groupe pour définir la valeur de Registre suivante sur vos PC Windows 10. Pour ce faire, consultez [configurer un élément du Registre](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>).

| **Action**   | **Mise à jour**                  |
| ------------ | ------------                |
| Sign         | **HKEY_LOCAL_MACHINE**      |
| Chemin de la clé     | **Software\Microsoft\Windows\CurrentVersion\Explorer** |
| Nom de valeur   | **SpecialRoamingOverrideAllowed** |
| Type de valeur   | **ENREGISTRÉE**               |
| Données de la valeur   | **1** (ou **0** pour désactiver) |
| Base         | **Decimal**                 |

5. Facultatif Activez les optimisations de la première ouverture de session pour accélérer la connexion des utilisateurs. Pour ce faire, consultez [appliquer des stratégies pour améliorer l’heure de connexion](https://docs.microsoft.com/windows/client-management/mandatory-user-profile#apply-policies-to-improve-sign-in-time).
6. Facultatif Réduisez davantage les délais de connexion en supprimant les applications inutiles de l’image de base Windows 10 que vous utilisez pour déployer des ordinateurs clients. Windows Server 2019 et Windows Server 2016 n’ont pas d’applications préconfigurées. vous pouvez donc ignorer cette étape sur les images serveur.
    - Pour supprimer des applications, utilisez l’applet de commande [Remove-AppxProvisionedPackage](https://docs.microsoft.com/powershell/module/dism/remove-appxprovisionedpackage?view=win10-ps) dans Windows PowerShell pour désinstaller les applications suivantes. Si vos PC sont déjà déployés, vous pouvez générer un script pour la suppression de ces applications à l’aide de [Remove-AppxPackage](https://docs.microsoft.com/powershell/module/appx/remove-appxpackage?view=win10-ps).
    
      - Microsoft. windowscommunicationsapps\_8wekyb3d8bbwe
      - Microsoft. BingWeather\_8wekyb3d8bbwe
      - Microsoft. DesktopAppInstaller\_8wekyb3d8bbwe
      - Microsoft. getstarted\_8wekyb3d8bbwe
      - Microsoft. Windows. photos\_8wekyb3d8bbwe
      - Microsoft. WindowsCamera\_8wekyb3d8bbwe
      - Microsoft. WindowsFeedbackHub\_8wekyb3d8bbwe
      - Microsoft. WindowsStore\_8wekyb3d8bbwe
      - Microsoft. XboxApp\_8wekyb3d8bbwe
      - Microsoft. XboxIdentityProvider\_8wekyb3d8bbwe
      - Microsoft. ZuneMusic\_8wekyb3d8bbwe

>[!NOTE]
>La désinstallation de ces applications réduit les temps de connexion, mais vous pouvez les conserver si votre déploiement en a besoin.

## <a name="step-8-enable-the-roaming-user-profiles-gpo"></a>Étape 8 : Activer les objets de stratégie de groupe des profils utilisateur itinérants

Si vous configurez les profils utilisateur itinérants sur des ordinateurs à l'aide d'une stratégie de groupe, ou si vous personnalisez d'autres paramètres de profil utilisateur itinérant à l'aide d'une stratégie de groupe, l'étape suivante consiste à activer l'objet de stratégie de groupe, en autorisant son application aux utilisateurs affectés.

>[!TIP]
>Si vous envisagez d’implémenter la prise en charge des ordinateurs principaux, faites-le maintenant avant d’activer l’objet de stratégie de groupe. Cela évite que des données utilisateur soient copiées sur des ordinateurs non principaux avant que la prise en charge des ordinateurs principaux soit activée. Pour obtenir les paramètres de stratégie spécifiques, consultez [déployer des ordinateurs principaux pour la redirection de dossiers et les profils utilisateur itinérants](deploy-primary-computers.md).

Voici comment activer l’objet de stratégie de groupe des profils utilisateur itinérants :

1. Ouvrez Gestion des stratégies de groupe.
2. Cliquez avec le bouton droit sur l’objet de stratégie de groupe que vous avez créé, puis sélectionnez **lien activé**. Une coche s'affiche en regard de l'élément de menu.

## <a name="step-9-test-roaming-user-profiles"></a>Étape 9 : Tester les profils utilisateur itinérants

Pour tester les profils utilisateur itinérants, connectez-vous à un ordinateur avec un compte d'utilisateur configuré pour les profils utilisateur itinérants, ou connectez-vous à un ordinateur configuré pour les profils utilisateur itinérants. Vérifiez ensuite que le profil est redirigé.

Voici comment tester les profils utilisateur itinérants :

1. Connectez-vous à un ordinateur principal (si vous avez activé la prise en charge des ordinateurs principaux) avec un compte d'utilisateur pour lequel vous avez activé les profils utilisateur itinérants. Si vous avez activé les profils utilisateur itinérants sur des ordinateurs spécifiques, connectez-vous à l'un de ces ordinateurs.
2. Si l'utilisateur s'est précédemment connecté à l'ordinateur, ouvrez une invite de commandes avec élévation de privilèges, puis tapez la commande suivante pour vérifier que les derniers paramètres de stratégie de groupe sont appliqués à l'ordinateur client :
    
    ```PowerShell
    GpUpdate /Force
    ```
3. Pour confirmer que le profil utilisateur est itinérant, ouvrez le **panneau**de configuration, sélectionnez **système et sécurité**, sélectionnez **système**, sélectionnez **paramètres système avancés**, sélectionnez **paramètres** dans la section profils utilisateur, puis recherchez  **Itinérance** dans la colonne **type** .

## <a name="appendix-a-checklist-for-deploying-roaming-user-profiles"></a>Annexe A : Liste de vérification du déploiement des profils utilisateur itinérants

| Statut                     | Action                                                |
| ---                        | ------                                                |
| ☐<br>☐<br>☐<br>☐<br>☐   | 1. Préparer le domaine<br>-Joindre les ordinateurs au domaine<br>-Activer l’utilisation de versions de profil distinctes<br>-Créer des comptes d’utilisateur<br>-(Facultatif) déployer la redirection de dossiers |
| ☐<br><br><br>             | 2. Créer un groupe de sécurité pour les profils utilisateur itinérants<br>-Nom du groupe :<br>Membres |
| ☐<br><br>                 | 3. Créer un partage de fichiers pour les profils utilisateur itinérants<br>-Nom du partage de fichiers : |
| ☐<br><br>                 | 4. Créer un objet de stratégie de groupe pour les profils utilisateur itinérants<br>-Nom de l’objet de stratégie de groupe :|
| ☐                         | 5. Configurer les paramètres de stratégie des profils utilisateur itinérants    |
| ☐<br>☐<br>☐              | 6. Activer les profils utilisateur itinérants<br>-Activé dans AD DS sur les comptes d’utilisateurs ?<br>-Activé dans stratégie de groupe sur les comptes d’ordinateurs ?<br> |
| ☐                         | 7. Facultatif Spécifier une disposition de démarrage obligatoire pour les PC Windows 10 |
| ☐<br>☐<br><br>☐<br><br>☐ | 8. Facultatif Activer la prise en charge des ordinateurs principaux<br>-Désigner les ordinateurs principaux pour les utilisateurs<br>-Emplacement des mappages d’utilisateurs et d’ordinateurs principaux :<br>-(Facultatif) activer la prise en charge des ordinateurs principaux pour la redirection de dossiers<br>-Basé sur un ordinateur ou un utilisateur ?<br>-(Facultatif) activer la prise en charge des ordinateurs principaux pour les profils utilisateur itinérants |
| ☐                        | 9. Activer les objets de stratégie de groupe des profils utilisateur itinérants                |
| ☐                        | 10. Tester les profils utilisateur itinérants                         |

## <a name="appendix-b-profile-version-reference-information"></a>Annexe B : Informations de référence sur les versions de profil

Chaque profil a une version de profil qui correspond approximativement à la version de Windows sur laquelle le profil est utilisé. Par exemple, Windows 10, version 1703 et version 1607 utilisent tous deux le. Version du profil V6. Microsoft crée une nouvelle version de profil uniquement lorsque cela est nécessaire pour maintenir la compatibilité, ce qui explique pourquoi, dans toutes les versions de Windows, il n’y a pas de nouvelle version de profil.

Le tableau suivant répertorie l'emplacement des profils utilisateur itinérants sur différentes versions de Windows.

| Version du système d'exploitation | Emplacement des profils utilisateur itinérants |
| --- | --- |
| Windows XP et Windows Server 2003 | ```\\<servername>\<fileshare>\<username>``` |
| Windows Vista et Windows Server 2008 | ```\\<servername>\<fileshare>\<username>.V2``` |
| Windows 7 et Windows Server 2008 R2 | ```\\<servername>\<fileshare>\<username>.V2``` |
| Windows 8 et Windows Server 2012 | ```\\<servername>\<fileshare>\<username>.V3```(après application de la mise à jour logicielle et de la clé de registre)<br>```\\<servername>\<fileshare>\<username>.V2```(avant l’application de la mise à jour logicielle et de la clé de registre) |
| Windows 8.1 et Windows Server 2012 R2 | ```\\<servername>\<fileshare>\<username>.V4```(après application de la mise à jour logicielle et de la clé de registre)<br>```\\<servername>\<fileshare>\<username>.V2```(avant l’application de la mise à jour logicielle et de la clé de registre) |
| Windows 10 | ```\\<servername>\<fileshare>\<username>.V5``` |
| Windows 10, version 1703 et version 1607 | ```\\<servername>\<fileshare>\<username>.V6``` |

## <a name="appendix-c-working-around-reset-start-menu-layouts-after-upgrades"></a>Annexe C : Contournement de la réinitialisation des mises en page du menu démarrer après des mises à niveau

Voici quelques façons de contourner les dispositions du menu démarrer en réinitialisation après une mise à niveau sur place :

- Si un seul utilisateur utilise l’appareil et que l’administrateur informatique utilise une stratégie de déploiement de système d’exploitation gérée telle que SCCM, il peut effectuer les opérations suivantes :
    
  1. Exporter la disposition du menu Démarrer avec Export-Startlayout avant la mise à niveau 
  2. Importer la disposition du menu Démarrer avec Import-StartLayout après OOBE mais avant que l’utilisateur se connecte  
 
     > [!NOTE] 
     > L’importation d’un StartLayout modifie le profil utilisateur par défaut. Tous les profils utilisateur créés après l’importation obtiennent la disposition de démarrage importée.
 
- Les administrateurs informatiques peuvent choisir de gérer la disposition du début avec stratégie de groupe. L’utilisation de stratégie de groupe fournit une solution de gestion centralisée pour appliquer une disposition de démarrage standardisée aux utilisateurs. Il existe 2 modes d’utilisation des modes de stratégie de groupe pour démarrer la gestion. Verrouillage complet et verrouillage partiel. Le scénario de verrouillage complet empêche l’utilisateur d’apporter des modifications à la disposition du démarrage. Le scénario de verrouillage partiel permet à l’utilisateur d’apporter des modifications à une zone de démarrage spécifique. Pour plus d’informations, consultez [personnaliser et exporter la disposition de démarrage](https://docs.microsoft.com/windows/configuration/customize-and-export-start-layout).
        
   > [!NOTE]
   > Les modifications apportées à l’utilisateur dans le scénario de verrouillage partiel seront toujours perdues pendant la mise à niveau.

- Laissez la réinitialisation de la disposition de démarrage se produire et autorisez les utilisateurs finaux à reconfigurer le démarrage. Un courrier électronique de notification ou une autre notification peut être envoyé aux utilisateurs finaux pour s’attendre à ce que leurs mises en page de démarrage soient réinitialisées après la mise à niveau du système d’exploitation vers un impact réduit. 

# <a name="change-history"></a>Historique des modifications

Le tableau suivant récapitule certaines des modifications les plus importantes apportées à cette rubrique.

| Date | Description |Reason|
| --- | ---         | ---   |
| 1er mai, 2019 | Ajout de mises à jour pour Windows Server 2019 |
| 10 avril 2018 | Ajout d’une discussion sur le moment où les personnalisations de l’utilisateur à démarrer sont perdues après une mise à niveau sur place du système d’exploitation|Problème connu de la légende. |
| Le 13 mars 2018 | Mise à jour pour Windows Server 2016 | Déplacé de la bibliothèque de versions précédentes et mis à jour pour la version actuelle de Windows Server. |
| 13 avril 2017 | Ajout d’informations de profil pour Windows 10, version 1703 et clarification du fonctionnement des versions de profils itinérants lors de la mise à niveau des systèmes d’exploitation : consultez [considérations relatives à l’utilisation de profils utilisateur itinérants sur plusieurs versions de Windows](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows). | Commentaires des clients. |
| 14 mars, 2017 | Ajout d’une étape facultative pour spécifier une disposition de démarrage obligatoire pour les [PC Windows 10 dans l’annexe a : Liste de vérification pour le déploiement de profils](#appendix-a-checklist-for-deploying-roaming-user-profiles)utilisateur itinérants. |Modifications des fonctionnalités dans la dernière version de Windows Update. |
| 23 janvier, 2017 | Ajout d’une étape [à l’étape 4 : Vous pouvez également créer un objet de stratégie de groupe](#step-4-optionally-create-a-gpo-for-roaming-user-profiles) pour les profils utilisateur itinérants afin de déléguer des autorisations de lecture aux utilisateurs authentifiés, ce qui est maintenant requis en raison d’une stratégie de groupe mise à jour de sécurité.|Modifications de sécurité apportées au traitement de stratégie de groupe. |
| 29 décembre 2016 | Ajout d’un lien [à l’étape 8 : Activez l’objet de stratégie de groupe](#step-8-enable-the-roaming-user-profiles-gpo) profils utilisateur itinérants pour faciliter l’obtention d’informations sur la façon de définir des stratégie de groupe pour les ordinateurs principaux. Correction également de quelques références aux étapes 5 et 6 qui étaient erronées.|Commentaires des clients. |
| 5 décembre, 2016 | Ajout d’informations expliquant le problème d’itinérance des paramètres du menu Démarrer. | Commentaires des clients. |
| 6 juillet, 2016 | Ajout des suffixes de version de profil [Windows 10 dans l’annexe B : Informations de référence sur](#appendix-b-profile-version-reference-information)les versions de profil. A également supprimé Windows XP et Windows Server 2003 de la liste des systèmes d’exploitation pris en charge. | Des mises à jour pour les nouvelles versions de Windows et des informations sur les versions de Windows qui ne sont plus prises en charge. |
| 7 juillet 2015 | Ajout d’une exigence et d’une étape pour désactiver la disponibilité continue lors de l’utilisation d’un serveur de fichiers en cluster. | Les partages de fichiers en cluster offrent de meilleures performances pour les écritures de petite taille (dont l’utilisation est courante avec les profils utilisateurs itinérants) lorsque la disponibilité en continu est désactivée. |
| 19 mars 2014 | Suffixes de version de profil en majuscules (. V2,. V3,. V4) dans [l’annexe B : Informations de référence sur](#appendix-b-profile-version-reference-information)les versions de profil. | Bien que Windows ne respecte pas la casse, si vous utilisez NFS avec le partage de fichiers, il est important de respecter la casse correcte (en majuscules) pour le suffixe de profil. |
| 9 octobre 2013 | Révision de Windows Server 2012 R2 et Windows 8.1, clarification de quelques éléments et ajout de [considérations lors de l’utilisation de profils utilisateur itinérants sur plusieurs versions de Windows](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows) et [annexe B : Sections informations de référence](#appendix-b-profile-version-reference-information) sur les versions de profil. | Mises à jour pour la nouvelle version ; Commentaires des clients. |

## <a name="more-information"></a>Plus d’informations

- [Déployer la redirection de dossiers, les Fichiers hors connexion et les profils utilisateur itinérants](deploy-folder-redirection.md)
- [Déployer des ordinateurs principaux pour la redirection de dossiers et les profils utilisateur itinérants](deploy-primary-computers.md)
- [Implémentation de la gestion des États utilisateur](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc784645(v=ws.10)>)
- [Déclaration de support de Microsoft concernant les données de profil utilisateur répliquées](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
- [Applications chargement avec DISM](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
- [Résolution des problèmes d’empaquetage, de déploiement et d’interrogation d’applications basées sur des Windows Runtime](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)