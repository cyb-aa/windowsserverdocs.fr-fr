---
title: Utilisez Robocopy pour preseed des fichiers pour la réplication DFS
description: Comment utiliser Robocopy.exe pour preseed des fichiers pour la réplication DFS.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: a7a14b1a1e0f91002b201869e4c68187ffaf3f8f
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082095"
---
# <a name="use-robocopy-to-preseed-files-for-dfs-replication"></a>Utilisez Robocopy pour preseed des fichiers pour la réplication DFS

>S’appliqueà: Windows Server2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2, WindowsServer2008

Cette rubrique explique comment utiliser l’outil de ligne de commande, **Robocopy.exe**, pour preseed des fichiers lorsque vous configurez la réplication pour la réplication de système de fichiers distribués (DFS) (également appelé DFSR ou DFS-R) dans Windows Server. Par preseeding les fichiers avant de configurer la réplication DFS, ajouter un nouveau partenaire de réplication, ou remplacer un serveur, vous pouvez accélérer la synchronisation initiale et activer un clonage de la base de données de réplication DFS dans Windows Server 2012 R2. La méthode Robocopy est une des méthodes preseeding plusieurs; pour une vue d’ensemble, voir [étape 1: preseed de fichiers pour la réplication DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495046(v%3dws.11)>).

L’utilitaire de ligne de commande Robocopy (copie de fichiers robuste) est inclus avec Windows Server. L’utilitaire fournit des options étendues qui incluent la copie sécurité, sauvegarde API prise en charge, les fonctionnalités de nouvelle tentative et la journalisation. Versions ultérieures incluent la prise en charge des e/s non mises en mémoire tampon et de multi-thread.

>[!IMPORTANT]
>Robocopy ne copie pas les fichiers verrouillés en mode exclusif. Si les utilisateurs ont tendance à verrouiller plusieurs fichiers pendant une longue période sur vos serveurs de fichiers, envisagez d’utiliser une autre méthode preseeding. Preseeding ne nécessite pas une parfaite correspondance entre les listes de fichiers sur les serveurs source et de destination, mais n’est plus de fichiers qui n’existent pas lors de la synchronisation initiale est effectuée pour la réplication DFS, la preseeding moins efficace. Pour réduire les conflits de verrouillage, utilisez Robocopy pendant les heures creuses pour votre organisation. Toujours examiner les journaux Robocopy après preseeding pour vous assurer que vous comprenez les fichiers qui ont été ignorés en raison des verrous exclusifs.

Pour utiliser Robocopy preseed des fichiers pour la réplication DFS, procédez comme suit:

