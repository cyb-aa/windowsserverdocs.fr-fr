---
title: Activer optimisé déplace des dossiers redirigés
description: Découvrez comment effectuer un déplacement optimisé des dossiers redirigés vers un nouveau partage de fichiers.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 98fd5d50645ad454204dcf9dabf58e97c246ab1a
ms.sourcegitcommit: 505505b7ec99506e7ff50eddbdd6aa94a602abe6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/11/2018
ms.locfileid: "3831379"
---
# Activer optimisé déplace des dossiers redirigés

>S’applique à: Windows 10, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

Cette rubrique décrit comment effectuer un déplacement optimisé des dossiers redirigés (la Redirection de dossiers) à un nouveau partage de fichiers. Si vous activez ce paramètre de stratégie, lorsqu’un administrateur déplace le partage de fichiers qui héberge des dossiers redirigés et met à jour le chemin d’accès cible des dossiers redirigés dans la stratégie de groupe, le contenu mis en cache est simplement renommé dans le cache local, fichiers hors connexion sans tout retard ou perte de données potentielle pour l’utilisateur.

Auparavant, les administrateurs peuvent modifier le chemin d’accès cible des dossiers redirigés dans la stratégie de groupe et de laisser que les ordinateurs clients copier les fichiers à prochaine ouverture de session l’utilisateur concerné, à l’origine d’une connexion différée. Par ailleurs, les administrateurs peuvent déplacer le partage de fichiers et mettre à jour le chemin d’accès cible des dossiers redirigés dans la stratégie de groupe. Toutefois, les modifications apportées localement sur les ordinateurs clients entre le début du déplacement et de la première synchronisation après le déplacement sont perdues.

## Conditions préalables

Déplacement optimisé présente les conditions suivantes:

- La Redirection de dossiers doit être le programme d’installation. Pour plus d’informations, voir [Déployer la Redirection de dossiers avec des fichiers hors connexion](deploy-folder-redirection.md).
- Les ordinateurs clients doivent exécuter Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.

## Étape 1: Activer optimisé déplacement dans la stratégie de groupe

Pour optimiser le déplacement de données de la Redirection de dossiers, utilisez la stratégie de groupe pour activer le paramètre de stratégie **Activer optimisé déplacement du contenu dans le cache de fichiers hors connexion sur les changements de chemin d’accès de serveur de la Redirection de dossiers** pour l’objet de stratégie de groupe (GPO) l’approprié. Configurer ce paramètre de stratégie sur **désactivé** ou **Non configuré** , le client copier tout le contenu de la Redirection de dossiers vers le nouvel emplacement et supprimez le contenu à partir de l’ancien emplacement si le chemin d’accès du serveur change.

Voici comment faire pour activer le déplacement optimisé des dossiers redirigés:

1. Dans Gestion des stratégies de groupe, cliquez sur l’objet de stratégie de groupe vous créé pour les paramètres de la Redirection de dossiers (par exemple, **la Redirection de dossiers et les paramètres de profils utilisateur itinérants**), puis **Modifier**.
2. Sous **Configuration utilisateur**, accédez à **La Redirection de dossiers**, **les stratégies**, puis **Modèles d’administration**, puis **système**.
3. Avec le bouton droit **Qu'activer optimisé déplacement du contenu dans le cache de fichiers hors connexion sur les changements de chemin d’accès de serveur de la Redirection de dossiers**et sélectionnez **Modifier**.
4. Sélectionnez **activé**et sélectionnez **OK**.

## Étape 2: Déplacez le partage de fichiers pour les dossiers redirigés

Lorsque le déplacement le partage de fichiers qui contient des utilisateurs des dossiers redirigés, il est important à prendre des précautions pour vous assurer que les dossiers sont déplacés correctement.

>[!IMPORTANT]
>Si les fichiers des utilisateurs sont utiliser ou si l’état de fichier complet n’est pas conservé dans le déplacement, les utilisateurs peuvent rencontrer des performances médiocres comme les fichiers sont copiés sur le réseau, les conflits de synchronisation générés par les fichiers hors connexion, ou même une perte de données.

1. Avertir les utilisateurs à l’avance que le serveur qui héberge leurs dossiers redirigés remplaceront et recommandons qu’ils effectuent les actions suivantes:

      - Synchroniser le contenu de leur cache de fichiers hors connexion et résoudre les conflits.
      - Ouvrez une invite de commandes avec élévation de privilèges, entrez **/force/force GpUpdate/target: User**, puis déconnectez-vous et reconnectez-vous pour vous assurer que les derniers paramètres de stratégie de groupe sont appliquées à l’ordinateur client

        >[!NOTE]
        >Par défaut, les ordinateurs clients mettre à jour la stratégie de groupe toutes les 90 minutes, si vous autorisez suffisamment de temps pour le client de réception de la stratégie de mise à jour pour les ordinateurs, vous n’avez pas besoin de demander aux utilisateurs d’utiliser **GpUpdate**.
2. Supprimer le partage de fichiers à partir du serveur pour vous assurer qu’aucun fichier dans le partage de fichiers n’est en cours d’utilisation. Pour ce faire dans le Gestionnaire de serveur, sur la page de **partages** de fichiers et de Services de stockage, cliquez sur le partage de fichiers approprié, puis sélectionnez **Supprimer**.

    Les utilisateurs fonctionne en mode hors connexion à l’aide de fichiers hors connexion jusqu'à ce que le déplacement est terminé et ils reçoivent les paramètres de la Redirection de dossiers mis à jour à partir de la stratégie de groupe.

3. À l’aide d’un compte avec des privilèges de sauvegarde, de déplacer le contenu du partage de fichier vers le nouvel emplacement à l’aide d’une méthode qui conserve les horodatages des fichiers, par exemple, une sauvegarde et de restauration de l’utilitaire. À utiliser la commande **Robocopy** , ouvrez une invite de commandes avec élévation de privilèges, puis tapez la commande suivante, où ```<Source>``` est l’emplacement actuel du partage de fichiers, et ```<Destination>``` est le nouvel emplacement:

    ```PowerShell
    Robocopy /B <Source> <Destination> /Copyall /MIR /EFSRAW
    ```

    >[!NOTE]
    >La commande **Xcopy** ne conserve pas l’ensemble de l’état du fichier.
4. Modifier les paramètres de stratégie de Redirection de dossiers, mise à jour de l’emplacement du dossier cible pour chaque dossier redirigé que vous voulez déplacer. Pour plus d’informations, voir l’étape 4 de [Déployer la Redirection de dossiers avec des fichiers hors connexion](deploy-folder-redirection.md).
5. Avertir les utilisateurs que le serveur qui héberge leurs dossiers redirigés a changé et qu’ils doivent utiliser la **commande/force GpUpdate/target: User** et ensuite se déconnecter et se reconnecte pour obtenir la configuration de mise à jour et de reprendre la synchronisation de fichier approprié.

    Les utilisateurs doivent s’à tous les ordinateurs au moins une fois pour vous assurer que les données obtient déplacées correctement dans chaque cache des fichiers hors connexion.

## Informations supplémentaires

* [Déployer la Redirection de dossiers avec des fichiers hors connexion](deploy-folder-redirection.md)
* [Déployer les profils utilisateur itinérants](deploy-roaming-user-profiles.md)
* [Vue d’ensemble de la Redirection de dossiers, fichiers hors connexion et les profils utilisateur itinérants](folder-redirection-rup-overview.md)