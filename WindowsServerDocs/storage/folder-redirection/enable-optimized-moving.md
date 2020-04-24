---
title: Activer les déplacements optimisés de dossiers redirigés
description: Découvrez comment effectuer un déplacement optimisé de dossiers redirigés vers un nouveau partage de fichiers.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: edf714bc0d6b39dbe7c5e800e953d7820fe9abc5
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "75352611"
---
# <a name="enable-optimized-moves-of-redirected-folders"></a>Activer les déplacements optimisés de dossiers redirigés

>S'applique à : Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server (canal semi-annuel)

Cette rubrique explique comment effectuer un déplacement optimisé de dossiers redirigés (redirection de dossiers) vers un nouveau partage de fichiers. Lorsque vous activez ce paramètre de stratégie et qu’un administrateur déplace le partage de fichiers hébergeant les dossiers redirigés et met à jour le chemin cible des dossiers redirigés dans la stratégie de groupe, le contenu mis en cache est simplement renommé dans le cache Fichiers hors connexion local sans aucun délai ni perte de données potentielle pour l’utilisateur.

Auparavant, les administrateurs pouvaient changer le chemin cible des dossiers redirigés dans la stratégie de groupe et laisser les ordinateurs clients copier les fichiers à la prochaine connexion de l’utilisateur affecté, provoquant ainsi une connexion retardée. Les administrateurs peuvent également déplacer le partage de fichiers et mettre à jour le chemin cible des dossiers redirigés dans la stratégie de groupe. Mais dans ce cas, toute modification apportée localement sur les ordinateurs clients entre le début du déplacement et la première synchronisation après le déplacement est perdue.

## <a name="prerequisites"></a>Prérequis

Voici les exigences relatives à un déplacement optimisé :

- La redirection de dossiers doit être configurée. Pour plus d’informations, consultez [Déployer la redirection de dossiers avec Fichiers hors connexion](deploy-folder-redirection.md).
- Les ordinateurs clients doivent exécuter Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server (canal semi-annuel).

## <a name="step-1-enable-optimized-move-in-group-policy"></a>Étape 1 : Activer le déplacement optimisé dans la stratégie de groupe

Pour optimiser le déplacement des données de redirection de dossiers, utilisez la stratégie de groupe pour activer le paramètre de stratégie **Autoriser le déplacement optimisé de contenu dans le cache des fichiers hors connexion lors de la modification du chemin d’accès au serveur de redirection de dossiers** de l’objet de stratégie de groupe approprié. Si vous choisissez **Désactivé** ou **Non configuré** pour ce paramètre de stratégie, le client copie tout le contenu de redirection de dossiers dans le nouvel emplacement, puis supprime le contenu de l’ancien emplacement si le chemin au serveur change.

Voici comment activer le déplacement optimisé de dossiers redirigés :

1. Dans Gestion des stratégies de groupe, cliquez avec le bouton droit sur l’objet de stratégie de groupe que vous avez créé pour les paramètres de redirection de dossiers (par exemple, **Paramètres de redirection de dossiers et de profils utilisateur itinérants**), puis sélectionnez **Modifier**.
2. Sous **Configuration utilisateur**, accédez à **Stratégies**, **Modèles d’administration**, **Système**, puis à **Redirection de dossiers**.
3. Cliquez avec le bouton droit sur **Autoriser le déplacement optimisé de contenu dans le cache des fichiers hors connexion lors de la modification du chemin d’accès au serveur de redirection de dossiers**, puis sélectionnez **Modifier**.
4. Sélectionnez **Activé**, puis **OK**.

## <a name="step-2-relocate-the-file-share-for-redirected-folders"></a>Étape 2 : Déplacer le partage de fichiers pour les dossiers redirigés

Quand vous déplacez le partage de fichiers contenant les dossiers redirigés des utilisateurs, il est important de prendre des précautions pour vous assurer que les dossiers sont correctement déplacés.

>[!IMPORTANT]
>Si les fichiers des utilisateurs sont en cours d’utilisation ou si l’état complet du fichier n’est pas conservé pendant le déplacement, les utilisateurs peuvent se heurter à des problèmes de performances lors de la copie des fichiers sur le réseau, faire face à des conflits de synchronisation générés par Fichiers hors connexion, voire subir une perte de données.

1. Avertissez les utilisateurs que le serveur hébergeant leurs dossiers redirigés va changer et recommandez-leur d’effectuer les actions suivantes :

      - Synchroniser le contenu du cache Fichiers hors connexion et résoudre les éventuels conflits.
      - Ouvrir une invite de commandes avec élévation de privilèges, entrer **GpUpdate /Target:User /Force**, puis se déconnecter et se reconnecter pour vérifier que les derniers paramètres de stratégie de groupe sont appliqués à l’ordinateur client.

        >[!NOTE]
        >Par défaut, les ordinateurs clients mettent à jour la stratégie de groupe toutes les 90 minutes. Donc, si vous laissez aux ordinateurs clients suffisamment de temps pour recevoir la stratégie mise à jour, il est inutile de demander aux utilisateurs d’utiliser **GpUpdate**.
2. Supprimez le partage de fichiers du serveur pour vous assurer qu’aucun fichier de ce partage n’est en cours d’utilisation. Pour effectuer cette opération dans le Gestionnaire de serveur, accédez à la page **Partages** de Services de fichiers et de stockage, cliquez avec le bouton droit sur le partage de fichiers approprié, puis sélectionnez **Supprimer**.

    Les utilisateurs travaillent sur les fichiers hors connexion jusqu’à ce que le déplacement soit terminé et qu’ils reçoivent les paramètres de redirection de dossiers mis à jour de la stratégie de groupe.

3. À l’aide d’un compte disposant de privilèges de sauvegarde, déplacez le contenu du partage de fichiers vers le nouvel emplacement selon une méthode qui conserve les horodateurs de fichier, comme un utilitaire de sauvegarde et de restauration. Pour utiliser la commande **Robocopy**, ouvrez une invite de commandes avec élévation de privilèges, puis tapez la commande suivante (où ```<Source>``` est l’emplacement actuel du partage de fichiers et ```<Destination>``` le nouvel emplacement) :

    ```PowerShell
    Robocopy /B <Source> <Destination> /Copyall /MIR /EFSRAW
    ```

    >[!NOTE]
    >La commande **Xcopy** ne conserve pas l’intégralité de l’état du fichier.
4. Modifiez les paramètres de stratégie de redirection de dossiers en mettant à jour l’emplacement du dossier cible pour chaque dossier redirigé à déplacer. Pour plus d’informations, consultez l’étape 4 de la procédure [Déployer la redirection de dossiers avec Fichiers hors connexion](deploy-folder-redirection.md).
5. Informez les utilisateurs que le serveur hébergeant leurs dossiers redirigés a changé et qu’ils doivent utiliser la commande **GpUpdate /Target:User /Force**, puis se déconnecter et se reconnecter pour obtenir la configuration mise à jour et reprendre la synchronisation des fichiers.

    Il est recommandé que les utilisateurs se connectent à tous les ordinateurs au moins une fois de manière à déplacer correctement les données dans chaque cache Fichiers hors connexion.

## <a name="more-information"></a>Autres informations

* [Déployer la redirection de dossiers avec Fichiers hors connexion](deploy-folder-redirection.md)
* [Déployer des profils utilisateur itinérants](deploy-roaming-user-profiles.md)
* [Vue d’ensemble de la redirection de dossiers, des fichiers hors connexion et des profils utilisateur itinérants](folder-redirection-rup-overview.md)