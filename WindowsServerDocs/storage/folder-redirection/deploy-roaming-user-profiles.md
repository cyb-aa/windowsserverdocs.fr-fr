---
Title: Déploiement de profils utilisateur itinérants
TOCTitle: Deploying Roaming User Profiles
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.date: 07/09/2018
ms.author: jgerend
ms.openlocfilehash: c662b8c44e3603ec972e06f3fb0ddbd55e1af904
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192720"
---
# <a name="deploying-roaming-user-profiles"></a>Déploiement de profils utilisateur itinérants

>S’applique à : Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server (canal semi-annuel), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Cette rubrique explique comment utiliser Windows Server pour déployer [les profils utilisateur itinérants](folder-redirection-rup-overview.md) sur les ordinateurs clients Windows. Les profils utilisateur itinérants redirigent les profils utilisateurs vers un partage de fichiers afin que les utilisateurs reçoivent le même système d’exploitation et les paramètres de l’application sur plusieurs ordinateurs.

Pour obtenir la liste des modifications récentes apportées à ce sujet, consultez le [l’historique des modifications](#change-history) section de cette rubrique.

>[!IMPORTANT]
>En raison des modifications de sécurité apportées dans [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016), nous avons mis à jour [étape 4 : Si vous le souhaitez créer un objet de stratégie de groupe pour les profils utilisateur itinérants](#step-4-optionally-create-a-gpo-for-roaming-user-profiles) dans cette rubrique afin que Windows peuvent correctement appliquer la stratégie de profils utilisateur itinérants (et pas revenir à des stratégies locales sur les PC affectés).

> [!IMPORTANT]
>  Personnalisations de l’utilisateur au démarrage est perdue après une mise à niveau in situ de système d’exploitation dans la configuration suivante :
> - Les utilisateurs sont configurés pour un profil itinérant
> - Les utilisateurs sont autorisés à apporter des modifications à démarrer
>
> Par conséquent, le menu Démarrer est réinitialisé à la valeur par défaut de la nouvelle version du système d’exploitation une fois que le système d’exploitation à niveau sur place. Pour les solutions de contournement, consultez [annexe c : Travail réinitialiser environ mises en page du menu Démarrer après mise à niveau](#appendix-c-working-around-reset-start-menu-layouts-after-upgrades).

## <a name="prerequisites"></a>Prérequis

### <a name="hardware-requirements"></a>Configuration matérielle requise

Les profils utilisateur itinérants nécessitent un ordinateur x64 64 ou x86 ; Il n’est pas pris en charge par RT de Windows.

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
- Pour utiliser la prise en charge de l’ordinateur principal dans les profils utilisateur itinérants, il existe d’ordinateur client supplémentaires et des spécifications de schéma Active Directory. Pour plus d’informations, consultez [déployer des ordinateurs principaux pour la Redirection de dossiers et profils utilisateur itinérants](deploy-primary-computers.md).
- La disposition de menu ne seront pas itinérantes sur Windows 10, Windows Server 2019 ou Windows Server 2016 s’il utilise plusieurs PC hôte de Session Bureau à distance, serveur ou virtualisé Desktop Infrastructure (VDI) Guide de démarrage d’un utilisateur. Pour résoudre ce problème, vous pouvez spécifier une mise en page de démarrage, comme décrit dans cette rubrique. Ou vous pouvez utiliser des disques de profil utilisateur, qui suivent correctement les paramètres du menu Démarrer lorsqu’il est utilisé avec les serveurs hôte de Session Bureau à distance ou VDI. Pour plus d’informations, consultez [gestion des données utilisateur plus facile avec les disques de profil utilisateur dans Windows Server 2012](https://blogs.technet.microsoft.com/enterprisemobility/2012/11/13/easier-user-data-management-with-user-profile-disks-in-windows-server-2012/).

### <a name="considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows"></a>Points à prendre en considération lors de l'utilisation des profils utilisateur itinérants sur plusieurs versions de Windows

Si vous décidez d'utiliser les profils utilisateur itinérants sur plusieurs versions de Windows, nous vous recommandons d'effectuer les actions suivantes :

- Configurez Windows pour tenir à jour des versions de profil distinctes pour chaque version de système d'exploitation. Cela permet d'éviter des problèmes indésirables et inattendus tels que l'endommagement de profils.
- Utilisez la redirection de dossiers pour stocker des fichiers utilisateur tels que des documents et des images hors des profils utilisateur. Cela permet que les mêmes fichiers soient disponibles pour les utilisateurs sur différentes versions de système d'exploitation. Cela garantit également des profils de petite taille et des connexions rapides.
- Allouez suffisamment d'espace de stockage pour les profils utilisateur itinérants. Si vous prenez en charge deux versions de système d'exploitation, le nombre de profils double (et par conséquent l'espace total utilisé), car un profil distinct est tenu à jour pour chaque version de système d'exploitation.
- N’utilisez pas les profils utilisateur itinérants entre des ordinateurs exécutant Windows Vista/Windows Server 2008 et Windows 7/Windows Server 2008 R2. Itinérance entre ces versions de système d’exploitation n’est pas pris en charge en raison d’incompatibilités dans leurs versions de profil.
- Informez vos utilisateurs que les modifications apportées à une version de système d'exploitation ne seront pas itinérantes vers une autre version de système d'exploitation.
- Lors du déplacement de votre environnement vers une version de Windows qui utilise une version de profil différent (par exemple, de Windows 10 vers Windows 10, version 1607 : consultez [annexe b : Informations de référence de version de profil](#appendix-b-profile-version-reference-information) pour obtenir la liste), les utilisateurs reçoivent un profil utilisateur itinérant vide. Vous pouvez réduire l’impact de l’obtention d’un nouveau profil à l’aide de la Redirection de dossiers pour rediriger les dossiers communs. Il n’est pas une méthode prise en charge de migrer les profils utilisateur itinérants à partir de la version d’un seul profil à un autre.

## <a name="step-1-enable-the-use-of-separate-profile-versions"></a>Étape 1 : Activer l'utilisation de versions de profil distinctes

Si vous déployez les profils utilisateur itinérants sur les ordinateurs exécutant Windows 8.1, Windows 8, Windows Server 2012 R2 ou Windows Server 2012, nous vous recommandons d’apporter certaines modifications à votre environnement Windows avant le déploiement. Ces modifications permettent de garantir que les futures mises à niveau du système d'exploitation s'effectueront sans problème, et facilitent l'exécution simultanée de plusieurs versions de Windows avec des profils utilisateur itinérants.

Pour apporter ces modifications, procédez comme suit.

1. Téléchargez et installez les mises à jour logicielles appropriées sur tous les ordinateurs sur lesquels vous prévoyez d'utiliser des profils itinérants, obligatoires, super obligatoires ou de domaine par défaut :

    - Windows 8.1 ou Windows Server 2012 R2 : installer la mise à jour logicielle décrite dans l’article [2887595](http://support.microsoft.com/kb/2887595) dans la Base de connaissances Microsoft (lorsqu’il est publié).
    - Windows 8 ou Windows Server 2012 : installez la mise à jour logicielle décrite dans l’article [2887239](http://support.microsoft.com/kb/2887239) de la Base de connaissances Microsoft.
2. Sur tous les ordinateurs exécutant Windows 8.1, Windows 8, Windows Server 2012 R2 ou Windows Server 2012 sur lequel vous allez utiliser les profils utilisateur itinérants, utilisez l’Éditeur du Registre ou la valeur DWORD de clé de stratégie de groupe pour créer le Registre suivant et configurez-le pour `1`. Pour plus d’informations sur la création de clés de Registre à l’aide d’une stratégie de groupe, voir [Configurer un élément de Registre](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>).

    ```PowerShell
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\ProfSvc\Parameters\UseProfilePathExtensionVersion
    ```

    >[!WARNING]
    >Une modification incorrecte du Registre peut endommager gravement votre système. Avant toute modification du registre, il est conseillé de sauvegarder toutes les données importantes de votre ordinateur.
3. Redémarrez les ordinateurs.

## <a name="step-2-create-a-roaming-user-profiles-security-group"></a>Étape 2 : Créer des groupes de sécurité de profils utilisateur itinérants

Si votre environnement n'est pas déjà configuré avec les profils utilisateur itinérants, la première étape consiste à créer un groupe de sécurité qui contient tous les utilisateurs et/ou ordinateurs auxquels vous voulez appliquer les paramètres de stratégie des profils utilisateur itinérants.

- Les administrateurs de déploiements de profils utilisateur itinérants à usage général créent généralement un groupe de sécurité pour les utilisateurs.
- Les administrateurs de services Bureau à distance ou de déploiements de bureaux virtuels utilisent généralement un groupe de sécurité pour les utilisateurs et les ordinateurs partagés.

Voici comment créer un groupe de sécurité pour les profils utilisateur itinérants :

1. Ouvrez le gestionnaire Server sur un ordinateur avec le centre d’Administration Active Directory est installé.
2. Sur le **outils** menu, sélectionnez **centre d’Administration Active Directory**. Le Centre d’administration Active Directory s’affiche.
3. Cliquez sur le domaine approprié ou l’unité d’organisation, sélectionnez **New**, puis sélectionnez **groupe**.
4. Dans la fenêtre **Créer un groupe**, dans la section **Groupe**, indiquez les paramètres suivants :

    - Dans **Nom du groupe**, tapez le nom du groupe de sécurité, par exemple : **Profils utilisateur itinérants et ordinateurs**.
    - Dans **étendue du groupe**, sélectionnez **sécurité**, puis sélectionnez **Global**.
5. Dans le **membres** section, sélectionnez **ajouter**. La boîte de dialogue Sélectionnez Utilisateurs, Contacts, Ordinateurs ou Groupes s’affiche.
6. Si vous souhaitez inclure des comptes d’ordinateur dans le groupe de sécurité, sélectionnez **Types d’objets**, sélectionnez le **ordinateurs** case à cocher, puis sélectionnez **OK**.
7. Tapez les noms des utilisateurs, groupes et/ou ordinateurs auxquels vous souhaitez déployer les profils utilisateur itinérants, sélectionnez **OK**, puis sélectionnez **OK** à nouveau.

## <a name="step-3-create-a-file-share-for-roaming-user-profiles"></a>Étape 3 : Créer un partage de fichiers pour les profils utilisateur itinérants

Si vous n’avez pas déjà un partage de fichiers distincts pour l’itinérance profils utilisateur (indépendants de tous les partages des dossiers redirigés afin d’empêcher la mise en cache involontaire du dossier des profils itinérants), procédez comme suit pour créer un partage de fichiers sur un serveur exécutant Windows Serveur.

>[!NOTE]
>Certaines fonctionnalités peuvent différer ou ne pas être disponibles selon la version de Windows Server que vous utilisez.

Voici comment créer un partage de fichiers sur Windows Server :

1. Dans le volet de navigation de gestionnaire de serveur, sélectionnez **File and Storage Services**, puis sélectionnez **partages** pour afficher la page partages.
2. Dans la vignette partages, sélectionnez **tâches**, puis sélectionnez **nouveau partage**. L'Assistant Nouveau partage s'affiche.
3. Sur le **sélectionner un profil** , sélectionnez **partage SMB – rapide**. Si vous avez File Server Resource Manager est installé et que vous utilisez les propriétés de gestion de dossier, sélectionnez à la place **partage SMB - avancé**.
4. Dans la page **Emplacement du partage** , sélectionnez le serveur et le volume sur lesquels vous voulez créer le partage.
5. Dans la page **Nom du partage**, tapez un nom pour le partage (par exemple, **Profil utilisateur$** ) dans la zone **Nom du partage**.

    >[!TIP]
    >Lors de la création du partage, masquez-le en plaçant un caractère ```$``` après le nom du partage. Cela masque le partage dans les navigateurs informels.
6. Dans la page **Autres paramètres**, décochez la case **Activer la disponibilité continue**, si celle-ci est présente. Vous pouvez cocher les cases **Activer l’énumération basée sur l’accès** et **Chiffrer l’accès aux données**.
7. Sur le **autorisations** page, sélectionnez **personnaliser les autorisations...** . La boîte de dialogue Paramètres de sécurité avancés s'affiche.
8. Sélectionnez **désactiver l’héritage**, puis sélectionnez **convertir les autorisations héritées en autorisations explicites sur cet objet**.
9. Définir les autorisations comme décrit dans [des autorisations requises pour le fichier de partagent qui héberge les profils utilisateur itinérants](#required-permissions-for-the-file-share-hosting-roaming-user-profiles) et affichées dans la capture d’écran suivante, supprimez des autorisations pour les groupes et comptes et en ajoutant spéciaux autorisations pour le groupe de profils utilisateur itinérants et ordinateurs que vous avez créé à l’étape 1.
    
    ![Fenêtre Paramètres de sécurité montrant les autorisations comme décrit dans le tableau 1 avancée](media\advanced-security-user-profiles.jpg)
    
    **Figure 1** Définition des autorisations pour le partage de profils utilisateur itinérants
10. Si vous choisissez le profil **Partage SMB - Avancé** , dans la page **Propriétés de gestion** , sélectionnez la valeur Utilisation du dossier **Fichiers utilisateur** .
11. Si vous choisissez le profil **Partage SMB - Avancé** , dans la page **Quota** , vous pouvez sélectionner un quota à appliquer aux utilisateurs du partage.
12. Sur le **Confirmation** page, sélectionnez **créer.**

### <a name="required-permissions-for-the-file-share-hosting-roaming-user-profiles"></a>Autorisations requises pour les fichier partage hébergement les profils utilisateur itinérants

|       |       |       |
|   -   |   -   |   -   |
| Compte d’utilisateur | Accès | S'applique à |
|   System    |  Contrôle total     |  Ce dossier, ses sous-dossiers et ses fichiers     |
|  Administrateurs     |  Contrôle total     |  Ce dossier uniquement     |
|  Propriétaire créateur     |  Contrôle total     |  Sous-dossiers et fichiers uniquement     |
| Groupe de sécurité des utilisateurs qui doivent placer des données sur le partage (Profils utilisateur itinérants et ordinateurs)      |  Liste du dossier / lecture de données *(les autorisations avancées)* <br />Créer des dossiers / Ajout de données *(les autorisations avancées)* |  Ce dossier uniquement     |
| Autres groupes et comptes   |  Aucun (supprimer)     |       |

## <a name="step-4-optionally-create-a-gpo-for-roaming-user-profiles"></a>Étape 4 : Créer éventuellement un objet de stratégie de groupe pour les profils utilisateur itinérants

Si un objet de stratégie de groupe n'est pas déjà créé pour les paramètres des profils utilisateur itinérants, utilisez la procédure suivante pour en créer un vide avec les profils utilisateur itinérants. Cet objet de stratégie de groupe vous permet de configurer des paramètres des profils utilisateur itinérants (tels que la prise en charge des ordinateurs principaux, qui est traitée séparément), et peut également être utilisé pour activer les profils utilisateur itinérants sur des ordinateurs, comme c'est généralement le cas lors d'un déploiement dans des environnements de bureaux virtuels ou avec les services Bureau à distance.

Voici comment créer un objet de stratégie de groupe pour les profils utilisateur itinérants :

1. Ouvrez le Gestionnaire de serveur sur un ordinateur sur lequel la Gestion des stratégies de groupe est installée.
2. À partir de la **outils** menu, sélectionnez **Group Policy Management**. La Gestion des stratégies de groupe s'affiche.
3. Cliquez sur le domaine ou l’unité d’organisation dans laquelle vous souhaitez configurer les profils utilisateur itinérants, puis sélectionnez **créer un objet GPO dans ce domaine et le lier ici**.
4. Dans le **nouvel objet GPO** boîte de dialogue, tapez un nom pour l’objet de stratégie de groupe (par exemple, **paramètres du profil utilisateur itinérant**), puis sélectionnez **OK**.
5. Cliquez avec le bouton droit sur l'objet de stratégie de groupe nouvellement créé, puis décochez la case **Lien activé**. Cela évite que l'objet de stratégie de groupe soit appliqué tant que vous n'avez pas terminé de le configurer.
6. Sélectionnez l'objet de stratégie de groupe. Dans le **filtrage de sécurité** section de la **étendue** onglet, sélectionnez **utilisateurs authentifiés**, puis sélectionnez **supprimer** pour empêcher l’objet de stratégie de groupe d’être appliquée à tout le monde.
7. Dans le **filtrage de sécurité** section, sélectionnez **ajouter**.
8. Dans le **sélectionner un utilisateur, ordinateur ou groupe** boîte de dialogue, tapez le nom de la sécurité du groupe que vous avez créé à l’étape 1 (par exemple, **profils utilisateur itinérants et ordinateurs**), puis sélectionnez **OK** .
9. Sélectionnez le **délégation** onglet, sélectionnez **ajouter**, type **utilisateurs authentifiés**, sélectionnez **OK**, puis sélectionnez **OK** à nouveau pour accepter la valeur par défaut des autorisations de lecture.
    
    Cette étape est nécessaire en raison des modifications de sécurité apportées dans [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016).

>[!IMPORTANT]
>En raison des modifications de sécurité apportées dans [MS16-072A](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016), vous devez maintenant donner les autorisations de lecture de délégué de groupe utilisateurs authentifiés à l’objet de stratégie de groupe : sinon l’objet de stratégie de groupe ne sont pas appliqué aux utilisateurs, ou si elle est déjà appliquée, l’objet de stratégie de groupe est supprimé, redirection de profils utilisateur vers l’ordinateur local. Pour plus d’informations, consultez [déploiement de groupe de stratégie de sécurité mise à jour MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/).

## <a name="step-5-optionally-set-up-roaming-user-profiles-on-user-accounts"></a>Étape 5 : Configurer éventuellement les profils utilisateur itinérants sur des comptes d'utilisateur

Si vous déployez les profils utilisateur itinérants sur des comptes d'utilisateur, procédez comme suit pour spécifier les profils utilisateur itinérants pour des comptes d'utilisateur dans les services de domaine Active Directory. Si vous déployez les profils utilisateur itinérants sur des ordinateurs, comme c’est généralement pour les Services Bureau à distance ou les déploiements de bureaux virtuels, au lieu de cela, utilisez la procédure décrite dans [étape 6 : Configurer éventuellement les profils utilisateur itinérants sur les ordinateurs](#step-6-optionally-set-up-roaming-user-profiles-on-computers).

>[!NOTE]
>Si vous configurez les profils utilisateur itinérants sur des comptes d'utilisateur à l'aide d'Active Directory et sur des ordinateurs à l'aide d'une stratégie de groupe, le paramètre de stratégie basé sur l'ordinateur est prioritaire.

Voici comment configurer les profils utilisateur itinérants sur les comptes d’utilisateur :

1. Dans le Centre d'administration Active Directory, accédez au conteneur (ou à l'unité d'organisation) **Utilisateurs** dans le domaine approprié.
2. Sélectionnez tous les utilisateurs auxquels vous souhaitez affecter un profil utilisateur itinérant, cliquez sur les utilisateurs, puis sélectionnez **propriétés**.
3. Dans le **profil** section, sélectionnez le **chemin du profil :** case à cocher, puis entrez le chemin d’accès au partage de fichiers où vous souhaitez stocker le profil utilisateur itinérant de l’utilisateur, suivi par `%username%` (c'est-à-dire automatiquement remplacé par l’utilisateur nom la première fois que l’utilisateur se connecte). Exemple :
    
    `\\fs1.corp.contoso.com\User Profiles$\%username%`
    
    Pour spécifier un profil utilisateur itinérant obligatoire, spécifiez le chemin d’accès au fichier NTuser.man précédemment créé, par exemple, `fs1.corp.contoso.comUser Profiles$default`. Pour plus d’informations, consultez [créer des profils d’utilisateur obligatoire](https://docs.microsoft.com/windows/client-management/mandatory-user-profile).
4. Sélectionnez **OK**.

>[!NOTE]
>Par défaut, le déploiement de toutes les applications Windows® Runtime (du Windows Store) est autorisé lors de l'utilisation des profils utilisateur itinérants. Toutefois, lorsque vous utilisez un profil spécial, les applications ne sont pas déployées par défaut. Les profils spéciaux sont des profils utilisateur où les modifications sont supprimées lorsque l'utilisateur se déconnecte :
><br><br>Pour supprimer des restrictions sur le déploiement d’applications pour des profils spéciaux, activez le paramètre de stratégie **Allow deployment operations in special profiles** (situé dans Configuration de l’ordinateur\Stratégies\Modèles d’administration\Composants Windows\Déploiement de packages d’application). Toutefois, les applications déployées de ce scénario laisseront stockées sur l'ordinateur certaines données qui risquent de s'accumuler si, par exemple, il y a des centaines d'utilisateurs sur un seul et même ordinateur. Pour nettoyer les applications, recherchez ou développez un outil qui utilise le [CleanupPackageForUserAsync](https://msdn.microsoft.com/library/windows/apps/windows.management.deployment.packagemanager.cleanuppackageforuserasync.aspx) API pour nettoyer les packages d’application pour les utilisateurs qui n’ont plus un profil sur l’ordinateur.
><br><br>Pour obtenir des informations générales supplémentaires sur les applications du Windows Store, voir [Gestion de l’accès client au Windows Store](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh832040(v=ws.11)>).

## <a name="step-6-optionally-set-up-roaming-user-profiles-on-computers"></a>Étape 6 : Configurer éventuellement les profils utilisateur itinérants sur des ordinateurs

Si vous déployez les profils utilisateur itinérants sur des ordinateurs, comme c'est généralement le cas pour les services Bureau à distance ou les déploiements de bureaux virtuels, utilisez la procédure suivante. Si vous déployez les profils utilisateur itinérants aux comptes d’utilisateurs, à la place utiliser la procédure décrite dans [étape 5 : Configurer éventuellement les profils utilisateur itinérants sur les comptes d’utilisateur](#step-5-optionally-set-up-roaming-user-profiles-on-user-accounts).

Vous pouvez utiliser la stratégie de groupe pour appliquer les profils utilisateur itinérants aux ordinateurs exécutant Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008.

>[!NOTE]
>Si vous configurez les profils utilisateur itinérants sur des ordinateurs à l'aide d'une stratégie de groupe et sur des comptes d'utilisateur à l'aide d'Active Directory, le paramètre de stratégie basé sur l'ordinateur est prioritaire.

Voici comment configurer les profils utilisateur itinérants sur des ordinateurs :

1. Ouvrez le Gestionnaire de serveur sur un ordinateur sur lequel la Gestion des stratégies de groupe est installée.
2. À partir de la **outils** menu, sélectionnez **Group Policy Management**. Gestion des stratégies de groupe s’affiche.
3. Dans la gestion de stratégie de groupe, cliquez sur le GPO que vous avez créé à l’étape 3 (par exemple, **paramètres du profil utilisateur itinérant**), puis sélectionnez **modifier**.
4. Dans la fenêtre Éditeur de gestion des stratégies de groupe, accédez à **Configuration ordinateur**, puis à **Stratégies**, **Modèles d'administration**, **Système** et enfin **Profils utilisateur**.
5. Avec le bouton droit **définir le chemin d’accès de profil pour tous les utilisateurs ouvrant une session sur cet ordinateur itinérant** , puis sélectionnez **modifier**.
    > [!TIP]
    > S'il est configuré, le dossier de base d'un utilisateur est le dossier par défaut utilisé par certains programmes tels que Windows PowerShell. Vous pouvez configurer un autre emplacement local ou réseau par utilisateur à l'aide de la section **Dossier de base** des propriétés de compte d'utilisateur dans AD DS. Pour configurer l’emplacement de dossier de base pour tous les utilisateurs d’un ordinateur exécutant Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 dans un environnement de bureau virtuel, activez la **dossier de base utilisateur ensemble**  paramètre de stratégie, puis spécifiez la lettre de lecteur et de partage de fichier à mapper (ou spécifiez un dossier local). N'utilisez pas de variables d'environnement ou de points de suspension. L'alias de l'utilisateur est ajouté à la fin du chemin d'accès spécifié pendant que l'utilisateur se connecte.
6. Dans le **propriétés** boîte de dialogue, sélectionnez **activé**
7. Dans le **les utilisateurs ouvrant une session sur cet ordinateur doivent utiliser ce chemin d’accès de profil itinérant** , entrez le chemin d’accès au partage de fichiers où vous souhaitez stocker le profil utilisateur itinérant de l’utilisateur, suivi par `%username%` (qui est automatiquement remplacé avec l’utilisateur nom la première fois l’utilisateur se connecte). Exemple :

    `\\fs1.corp.contoso.com\User Profiles$\%username%`

    Pour spécifier un profil utilisateur itinérant obligatoire, qui est un profil préconfiguré auquel les utilisateurs ne peuvent pas apporter des modifications permanentes (les modifications apportées sont réinitialisées lorsque l’utilisateur se déconnecte), spécifiez le chemin d’accès au fichier NTuser.man précédemment créé, par exemple, `\\fs1.corp.contoso.com\User Profiles$\default`. Pour plus d’informations, voir [Création d’un profil utilisateur obligatoire](https://docs.microsoft.com/windows/client-management/mandatory-user-profile).
8. Sélectionnez **OK**.

## <a name="step-7-optionally-specify-a-start-layout-for-windows-10-pcs"></a>Étape 7 : Si vous le souhaitez spécifier une mise en page de démarrage pour les PC Windows 10

Vous pouvez utiliser la stratégie de groupe pour appliquer une disposition de menu de démarrage spécifique afin que les utilisateurs voient la même mise en page de démarrage sur tous les PC. Si les utilisateurs se connectent à plusieurs PC et que vous voulez qu’ils ont une disposition de début cohérente sur PC, assurez-vous que l’objet de stratégie de groupe s’applique à tous leurs PC.

Pour spécifier une mise en page de démarrage, procédez comme suit :

1. Mettre à jour de votre PC Windows 10 vers Windows 10 version 1607 (également connu sous la mise à jour anniversaire) ou une version ultérieure et installer la mise à jour cumulative le 14 mars 2017 ([KB4013429](https://support.microsoft.com/kb/4013429)) ou une version ultérieure.
2. Créer un fichier complet ou partiel Start menu Disposition XML. Pour ce faire, consultez [personnaliser et exportation mise en page de démarrage](https://docs.microsoft.com/windows/configuration/customize-and-export-start-layout).
    * Si vous spécifiez un *complète* mise en page de démarrage, un utilisateur ne peut pas personnaliser n’importe quelle partie du menu Démarrer. Si vous spécifiez un *partielle* mise en page de démarrage, les utilisateurs peuvent personnaliser uniquement les groupes verrouillés de vignettes que vous spécifiez. Toutefois, avec une disposition partielle de début, les personnalisations de l’utilisateur dans le menu Démarrer ne seront pas itinérantes à d’autres ordinateurs.
3. Utilisez la stratégie de groupe pour appliquer la mise en page de démarrage personnalisée à l’objet GPO que vous avez créé pour les profils utilisateur itinérants. Pour ce faire, consultez [utilisez stratégie de groupe pour appliquer une mise en page de démarrage personnalisée dans un domaine](https://docs.microsoft.com/windows/configuration/customize-windows-10-start-screens-by-using-group-policy#bkmk-domaingpodeployment).
4. Utilisez la stratégie de groupe pour définir la valeur de Registre suivante sur votre PC Windows 10. Pour ce faire, consultez [configurer un élément de Registre](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>).

| **Action** | **Mise à jour** |
|------------|------------|
|Hive|**HKEY_LOCAL_MACHINE**|
|Chemin de clé|**Software\Microsoft\Windows\CurrentVersion\Explorer**|
|Nom de valeur|**SpecialRoamingOverrideAllowed**|
|Type de valeur|**REG_DWORD**|
|Données de la valeur|**1** (ou **0** pour désactiver)|
|Base|**Décimal**|

5. (Facultatif) Activer les optimisations de première ouverture de session rendre la connexion plus rapide pour les utilisateurs. Pour ce faire, consultez [appliquer des stratégies pour améliorer les temps de connexion](https://docs.microsoft.com/windows/client-management/mandatory-user-profile#apply-policies-to-improve-sign-in-time).
6. (Facultatif) Réduisent davantage de temps de connexion en supprimant les applications inutiles de l’image de base Windows 10 que vous utilisez pour déployer les PC clients. 2019 de serveur Windows et Windows Server 2016 est inutile d’applications préconfigurées, donc vous pouvez ignorer cette étape sur les images de serveur.
    - Pour supprimer des applications, utilisez la [Remove-AppxProvisionedPackage](https://docs.microsoft.com/powershell/module/dism/remove-appxprovisionedpackage?view=win10-ps) applet de commande PowerShell de Windows pour désinstaller les applications suivantes. Si vos PC sont déjà déployées. vous pouvez utiliser un script la suppression de ces applications à l’aide de la [Remove-AppxPackage](https://docs.microsoft.com/powershell/module/appx/remove-appxpackage?view=win10-ps).
    
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
>Désinstallation de ces applications diminue les temps de connexion, mais vous pouvez les laisser installés si un d’eux a besoin de votre déploiement.

## <a name="step-8-enable-the-roaming-user-profiles-gpo"></a>Étape 8 : Activer les objets de stratégie de groupe des profils utilisateur itinérants

Si vous configurez les profils utilisateur itinérants sur des ordinateurs à l'aide d'une stratégie de groupe, ou si vous personnalisez d'autres paramètres de profil utilisateur itinérant à l'aide d'une stratégie de groupe, l'étape suivante consiste à activer l'objet de stratégie de groupe, en autorisant son application aux utilisateurs affectés.

>[!TIP]
>Si vous envisagez d’implémenter la prise en charge de l’ordinateur principal, faites-le maintenant, avant d’activer l’objet de stratégie de groupe. Cela évite que des données utilisateur soient copiées sur des ordinateurs non principaux avant que la prise en charge des ordinateurs principaux soit activée. Pour les paramètres de stratégie spécifiques, consultez [déployer des ordinateurs principaux pour la Redirection de dossiers et profils utilisateur itinérants](deploy-primary-computers.md).

Voici comment activer le GPO de profil utilisateur itinérant :

1. Ouvrez Gestion des stratégies de groupe.
2. Avec le bouton droit de l’objet de stratégie de groupe que vous avez créé, puis sélectionnez **lien activé**. Une coche s'affiche en regard de l'élément de menu.

## <a name="step-9-test-roaming-user-profiles"></a>Étape 9 : Tester les profils utilisateur itinérants

Pour tester les profils utilisateur itinérants, connectez-vous à un ordinateur avec un compte d'utilisateur configuré pour les profils utilisateur itinérants, ou connectez-vous à un ordinateur configuré pour les profils utilisateur itinérants. Vérifiez ensuite que le profil est redirigé.

Voici comment tester les profils utilisateur itinérants :

1. Connectez-vous à un ordinateur principal (si vous avez activé la prise en charge des ordinateurs principaux) avec un compte d'utilisateur pour lequel vous avez activé les profils utilisateur itinérants. Si vous avez activé les profils utilisateur itinérants sur des ordinateurs spécifiques, connectez-vous à l'un de ces ordinateurs.
2. Si l'utilisateur s'est précédemment connecté à l'ordinateur, ouvrez une invite de commandes avec élévation de privilèges, puis tapez la commande suivante pour vérifier que les derniers paramètres de stratégie de groupe sont appliqués à l'ordinateur client :
    
    ```PowerShell
    GpUpdate /Force
    ```
3. Pour vérifier que le profil utilisateur est itinérant, ouvrez **le panneau de configuration**, sélectionnez **système et sécurité**, sélectionnez **système**, sélectionnez **paramètres système avancés** , sélectionnez **paramètres** dans les profils utilisateur section, puis recherchez **itinérance** dans le **Type** colonne.

## <a name="appendix-a-checklist-for-deploying-roaming-user-profiles"></a>Annexe A : Liste de vérification du déploiement des profils utilisateur itinérants

|État|Action|
|:---:|---|
|☐<br>☐<br>☐<br>☐<br>☐|1. Préparer le domaine<br>-Joindre des ordinateurs au domaine<br>-Activer l’utilisation de versions de profil distinctes<br>-Créer des comptes d’utilisateur<br>-(Facultatif) déployer la Redirection de dossiers|
|☐<br><br><br>|2. Créer un groupe de sécurité pour les profils utilisateur itinérants<br>-Nom du groupe :<br>-Membres :|
|☐<br><br>|3. Créer un partage de fichiers pour les profils utilisateur itinérants<br>-Nom de partage de fichiers :|
|☐<br><br>|4. Créer un objet de stratégie de groupe pour les profils utilisateur itinérants<br>-Nom de l’objet stratégie de groupe :|
|☐|5. Configurer les paramètres de stratégie des profils utilisateur itinérants|
|☐<br>☐<br>☐|6. Activer les profils utilisateur itinérants<br>-Activée dans AD DS sur des comptes d’utilisateur ?<br>-Activé dans la stratégie de groupe sur les comptes d’ordinateur ?<br>|
|☐|7. (Facultatif) Spécifier une disposition de début obligatoire pour les PC Windows 10|
|☐<br>☐<br><br>☐<br><br>☐|8. (Facultatif) Activer la prise en charge de l’ordinateur principal<br>-Désigner les ordinateurs principaux pour les utilisateurs<br>-Location de mappages utilisateurs et ordinateurs principaux :<br>-Prise en charge des ordinateurs principaux (facultatif) activer la Redirection de dossiers<br>-Basées sur ordinateur ou utilisateur ?<br>-(Facultatif) activer la prise en charge des ordinateurs principaux pour les profils utilisateur itinérants|
|☐|9. Activer les objets de stratégie de groupe des profils utilisateur itinérants|
|☐|10. Tester les profils utilisateur itinérants|

## <a name="appendix-b-profile-version-reference-information"></a>Annexe B : Informations de référence sur les versions de profil

Chaque profil possède une version de profil qui correspond à peu près à la version de Windows sur lequel le profil est utilisé. Par exemple, Windows 10, version 1703 et version 1607 utilisent tous deux le. Version du profil V6. Microsoft crée une nouvelle version de profil uniquement lorsque cela est nécessaire pour assurer la compatibilité, c'est-à-dire pourquoi ne pas toutes les versions de Windows inclut une nouvelle version de profil.

Le tableau suivant répertorie l'emplacement des profils utilisateur itinérants sur différentes versions de Windows.

|Version du système d'exploitation|Emplacement des profils utilisateur itinérants|
|---|---|
|Windows XP et Windows Server 2003|```\\<servername>\<fileshare>\<username>```|
|Windows Vista et Windows Server 2008|```\\<servername>\<fileshare>\<username>.V2```|
|Windows 7 et Windows Server 2008 R2|```\\<servername>\<fileshare>\<username>.V2```|
|Windows 8 et Windows Server 2012|```\\<servername>\<fileshare>\<username>.V3``` (une fois que la clé de Registre et de mise à jour logiciel sont appliqués)<br>```\\<servername>\<fileshare>\<username>.V2``` (avant le logiciel de clé de Registre et de mise à jour sont appliquées)|
|Windows 8.1 et Windows Server 2012 R2|```\\<servername>\<fileshare>\<username>.V4``` (une fois que la clé de Registre et de mise à jour logiciel sont appliqués)<br>```\\<servername>\<fileshare>\<username>.V2``` (avant le logiciel de clé de Registre et de mise à jour sont appliquées)|
|Windows 10|```\\<servername>\<fileshare>\<username>.V5```|
|Windows 10, version 1703 et version 1607|```\\<servername>\<fileshare>\<username>.V6```|

## <a name="appendix-c-working-around-reset-start-menu-layouts-after-upgrades"></a>Annexe c : Travail réinitialiser environ mises en page du menu Démarrer après mise à niveau

Voici quelques méthodes permettant de contourner les dispositions de menu Démarrer réinitialisées après une mise à niveau sur place :

 - Si seul un utilisateur utilise jamais le périphérique et l’administrateur informatique utilise une stratégie de déploiement de système d’exploitation gérée tel que SCCM qu’ils peuvent les opérations suivantes :
    
    1. Exporter la disposition du menu Démarrer avec Export-Startlayout avant la mise à niveau 
    2. Importer la disposition du menu Démarrer avec Import-StartLayout après OOBE mais avant que l’utilisateur se connecte  
 
    > [!NOTE] 
    > L’importation d’un StartLayout modifie le profil utilisateur par défaut. Tous les profils utilisateur créés après que l’importation s’est produite obtiendra la disposition de début importée.
 
 - Administrateurs informatiques peuvent choisir de gérer la mise en page de démarrage avec la stratégie de groupe. La stratégie de groupe fournit une solution de gestion centralisée pour appliquer une mise en page Démarrer standardisé aux utilisateurs. Il existe 2 modes aux modes d’à l’aide de la stratégie de groupe pour la gestion de début. Verrouillage complet et partiel verrouillage. Le scénario complet de verrouillage empêche l’utilisateur d’apporter des modifications à la mise en page de démarrage. Le scénario de verrouillage partielle permet à utilisateur apporter des modifications à une zone spécifique du début. Pour plus d’informations, consultez [personnaliser et exportation mise en page de démarrage](https://docs.microsoft.com/windows/configuration/customize-and-export-start-layout).
        
    > [!NOTE]
    > Modification dans le scénario de verrouillage partielle toujours seront perdues pendant la mise à niveau.

 -  Laisser le début de réinitialisation de la disposition se produisent et permettre aux utilisateurs finaux reconfigurer le début. Un e-mail de notification ou autres notification peut être envoyée aux utilisateurs finaux d’attendre leurs mises en page de démarrage doivent être réinitialisées une fois que le système d’exploitation mise à niveau vers un impact réduit. 

# <a name="change-history"></a>Historique des modifications

Le tableau suivant récapitule certaines des modifications les plus importantes apportées à cette rubrique.

|Date|Description |Reason|
|--- |---         |---   |
|1er mai 2019|Ajout des mises à jour pour 2019|
|10 avril 2018|Ajout de discussion de lorsque les personnalisations utilisateur début sont perdues après une mise à niveau in situ de système d’exploitation|Problème connu de légende.|
|13 mars 2018 |Mise à jour pour Windows Server 2016 | Déplacé hors de la bibliothèque de Versions précédentes et mis à jour pour la version actuelle de Windows Server.|
|13 avril 2017|Ajouté des informations de profil pour Windows 10, version 1703 et de clarifier le fonctionnement de l’itinérance travail de versions de profil lors de la mise à niveau des systèmes d’exploitation, consultez [considérations sur l’utilisation de profils utilisateur itinérants sur plusieurs versions de Windows](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows).|Commentaires des clients.|
|14 mars 2017|Ajout de l’étape facultatif permettant de spécifier une disposition de début obligatoire pour les PC Windows 10 dans [annexe a : Liste de vérification pour le déploiement de profils utilisateur itinérants](#appendix-a-checklist-for-deploying-roaming-user-profiles).|Modifications des fonctionnalités dans la dernière mise à jour de Windows.|
|23 janvier 2017|Ajouté une étape pour [étape 4 : Si vous le souhaitez créer un objet de stratégie de groupe pour les profils utilisateur itinérants](#step-4-optionally-create-a-gpo-for-roaming-user-profiles) à déléguer des autorisations en lecture aux utilisateurs authentifiés, qui est désormais nécessaire en raison d’une mise à jour de sécurité de stratégie de groupe.|Modifications de la sécurité pour le traitement de la stratégie de groupe.|
|29 décembre 2016|Ajout d’un lien dans [étape 8 : Activer le GPO de profils utilisateur itinérants](#step-8-enable-the-roaming-user-profiles-gpo) pour faciliter l’obtention des informations sur la façon de définir la stratégie de groupe pour les ordinateurs principaux. Correction également quelques références aux incorrect des étapes 5 et 6 ayant les numéros.|Commentaires des clients.|
|Le 5 décembre 2016|Ajout d’informations sur les expliquant les paramètres du menu Démarrer une itinérance du problème.|Commentaires des clients.|
|6 juillet 2016|Ajouter des suffixes de versions de profil Windows 10 dans [annexe b : Informations de référence de version de profil](#appendix-b-profile-version-reference-information). Supprimer également Windows XP et Windows Server 2003 à partir de la liste des systèmes d’exploitation pris en charge.|Mises à jour pour les nouvelles versions de Windows et suppression d’informations sur les versions de Windows qui ne sont plus prises en charge.|
|7 juillet 2015|Ajout d’une exigence et d’une étape pour désactiver la disponibilité continue lors de l’utilisation d’un serveur de fichiers en cluster.|Les partages de fichiers en cluster offrent de meilleures performances pour les écritures de petite taille (dont l’utilisation est courante avec les profils utilisateurs itinérants) lorsque la disponibilité en continu est désactivée.|
|19 mars 2014|En majuscules des suffixes de versions de profil (. V2. V3. V4) dans [annexe b : Informations de référence de version de profil](#appendix-b-profile-version-reference-information).|Bien que Windows respecte la casse, si vous utilisez NFS avec le partage de fichiers, il est important de disposer de la casse (majuscule) correcte pour le suffixe du profil.|
|9 octobre 2013|Révisé pour Windows Server 2012 R2 et Windows 8.1, clarifier les choses et ajouté le [considérations sur l’utilisation de profils utilisateur itinérants sur plusieurs versions de Windows](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows) et [annexe b : Informations de référence de version de profil](#appendix-b-profile-version-reference-information) sections.|Mises à jour pour la nouvelle version ; commentaires des clients.|

## <a name="more-information"></a>Informations supplémentaires

- [Déployer la Redirection de dossiers, fichiers hors connexion et les profils utilisateur itinérants](deploy-folder-redirection.md)
- [Déployer des ordinateurs principaux pour la Redirection de dossiers et les profils utilisateur itinérants](deploy-primary-computers.md)
- [Implémentation de la gestion de l’état utilisateur](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc784645(v=ws.10)>)
- [Instruction de prise en charge de Microsoft autour des données de profil utilisateur répliquées](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
- [Supprimer des applications avec DISM](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
- [Résolution des problèmes d’empaquetage, déploiement et interrogation des applications basées sur le Windows Runtime](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)