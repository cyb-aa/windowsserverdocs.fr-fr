---
title: Déployer la Redirection de dossiers avec des fichiers hors connexion
description: Découvrez comment utiliser Windows Server pour déployer la Redirection de dossiers avec des fichiers hors connexion sur les ordinateurs clients Windows.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 33942db34314e0ff60b24d4b9c8e5e33b4ca92fd
ms.sourcegitcommit: 5549ac178f8f3d116e88761a95223063a636ac94
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2018
ms.locfileid: "4376621"
---
# Déployer la Redirection de dossiers avec des fichiers hors connexion

>S’applique à: Windows 10, Windows 7, Windows 8, Windows 8.1, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, Windows Vista

Cette rubrique décrit comment utiliser Windows Server pour déployer la Redirection de dossiers avec des fichiers hors connexion sur les ordinateurs clients Windows.

Pour obtenir la liste des modifications récentes à ce sujet, voir [l’historique des modifications](#change-history).

>[!IMPORTANT]
>En raison des modifications de sécurité effectuées dans [MS16-072](https://support.microsoft.com/en-us/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), nous avons mis à jour [étape 3: créer un objet de stratégie de groupe pour la Redirection de dossiers](#step-3:-create-a-gpo-for-folder-redirection) de cette rubrique donc que Windows peut appliquer correctement la stratégie de Redirection de dossiers (et pas restaurer des dossiers redirigés sur les ordinateurs concernés).

## Conditions préalables

### Configuration matérielle requise

La Redirection de dossiers nécessite un ordinateur x64 ou x86; Il n’est pas pris en charge par le modèle de RT Windows®.

### Configuration logicielle requise

La Redirection de dossiers présente la configuration logicielle suivante:

- Pour administrer la Redirection de dossiers, vous devez être connecté tant que membre du groupe de sécurité administrateurs de domaine, le groupe de sécurité administrateurs de l’entreprise ou du groupe de sécurité propriétaires de créateurs de stratégie de groupe.
- Les ordinateurs clients doivent exécuter Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008.
- Les ordinateurs clients doivent être joints à l’Active Directory Domain Services (AD DS) que vous gérez.
- Un ordinateur doit être disponible avec la gestion des stratégies de groupe et le centre d’Administration Active Directory sont installés.
- Un serveur de fichiers doit être disponible pour héberger les dossiers redirigés.
    - Si le partage de fichiers utilise des espaces de noms DFS, les dossiers DFS (liens) doivent avoir une seule cible pour empêcher les utilisateurs d’apporter des modifications conflictuelles sur des serveurs différents.
    - Si le partage de fichiers utilise la réplication DFS pour répliquer le contenu avec un autre serveur, les utilisateurs doivent être en mesure d’accéder uniquement le serveur source pour empêcher les utilisateurs d’apporter des modifications conflictuelles sur des serveurs différents.
    - Lorsque vous utilisez un partage de fichiers en cluster, désactivez une disponibilité continue sur le partage de fichiers pour éviter les problèmes de performances avec la Redirection de dossiers et fichiers hors connexion. En outre, les fichiers hors connexion ne peut pas passer en mode hors connexion pendant 3 à 6 minutes après qu’un utilisateur perd l’accès à un partage de fichiers disponibles en permanence, ce qui peut frustrer les utilisateurs qui ne sont pas encore à l’aide du mode toujours en mode hors connexion des fichiers hors connexion.

>[!NOTE]
>Certaines des fonctionnalités plus récentes de la Redirection de dossiers ont ordinateur client supplémentaires et des spécifications de schéma Active Directory. Pour plus d’informations, voir [déployer des ordinateurs principales](deploy-primary-computers.md), [Désactiver les fichiers hors connexion sur les dossiers](disable-offline-files-on-folders.md), [Activer toujours mode hors connexion](enable-always-offline.md)et [permettent le déplacement du dossier optimisée](enable-optimized-moving.md).

## Étape 1: Créer un groupe de sécurité de la redirection de dossiers

Si votre environnement n’est pas déjà configuré avec la Redirection de dossiers, la première étape consiste à créer un groupe de sécurité qui contient tous les utilisateurs auxquels vous souhaitez appliquer les paramètres de stratégie de Redirection de dossiers.

Voici comment créer un groupe de sécurité pour la Redirection de dossiers:

1. Ouvrez le Gestionnaire de serveur sur un ordinateur avec le centre d’Administration Active Directory est installé.
2. Dans le menu **Outils** , sélectionnez le **Centre d’Administration Active Directory**. Le Centre d’administration Active Directory s’affiche.
3. Avec le bouton droit de l’unité d’organisation ou le domaine approprié, sélectionnez **Nouveau**et puis sélectionnez le **groupe**.
4. Dans la fenêtre **Créer un groupe**, dans la section **Groupe**, indiquez les paramètres suivants:
    - Dans la zone **nom du groupe**, tapez le nom du groupe de sécurité, par exemple: **Les utilisateurs de la Redirection de dossiers**.
    - Dans l' **étendue du groupe**, sélectionnez **la sécurité**et sélectionnez **Global**.
5. Dans la section **membres** , sélectionnez **Ajouter**. La boîte de dialogue Sélectionnez Utilisateurs, Contacts, Ordinateurs ou Groupes s’affiche.
6. Tapez les noms des utilisateurs ou des groupes auxquels vous souhaitez déployer la Redirection de dossiers, sélectionnez **OK**et sélectionnez **OK** à nouveau.

## Étape 2: Créer un partage de fichiers pour les dossiers redirigés

Si vous ne disposez pas déjà d’un partage de fichiers pour les dossiers redirigés, utilisez la procédure suivante pour créer un partage de fichiers sur un serveur exécutant Windows Server 2012.

>[!NOTE]
>Certaines fonctionnalités peuvent différer ou ne pas être disponible si vous créez le partage de fichiers sur un serveur qui exécute une autre version de Windows Server.

Voici comment créer un partage de fichiers sur Windows Server 2012 et Windows Server 2016:

1. Dans le volet de navigation le Gestionnaire de serveur, sélectionnez le **fichier et les Services de stockage**et sélectionnez **partages** pour afficher la page de partages.
2. Dans la vignette de **partages** , sélectionnez les **tâches**, puis sélectionnez **Nouveau partage**. L’Assistant Nouveau partage s’affiche.
3. Sur la page **Sélectionner un profil** , sélectionnez le **Partage SMB – rapide**. Si vous avez File Server Resource Manager est installé et que vous utilisez les propriétés de gestion de dossier, sélectionnez à la place **Partage SMB - avancée**.
4. Sur la page **Partagent un emplacement** , sélectionnez le serveur et le volume sur lequel vous souhaitez créer le partage.
5. Sur la page du **Nom de partage** , tapez un nom pour le partage (par exemple, **les utilisateurs$**) dans la zone **nom du partage** .
    >[!TIP]
    >Lors de la création du partage, masquez-le en plaçant un ```$``` après le nom de partage. Cela masque le partage dans le voisinage.
6. Sur la page **d’Autres paramètres** , décochez la case de disponibilité permanente activer, le cas échéant et si vous le souhaitez sélectionnez les cases à cocher **Activer l’énumération basée sur l’accès** et **chiffrer l’accès aux données** .
7. Sur la page **autorisations** , sélectionnez **Personnaliser les autorisations …**. La boîte de dialogue Paramètres de sécurité avancée s’affiche.
8. Sélectionnez **désactiver l’héritage**, puis de **convertir des autorisations dans l’autorisation explicite sur cet objet héritées**.
9. Définir des autorisations en tant que décrits le tableau 1 et illustré Figure 1, suppression des autorisations pour les comptes et les groupes non répertoriés et en ajoutant des autorisations spéciales pour le groupe d’utilisateurs de la Redirection de dossiers que vous avez créé à l’étape 1.
    
    ![Définition des autorisations pour le partage de dossiers redirigés](media/deploy-folder-redirection/setting-the-permissions-for-the-redirected-folders-share.png)
    
    **Figure 1** Définition des autorisations pour le partage de dossiers redirigés
10. Si vous avez choisi le profil de **Partage SMB - avancée** , sur la page de **Propriétés de gestion** , sélectionnez la valeur de l’utilisation de dossiers de **Fichiers de l’utilisateur** .
11. Si vous avez choisi le profil **SMB partager - avancée** , sur la page de **Quota** , sélectionnez éventuellement un quota à appliquer aux utilisateurs du partage.
12. Sur la page de **Confirmation** , sélectionnez **créer.**

### Partagent des autorisations requises pour le fichier qui héberge les dossiers redirigés

<table>
<tbody>
<tr class="odd">
<td>Compte d’utilisateur</td>
<td>Access</td>
<td>S'applique à</td>
</tr>
<tr class="even">
<td>Système</td>
<td>Contrôle total</td>
<td>Ce dossier, les sous-dossiers et les fichiers</td>
</tr>
<tr class="odd">
<td>Administrateurs</td>
<td>Contrôle total</td>
<td>Ce dossier uniquement</td>
</tr>
<tr class="even">
<td>Créateur/propriétaire</td>
<td>Contrôle total</td>
<td>Sous-dossiers et fichiers uniquement</td>
</tr>
<tr class="odd">
<td>Groupe de sécurité des utilisateurs qui ont besoin de placer des données sur le partage (utilisateurs de la Redirection de dossiers)</td>
<td>Liste du dossier / lire les données<sup>1</sup><br />
<br />
Créez des dossiers / append données<sup>1</sup><br />
<br />
Lire les attributs<sup>1</sup><br />
<br />
Lire les attributs étendus<sup>1</sup><br />
<br />
Autorisations de lecture<sup>1</sup></td>
<td>Ce dossier uniquement</td>
</tr>
<tr class="even">
<td>Autres groupes et comptes</td>
<td>Aucun (supprimer)</td>
<td></td>
</tr>
</tbody>
</table>

Autorisations avancées 1

## Étape 3: Créer un objet de stratégie de groupe pour la Redirection de dossiers

Si vous ne disposez pas déjà d’un objet GPO créé pour les paramètres de la Redirection de dossiers, utilisez la procédure suivante pour en créer un.

Voici comment créer un objet de stratégie de groupe pour la Redirection de dossiers:

1. Ouvrez le Gestionnaire de serveur sur un ordinateur avec la gestion des stratégies de groupe est installé.
2. Dans le menu **Outils** , sélectionnez la **Gestion des stratégies de groupe**.
3. Cliquez sur le domaine ou l’unité d’organisation dans laquelle vous souhaitez configurer la Redirection de dossiers, puis sélectionnez **créer un objet GPO dans ce domaine et le lier ici**.
4. Dans la boîte de dialogue **Nouvel objet GPO** , tapez un nom pour l’objet de stratégie de groupe (par exemple, **Les paramètres de la Redirection de dossiers**) et sélectionnez **OK**.
5. Avec le bouton droit de la stratégie de groupe qui vient d’être créé, puis désactivez la case à cocher **Lien activé** . Cela empêche l’objet de stratégie de groupe n’est pas appliqué jusqu'à ce que vous avez terminé la configuration de ce dernier.
6. Sélectionnez l’objet de stratégie de groupe. Dans la section **Filtrage de sécurité** de l’onglet **étendue** , sélectionnez **Utilisateurs authentifiés**, puis **supprimez** pour empêcher l’objet de stratégie de groupe d’être appliquée à tout le monde.
7. Dans la section **Filtrage de sécurité** , sélectionnez **Ajouter**.
8. Dans la boîte de dialogue **Sélectionner un utilisateur, les ordinateurs ou les groupes** , tapez le nom du groupe de sécurité que vous avez créé à l’étape 1 (par exemple, **Les utilisateurs de la Redirection de dossiers**) et sélectionnez **OK**.
9. Sélectionnez l’onglet **délégation** , sélectionnez **Ajouter**, tapez **Utilisateurs authentifiés**, sélectionnez **OK**et sélectionnez **OK** à nouveau pour accepter les autorisations de lecture par défaut.
    
    Cette étape est nécessaire en raison des modifications apportées dans [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016)la sécurité.

>[!IMPORTANT]
>En raison de la sécurité, les modifications apportées dans [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), vous pouvez désormais doivent donner le groupe utilisateurs authentifiés délégué des autorisations de lecture à l’objet GPO la Redirection de dossiers: dans le cas contraire l’objet de stratégie de groupe ne sont pas appliqué aux utilisateurs, ou si elle est déjà appliquée, l’objet de stratégie de groupe est supprimée, en redirigeant dossiers vers le PC local. Pour plus d’informations, voir le [déploiement de groupe de la stratégie de sécurité mise à jour MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/).

## Étape 4: Configurer la redirection de dossiers avec les fichiers hors connexion

Après avoir créé un objet de stratégie de groupe pour les paramètres de la Redirection de dossiers, modifiez les paramètres de stratégie de groupe pour activer et configurer la Redirection de dossiers, comme indiqué dans la procédure suivante.

>[!NOTE]
>Fichiers hors connexion est activée par défaut pour les dossiers redirigés sur les ordinateurs clients Windows et désactivé sur les ordinateurs exécutant Windows Server, à moins que modifiées par l’utilisateur. Pour utiliser la stratégie de groupe pour contrôler si les fichiers hors connexion est activé, utilisez le **Autoriser ou interdire l’utilisation de la fonctionnalité de fichiers hors connexion** paramètre de stratégie.
> Pour plus d’informations sur certaines des autres paramètres de stratégie de groupe de fichiers hors connexion, consultez [Activer avancée en mode hors connexion des fichiers fonctionnalité](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn270369(v%3dws.11)>)et [Configuration de la stratégie de groupe pour les fichiers hors connexion](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc759721(v%3dws.10)>).

Voici comment configurer la Redirection de dossiers dans la stratégie de groupe:

1. Dans Gestion des stratégies de groupe avec le bouton droit de l’objet GPO que vous avez créé (par exemple, **Les paramètres de la Redirection de dossiers**) et sélectionnez **Modifier**.
2. Dans la fenêtre Éditeur de gestion des stratégies de groupe, accédez à la **Configuration de l’utilisateur**, puis **stratégies**, puis **Les paramètres Windows**et **La Redirection de dossiers**.
3. Clic droit sur un dossier que vous voulez rediriger (par exemple, **les Documents**), puis sélectionnez **Propriétés**.
4. Dans la boîte de dialogue **Propriétés** , dans la zone de **paramètre** , sélectionnez **base - Rediriger les dossiers de tout le monde vers le même emplacement**.

    > [!NOTE]
    > Pour appliquer la Redirection de dossiers pour les ordinateurs clients exécutant Windows XP ou Windows Server 2003, sélectionnez l’onglet **paramètres** et sélectionnez le **s’appliquent également la stratégie de redirection à Windows 2000, Windows 2000 Server, Windows XP et d’exploitation Windows Server 2003 systèmes** case à cocher.
5. Dans la section **emplacement du dossier cible** , sélectionnez **créer un dossier pour chaque utilisateur sous le chemin d’accès racine** , puis, dans la zone de **Chemin d’accès racine** , tapez le chemin d’accès au partage de fichiers stockant des dossiers redirigés, par exemple: **\\\fs1.corp.contoso.com\\ les utilisateurs$**
6. Sélectionnez l’onglet **paramètres** , puis dans la section de la **Suppression d’une stratégie** , si vous le souhaitez **rediriger le dossier vers l’emplacement du profil utilisateur local lorsque la stratégie est supprimée** (ce paramètre peut aider à effectuer la Redirection de dossiers se comportent plus manière prévisible pour adminisitrators et les utilisateurs).
7. Sélectionnez **OK**et puis sélectionnez **«Oui»** dans la boîte de dialogue d’avertissement.

## Étape 5: Activer la Redirection de dossiers de stratégie de groupe

Une fois que vous avez terminé de configurer les paramètres de stratégie de groupe de Redirection de dossiers, l’étape suivante consiste à activer l’objet de stratégie de groupe, qui lui permettront de s’appliquer à des utilisateurs concernés.

>[!TIP]
>Si vous prévoyez de mettre en œuvre la prise en charge de l’ordinateur principal ou d’autres paramètres de stratégie, faites-le maintenant, avant d’activer l’objet de stratégie de groupe. Cela empêche les données utilisateur d’être copiés sur des ordinateurs non principal avant d’activer la prise en charge de l’ordinateur principal.

Voici comment faire pour activer le GPO de la Redirection de dossiers:

1. Gestion des stratégies de groupe Open.
2. Avec le bouton droit de la stratégie de groupe que vous avez créé et sélectionnez **Lien activé**. Une case à cocher s’affiche en regard de l’élément de menu.

## Étape 6: Tester la Redirection de dossiers

Pour tester la Redirection de dossiers, se connecter à un ordinateur avec un compte d’utilisateur configuré pour la Redirection de dossiers. Ensuite, vérifiez que les dossiers et les profils sont redirigées.

Voici comment procéder tester la Redirection de dossiers:

1. Se connecter à un ordinateur principal (si vous avez activé la prise en charge de l’ordinateur principal) avec un compte d’utilisateur pour lequel vous avez activé la Redirection de dossiers.
2. Si l’utilisateur a déjà connecté à l’ordinateur, ouvrez une invite de commandes avec élévation de privilèges et tapez la commande suivante pour vous assurer que les derniers paramètres de stratégie de groupe sont appliquées à l’ordinateur client:
    
    ```PowerShell
    gpupdate /force
    ```
3. Ouvrez l’Explorateur de fichiers.
4. Avec le bouton droit à un dossier redirigé (par exemple, le dossier Mes Documents dans la bibliothèque Documents), puis sélectionnez **Propriétés**.
5. Sélectionnez l’onglet **emplacement** et vérifiez que le chemin d’accès au partage de fichiers que vous avez spécifié au lieu d’un chemin d’accès local s’affiche.

## Annexe a: de liste de vérification pour le déploiement de la Redirection de dossiers

|le statut|Action|
|:---:|---|
|☐<br>☐<br>☐|1. préparer le domaine<br>-Joindre des ordinateurs au domaine<br>-Créer des comptes d’utilisateur|
|☐<br><br><br>|2. créer le groupe de sécurité pour la Redirection de dossiers<br>-Nom du groupe:<br>-Membres:|
|☐<br><br>|3. création d’un partage de fichiers pour les dossiers redirigés<br>-Nom du partage de fichiers:|
|☐<br><br>|4. création d’un objet de stratégie de groupe pour la Redirection de dossiers<br>-Nom de l’objet stratégie de groupe:|
|☐<br><br>☐<br>☐<br>☐<br>☐<br>☐|5. configurer les paramètres de stratégie de Redirection de dossiers et fichiers hors connexion<br>-Redirection de dossiers:<br>-Windows 2000, Windows XP et prise en charge de Windows Server 2003 activée?<br>-Fichiers hors connexion activé? (activé par défaut sur les ordinateurs clients Windows)<br>-Mode toujours en mode hors connexion activé?<br>-Synchronisation des fichiers en arrière-plan activée?<br>-Optimisé déplacement des dossiers redirigés activé?|
|☐<br><br>☐<br><br>☐<br>☐|6. (prise en charge de facultatif) activer ordinateur principal<br>--Basé sur l’ordinateur ou utilisateur?<br>-Désigner des ordinateurs principales pour les utilisateurs<br>-Emplacement de l’utilisateur et les mappages de l’ordinateur principal:<br>-Prise en charge d’ordinateur principal (facultatif) activer la Redirection de dossiers<br>-Prise en charge d’ordinateur principal activer (facultatif) pour les profils utilisateur itinérants|
|☐|7. activer la Redirection de dossiers de stratégie de groupe|
|☐|8. tester la Redirection de dossiers|

## Historique des modifications

Le tableau suivant récapitule quelques-unes des modifications plus importantes à cette rubrique.

|Date|Description|Raison|
|---|---|---|
|18 janvier 2017|Ajout d’une étape à [étape 3: créer un objet de stratégie de groupe pour la Redirection de dossiers](#step-3:-create-a-gpo-for-folder-redirection) pour déléguer des autorisations en lecture aux utilisateurs authentifiés, ce qui est désormais requis en raison d’une mise à jour de sécurité de la stratégie de groupe.|Commentaires des clients.|

## Informations supplémentaires

* [Redirection de dossiers, fichiers hors connexion et profils utilisateurs itinérants](folder-redirection-rup-overview.md)
* [Déployer des ordinateurs principales pour la Redirection de dossiers et les profils utilisateur itinérants](deploy-primary-computers.md)
* [Activer la fonctionnalité avancée des fichiers hors connexion](enable-always-offline.md)
* [Déclaration de prise en charge de Microsoft autour des données de profil utilisateur répliqué](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
* [Chargement indépendant d’applications avec DISM](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
* [Résolution des problèmes de création de package, le déploiement et la requête d’applications basées sur le Windows Runtime](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)