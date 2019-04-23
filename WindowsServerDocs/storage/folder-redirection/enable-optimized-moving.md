---
title: Activer optimisé déplace des dossiers redirigés
description: Comment effectuer un déplacement optimisé des dossiers redirigés vers un nouveau partage de fichier.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 98fd5d50645ad454204dcf9dabf58e97c246ab1a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853990"
---
# <a name="enable-optimized-moves-of-redirected-folders"></a>Activer optimisé déplace des dossiers redirigés

>S’applique à : Windows 10, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

Cette rubrique décrit comment effectuer un déplacement optimisé des dossiers redirigés (Redirection de dossiers) à un partage de fichiers. Si vous activez ce paramètre de stratégie, lorsqu’un administrateur déplace le partage de fichiers hébergeant les dossiers redirigés et met à jour le chemin d’accès cible des dossiers redirigés dans Stratégie de groupe, le contenu mis en cache a simplement été renommé dans le cache de fichiers hors connexion local sans les retards ou perte de données pour l’utilisateur.

Auparavant, les administrateurs peuvent modifier le chemin d’accès cible des dossiers redirigés dans Stratégie de groupe et de laisser que les ordinateurs clients copier les fichiers à la prochaine connexion de l’utilisateur concerné, à l’origine d’une connexion retardée. Vous pouvez également les administrateurs peuvent déplacer le partage de fichiers et mettre à jour le chemin d’accès cible des dossiers redirigés dans Stratégie de groupe. Toutefois, toutes les modifications apportées localement sur les ordinateurs clients entre le début du déplacement et de la première synchronisation après le déplacement seraient perdues.

## <a name="prerequisites"></a>Prérequis

Déplacement optimisé présente les exigences suivantes :

- La Redirection de dossiers doit être configuré. Pour plus d’informations, consultez [déployer la Redirection de dossiers avec les fichiers hors connexion](deploy-folder-redirection.md).
- Ordinateurs clients doivent exécuter Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.

## <a name="step-1-enable-optimized-move-in-group-policy"></a>Étape 1 : Activer le déplacement optimisé dans Stratégie de groupe

Pour optimiser le déplacement de données de la Redirection de dossiers, utilisez la stratégie de groupe pour activer la **activer optimisé déplacer du contenu dans le cache de fichiers hors connexion sur les changements de chemin d’accès de la Redirection de dossiers serveur** paramètre de stratégie pour la stratégie de groupe approprié Objet (GPO). Configuration de ce paramètre de stratégie à **désactivé** ou **pas configuré** oblige le client à copier tout le contenu de la Redirection de dossiers vers le nouvel emplacement et supprimez le contenu de l’ancien emplacement si le modifications de chemin d’accès de serveur.

Voici comment activer le déplacement optimisé des dossiers redirigés :

1. Dans Gestion de stratégie de groupe, cliquez sur le GPO que vous avez créé pour les paramètres de la Redirection de dossiers (par exemple, **Redirection de dossiers et les paramètres du profil utilisateur itinérant**), puis sélectionnez **modifier**.
2. Sous **Configuration utilisateur**, accédez à **stratégies**, puis **modèles d’administration**, puis **système**, puis  **La Redirection de dossiers**.
3. Avec le bouton droit **activer optimisé déplacer du contenu dans le cache de fichiers hors connexion sur les changements de chemin d’accès de la Redirection de dossiers serveur**, puis sélectionnez **modifier**.
4. Sélectionnez **activé**, puis sélectionnez **OK**.

## <a name="step-2-relocate-the-file-share-for-redirected-folders"></a>Étape 2 : Déplacer le partage de fichiers des dossiers redirigés

Lorsque le déplacement de partage de fichiers qui contient les utilisateurs des dossiers redirigés, il est important de prendre certaines précautions pour garantir que les dossiers sont correctement déplacés.

>[!IMPORTANT]
>Si les fichiers des utilisateurs sont en utiliser, ou si l’état de fichier complet n’est pas conservé dans le déplacement, les utilisateurs peuvent rencontrer des performances médiocres que les fichiers sont copiés sur le réseau, les conflits de synchronisation générés par les fichiers hors connexion, ou même une perte de données.

1. Prévenez les utilisateurs que le serveur qui héberge leurs dossiers redirigés modifie et recommandons d’effectuer des actions suivantes :

      - Synchroniser le contenu de leur cache de fichiers hors connexion et résolvez les conflits.
      - Ouvrez une invite de commandes avec élévation de privilèges, entrez **les GpUpdate /Target:User /Force**et ensuite, déconnectez-vous et reconnectez-vous pour vous assurer que les derniers paramètres de stratégie de groupe sont appliquées à l’ordinateur client

        >[!NOTE]
        >Par défaut, les ordinateurs clients mettre à jour la stratégie de groupe toutes les 90 minutes, donc si vous autorisez un délai suffisant pour client ordinateurs de recevoir la stratégie mise à jour, vous n’avez pas besoin de demander aux utilisateurs d’utiliser **GpUpdate**.
2. Supprimer le partage de fichiers à partir du serveur pour vous assurer qu’aucun fichier dans le partage de fichiers n’est en cours d’utilisation. Pour ce faire dans le Gestionnaire de serveur, sur le **partages** page Services de fichiers et stockage, cliquez sur le partage de fichiers approprié, puis sélectionnez **supprimer**.

    Les utilisateurs travailleront hors connexion à l’aide de fichiers hors connexion jusqu'à ce que le déplacement est terminé et qu’ils reçoivent les paramètres de la Redirection de dossiers mis à jour à partir de la stratégie de groupe.

3. À l’aide d’un compte avec des privilèges de sauvegarde, de déplacer le contenu du partage de fichiers vers le nouvel emplacement à l’aide d’une méthode qui conserve les horodatages des fichiers, notamment une sauvegarde et de restauration d’utilitaire. Pour utiliser le **Robocopy** commande, ouvrez une invite de commandes avec élévation de privilèges, puis tapez la commande suivante, où ```<Source>``` est l’emplacement actuel du partage de fichiers, et ```<Destination>``` correspond au nouvel emplacement :

    ```PowerShell
    Robocopy /B <Source> <Destination> /Copyall /MIR /EFSRAW
    ```

    >[!NOTE]
    >Le **Xcopy** commande ne conserve pas l’ensemble de l’état du fichier.
4. Modifier les paramètres de stratégie de Redirection de dossiers, la mise à jour de l’emplacement du dossier cible pour chaque dossier redirigé que vous souhaitez déplacer. Pour plus d’informations, voir l’étape 4 de [déployer la Redirection de dossiers avec les fichiers hors connexion](deploy-folder-redirection.md).
5. Informer les utilisateurs que le serveur qui héberge leurs dossiers redirigés a changé et qu’ils doivent utiliser le **les GpUpdate /Target:User /Force** commande et ensuite, déconnectez-vous et reconnectez-vous pour obtenir la configuration mise à jour et de reprendre le fichier approprié synchronisation.

    Les utilisateurs doivent se connecter à tous les ordinateurs au moins une fois pour vous assurer que les données obtient correctement déplacées dans chaque cache de fichiers hors connexion.

## <a name="more-information"></a>Informations supplémentaires

* [Déployer la Redirection de dossiers des fichiers hors connexion](deploy-folder-redirection.md)
* [Déployer les profils utilisateur itinérants](deploy-roaming-user-profiles.md)
* [Vue d’ensemble de la Redirection de dossiers, fichiers hors connexion et les profils utilisateur itinérants](folder-redirection-rup-overview.md)