1. [Téléchargez et installez la dernière version de Robocopy.](#step-1:-download-and-install-the-latest-version-of-robocopy)
2. [Stabilisation des fichiers qui seront répliqués.](#step-2:-stabilize-files-that-will-be-replicated)
3. [Copiez les fichiers répliqués sur le serveur de destination.](#step-3:-copy-the-replicated-files-to-the-destination-server)

## <a name="prerequisites"></a>Prérequis

Parce que preseeding n’implique pas directement la réplication DFS, il vous suffit de remplir les conditions pour effectuer une copie du fichier avec Robocopy.

- Vous devez disposer d’un compte qui est membre du groupe Administrateurs local sur les serveurs source et de destination.

- Installer la version la plus récente de Robocopy sur le serveur que vous utiliserez pour copier les fichiers, le serveur source ou le serveur de destination; Vous devez installer la version la plus récente pour la version du système d’exploitation. Pour plus d’informations, voir [étape 2: stabilisation des fichiers qui seront répliqués](#step-2:-stabilize-files-that-will-be-replicated). Sauf si vous sont preseeding des fichiers à partir d’un serveur exécutant Windows Server 2003 R2, vous pouvez exécuter Robocopy sur le serveur source ou de destination. Le serveur de destination, qui est généralement la plus récente version de système d’exploitation, vous donne accès à la version la plus récente de Robocopy.

- Assurez-vous que l’espace de stockage est disponible sur le lecteur de destination. Ne pas créer un dossier sur le chemin d’accès que vous souhaitez copier vers: Robocopy doit créer le dossier racine.
    
    >[!NOTE]
    >Lorsque vous déterminez la quantité d’espace à allouer pour les fichiers preseeded, prenez en compte la croissance des données attendue sur les exigences de stockage et de temps pour la réplication DFS. Pour l’aide de la planification, voir [Modifier la taille du Quota du dossier intermédiaire et conflit ainsi que le dossier supprimés](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754229(v=ws.11)) dans la [Gestion de la réplication DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754771(v=ws.11)>).

- Sur le serveur source, si vous le souhaitez installer Process Monitor ou Process Explorer, vous pouvez utiliser pour vérifier pour les applications qui sont des fichiers de verrouillage. Pour plus d’informations de téléchargement, voir [Process Monitor](https://docs.microsoft.com/sysinternals/downloads/procmon) et [Process Explorer](https://docs.microsoft.com/sysinternals/downloads/process-explorer).

## <a name="step-1-download-and-install-the-latest-version-of-robocopy"></a>Étape 1: Télécharger et installer la dernière version de Robocopy

Avant d’utiliser Robocopy pour preseed des fichiers, vous téléchargez et installez la dernière version de **Robocopy.exe**. Cela garantit que la réplication DFS n’ignore les fichiers en raison de problèmes dans les versions de Robocopy.

La source de la dernière version Robocopy compatible dépend de la version de Windows Server qui s’exécute sur le serveur. Pour plus d’informations sur le téléchargement du correctif avec la version la plus récente de Robocopy pour Windows Server 2008 R2 ou Windows Server 2008, voir [liste des correctifs actuellement disponibles pour les technologies de système de fichiers distribués (DFS) dans Windows Server 2008 et Windows Server 2008 R2](https://support.microsoft.com/help/968429/list-of-currently-available-hotfixes-for-distributed-file-system-dfs-t).

Vous pouvez également rechercher et installer le correctif le plus récent pour un système d’exploitation en procédant comme suit.

### <a name="locate-and-install-the-latest-version-of-robocopy-for-a-specific-version-of-windows-server"></a>Recherchez et installez la dernière version de Robocopy pour une version spécifique de Windows Server

1. Dans un navigateur web, ouvrez [https://support.microsoft.com](https://support.microsoft.com/).

2. **Prise en charge de la recherche**, entrez ce qui suit chaîne, en remplaçant `<operating system version>` avec le système d’exploitation approprié, puis appuyez sur la touche ENTRÉE:
    
    ```robocopy.exe kbqfe "<operating system version>"```
    
    Par exemple, entrez **robocopy.exe kbqfe «Windows Server 2008 R2»**.

3. Recherchez et téléchargez le correctif logiciel avec le numéro d’identification le plus élevé (autrement dit, la version la plus récente).

4. Installer le correctif logiciel sur le serveur.

## <a name="step-2-stabilize-files-that-will-be-replicated"></a>Étape 2: Stabilisation des fichiers qui seront répliquées

Après avoir installé la dernière version de Robocopy sur le serveur, vous devez empêcher les fichiers verrouillés de blocage de copie à l’aide des méthodes décrites dans le tableau suivant. La plupart des applications ne pas verrouiller exclusivement les fichiers. Toutefois, en fonctionnement normal, un petit pourcentage de fichiers peut être verrouillé sur les serveurs de fichiers.

|Source du verrou|Explication|Prévention|
|---|---|---|
|Les utilisateurs à distance ouvrir les fichiers sur les partages.|Les employés se connecter à un serveur de fichiers standard et modifier des documents, du contenu multimédia ou autres fichiers. Parfois appelé le dossier de base traditionnelle ou données partagées des charges de travail.|Opérations sont uniquement Robocopy pendant heures creuses, les heures creuses. Cela réduit le nombre de fichiers Robocopy doit ignorer pendant preseeding.<br><br>Envisagez de définir temporairement un accès en lecture seule sur les partages de fichiers à l’aide des applets de commande Windows PowerShell **Grant-SmbShareAccess** et **Fermer-SmbSession** seront répliqués. Si vous définissez des autorisations pour un groupe courants tels que tout le monde ou utilisateurs authentifiés lire, les utilisateurs standards peuvent être moins susceptibles d’ouvrir des fichiers avec les verrous exclusifs (si leurs applications détecter l’accès en lecture seule lorsque les fichiers sont ouverts).<br><br>Vous pouvez également envisager de définir une règle de pare-feu temporaire pour le port SMB 445 en entrée pour ce serveur pour bloquer l’accès aux fichiers ou utilisez l’applet de commande **Bloc-SmbShareAccess** . Toutefois, ces deux méthodes sont très gênants pour les opérations de l’utilisateur.|
|Applications ouvrent les fichiers locaux.|Verrouiller des charges de travail s’exécutant sur un serveur de fichiers parfois des fichiers.|Désactiver ou désinstaller les applications qui verrouillent les fichiers temporairement. Vous pouvez utiliser Process Monitor ou Process Explorer afin de déterminer quelles applications sont verrouillage des fichiers. Pour télécharger Process Monitor ou Process Explorer, consultez les pages [Process Monitor](https://docs.microsoft.com/sysinternals/downloads/procmon) et [Process Explorer](https://docs.microsoft.com/sysinternals/downloads/process-explorer) .|

## <a name="step-3-copy-the-replicated-files-to-the-destination-server"></a>Étape 3: Copier les fichiers répliqués sur le serveur de destination

Après la réduction des verrous sur les fichiers qui seront répliqués, vous pouvez preseed les fichiers du serveur source vers le serveur de destination.

>[!NOTE]
>Vous pouvez exécuter Robocopy sur l’ordinateur source ou de l’ordinateur de destination. La procédure suivante décrit Robocopy en cours d’exécution sur le serveur de destination qui exécute généralement un système d’exploitation plus récent, pour tirer parti des fonctionnalités Robocopy supplémentaires que le système d’exploitation plus récent peut fournir.

### <a name="preseed-the-replicated-files-onto-the-destination-server-with-robocopy"></a>Preseed les fichiers répliqués sur le serveur de destination avec Robocopy

1. Connectez-vous au serveur de destination avec un compte qui est membre du groupe Administrateurs local sur les serveurs source et de destination.

2. Ouvrez une invite de commandes avec élévation de privilèges.

3. Pour preseed les fichiers à partir de la source au serveur de destination, exécutez la commande suivante, en remplaçant votre propre source, destination et chemins d’accès du fichier journal pour les valeurs entre crochets:
    
    ```PowerShell
    robocopy "<source replicated folder path>" "<destination replicated folder path>" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:<log file path> /v
    ```
    
    Cette commande copie tout le contenu du dossier source vers le dossier de destination, avec les paramètres suivants:
    
    |Paramètre|Description|
    |---|---|
    |«\ < source chemin_accès\nom_fichier dossier répliqué >»|Spécifie le dossier source à preseed sur le serveur de destination.|
    |«\ < destination répliqués chemin_accès\nom_fichier dossier >»|Spécifie le chemin d’accès au dossier qui stockera les fichiers preseeded.<br><br>Le dossier de destination ne doit pas exister sur le serveur de destination. Pour obtenir les hachages de fichier correspondants, Robocopy doit créer le dossier racine lorsqu’il preseeds les fichiers.|
    |/e|Copie les sous-répertoires et leurs fichiers, ainsi que les sous-répertoires vides.|
    |/b|Copie les fichiers en mode de sauvegarde.|
    |/ copyal|Copie toutes les informations de fichier, y compris les données, les attributs, horodatages, la liste de contrôle d’accès (ACL) NTFS, les informations sur le propriétaire et informations d’audit.|
    |/r:6|Tentatives de l’opération de six lorsqu’une erreur se produit.|
    |/w:5|Attend 5 secondes entre les tentatives.|
    |MT:64|Copie les 64 fichiers simultanément.|
    |/XD DfsrPrivate|Exclut le dossier DfsrPrivate.|
    |/tee|Écrit la sortie de l’état dans la fenêtre de console, ainsi que dans le fichier journal.|
    |/ log \ < chemin du fichier journal >|Spécifie le fichier journal à écrire. Remplace le contenu du fichier existant. (Pour ajouter les entrées dans le fichier journal existant, utilisez `/log+ <log file path>`.)|
    |/v|Génère la sortie détaillée qui inclut les fichiers ignorés.|
    
    Par exemple, la commande suivante réplique les fichiers à partir du dossier source répliquées, E:\\RF01, sur le lecteur de données D sur le serveur de destination:
    
    ```PowerShell
    robocopy.exe "\\srv01\e$\rf01" "d:\rf01" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:c:\temp\preseedsrv02.log
    ```
    
    >[!NOTE]
    >Nous recommandons d’utiliser les paramètres décrits ci-dessus, lorsque vous utilisez Robocopy pour preseed des fichiers pour la réplication DFS. Toutefois, vous pouvez modifier certaines de leurs valeurs, ou ajouter des paramètres supplémentaires. Par exemple, vous trouverez par le biais de test que vous avez la possibilité de définir une valeur plus élevée (nombre de threads) pour le *paramètre/MT* . En outre, si vous dupliquerez principalement des fichiers de grande taille, vous pourrez peut-être augmenter les performances de la copie en ajoutant l’option **/j** régulièrement e/s. Pour plus d’informations sur les paramètres de Robocopy, voir la référence de ligne de commande [Robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy) .

    >[!WARNING]
    >Pour éviter de perdre des données lorsque vous utilisez Robocopy pour preseed des fichiers pour la réplication DFS, n’apportez les modifications suivantes aux paramètres recommandés:
    >- N’utilisez pas le paramètre */mir* (qui reflète une arborescence) ou */mov* (qui déplace les fichiers, puis les supprime de la source).
    >-  Ne supprimez pas les options **/e** **/b**et **/copyall** .

4. Une fois que la copie est terminée, examinez le journal des erreurs ou les fichiers ignorés. Utilisez Robocopy pour copier les fichiers ignorés individuellement au lieu de recopiant l’ensemble des fichiers. Si les fichiers ont été ignorés en raison des verrouillages, essayez de copier des fichiers avec Robocopy ou accepter que ces fichiers nécessitent la réplication sur la transmission par la réplication DFS lors de la synchronisation initiale.

## <a name="next-step"></a>Étape suivante

Après avoir terminé la copie initiale et que vous utilisez Robocopy pour résoudre les problèmes avec les fichiers ignorés autant que possible, vous utiliserez l’applet de commande **Get-DfsrFileHash** dans Windows PowerShell ou la commande **dfsrdiag /** pour valider les fichiers preseeded en comparant hachages de fichier sur les serveurs source et de destination. Pour des instructions détaillées, consultez la rubrique [étape 2: valider les fichiers Preseeded pour la réplication DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495042(v%3dws.11)>).