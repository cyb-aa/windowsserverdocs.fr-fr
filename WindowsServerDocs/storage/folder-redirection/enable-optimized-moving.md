---
title: Activer les déplacements optimisés de dossiers redirigés
description: Comment effectuer un déplacement optimisé de dossiers redirigés vers un nouveau partage de fichiers.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: edf6596f7daaa2f496b8b4da36e98ee72b05dfcd
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867259"
---
# <a name="enable-optimized-moves-of-redirected-folders"></a>Activer les déplacements optimisés de dossiers redirigés

>S’applique à : Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server (canal semi-annuel)

Cette rubrique explique comment effectuer un déplacement optimisé de dossiers redirigés (redirection de dossiers) vers un nouveau partage de fichiers. Si vous activez ce paramètre de stratégie, lorsqu’un administrateur déplace le partage de fichiers hébergeant les dossiers redirigés et met à jour le chemin d’accès cible des dossiers redirigés dans stratégie de groupe, le contenu mis en cache est simplement renommé dans le cache de Fichiers hors connexion local sans aucun retards ou risque de perte de données pour l’utilisateur.

Auparavant, les administrateurs pouvaient modifier le chemin d’accès cible des dossiers redirigés dans stratégie de groupe et laisser les ordinateurs clients copier les fichiers à la prochaine connexion de l’utilisateur affecté, provoquant ainsi une connexion retardée. Les administrateurs peuvent également déplacer le partage de fichiers et mettre à jour le chemin cible des dossiers redirigés dans stratégie de groupe. Toutefois, toutes les modifications apportées localement sur les ordinateurs clients entre le début du déplacement et la première synchronisation après le déplacement seraient perdues.

## <a name="prerequisites"></a>Prérequis

Le déplacement optimisé présente les exigences suivantes :

- La redirection de dossiers doit être configurée. Pour plus d’informations, consultez [déployer la redirection de dossiers avec fichiers hors connexion](deploy-folder-redirection.md).
- Les ordinateurs clients doivent exécuter Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server (canal semi-annuel).

## <a name="step-1-enable-optimized-move-in-group-policy"></a>Étape 1 : Activer le déplacement optimisé stratégie de groupe

Pour optimiser le réadressage des données de redirection de dossiers, utilisez stratégie de groupe pour activer le paramètre de stratégie **activer le déplacement du contenu optimisé dans fichiers hors connexion cache sur le serveur redirection de dossiers** pour l’objet de stratégie de groupe (GPO) approprié. Si vous configurez ce paramètre de stratégie sur **désactivé** ou **non configuré** , le client copie tout le contenu de redirection de dossiers vers le nouvel emplacement, puis supprime le contenu de l’ancien emplacement si le chemin d’accès au serveur est modifié.

Voici comment activer le déplacement optimisé de dossiers redirigés :

1. Dans stratégie de groupe gestion, cliquez avec le bouton droit sur l’objet de stratégie de groupe que vous avez créé pour les paramètres de redirection de dossiers (par exemple, la **redirection de dossiers et les paramètres des profils utilisateur itinérants**), puis sélectionnez **modifier**.
2. Sous **Configuration utilisateur**, accédez à **stratégies**, **modèles d’administration**, puis **système**, puis **redirection de dossiers**.
3. Cliquez avec le bouton droit sur **activer le déplacement du contenu optimisé dans fichiers hors connexion cache sur le chemin d’accès du serveur de redirection de dossiers**, puis sélectionnez **modifier**.
4. Sélectionnez **activé**, puis cliquez sur **OK**.

## <a name="step-2-relocate-the-file-share-for-redirected-folders"></a>Étape 2 : Déplacer le partage de fichiers pour les dossiers redirigés

Lors du déplacement du partage de fichiers qui contient les dossiers redirigés des utilisateurs, il est nécessaire de prendre des précautions pour s’assurer que les dossiers sont correctement déplacés.

>[!IMPORTANT]
>Si les fichiers des utilisateurs sont en cours d’utilisation ou si l’état complet du fichier n’est pas conservé lors du déplacement, les utilisateurs peuvent rencontrer des problèmes de performances, car les fichiers sont copiés sur le réseau, les conflits de synchronisation générés par Fichiers hors connexion, voire la perte de données.

1. Informez les utilisateurs à l’avance que le serveur hébergeant leurs dossiers redirigés changera et vous conseillera d’effectuer les actions suivantes :

      - Synchronisez le contenu du cache de Fichiers hors connexion et résolvez les éventuels conflits.
      - Ouvrez une invite de commandes avec élévation de privilèges, entrez **gpupdate/target : User/force**, puis déconnectez-vous et reconnectez-vous pour vérifier que les derniers paramètres de stratégie de groupe sont appliqués à l’ordinateur client.

        >[!NOTE]
        >Par défaut, les ordinateurs clients mettent à jour les stratégie de groupe toutes les 90 minutes. par conséquent, si vous prévoyez suffisamment de temps pour que les ordinateurs clients reçoivent la stratégie mise à jour, vous n’avez pas besoin de demander aux utilisateurs d’utiliser **gpupdate**.
2. Supprimez le partage de fichiers du serveur pour vous assurer qu’aucun fichier du partage de fichiers n’est en cours d’utilisation. Pour ce faire, dans Gestionnaire de serveur, dans la page **partages** des services de fichiers et de stockage, cliquez avec le bouton droit sur le partage de fichiers approprié, puis sélectionnez **supprimer**.

    Les utilisateurs travaillent hors connexion à l’aide de Fichiers hors connexion jusqu’à ce que le déplacement soit terminé et qu’ils reçoivent les paramètres de redirection de dossiers mis à jour à partir de stratégie de groupe.

3. À l’aide d’un compte avec des privilèges de sauvegarde, déplacez le contenu du partage de fichiers vers le nouvel emplacement à l’aide d’une méthode qui conserve les horodateurs de fichier, tels qu’un utilitaire de sauvegarde et de restauration. Pour utiliser la commande **Robocopy** , ouvrez une invite de commandes avec élévation de privilèges, puis tapez la commande suivante ```<Source>``` , où est l’emplacement actuel du partage de fichiers ```<Destination>``` et est le nouvel emplacement :

    ```PowerShell
    Robocopy /B <Source> <Destination> /Copyall /MIR /EFSRAW
    ```

    >[!NOTE]
    >La commande **xcopy** ne conserve pas l’intégralité de l’état du fichier.
4. Modifiez les paramètres de stratégie de redirection de dossiers, en mettant à jour l’emplacement du dossier cible pour chaque dossier redirigé que vous souhaitez déplacer. Pour plus d’informations, consultez l’étape 4 de [déployer la redirection de dossiers avec fichiers hors connexion](deploy-folder-redirection.md).
5. Informez les utilisateurs que le serveur hébergeant leurs dossiers redirigés a changé, et qu’ils doivent utiliser la commande **gpupdate/target : User/force** , puis se déconnecter et se reconnecter pour récupérer la configuration mise à jour et reprendre la synchronisation de fichiers appropriée.

    Les utilisateurs doivent se connecter à tous les ordinateurs au moins une fois pour s’assurer que les données sont correctement déplacées dans chaque cache de Fichiers hors connexion.

## <a name="more-information"></a>Plus d’informations

* [Déployer la redirection de dossiers avec Fichiers hors connexion](deploy-folder-redirection.md)
* [Déployer des profils utilisateur itinérants](deploy-roaming-user-profiles.md)
* [Vue d’ensemble de la redirection de dossiers, des Fichiers hors connexion et des profils utilisateur itinérants](folder-redirection-rup-overview.md)