---
title: Utilisez Robocopy pour prédéfini de fichiers pour la réplication DFS
description: Comment utiliser Robocopy.exe à prédéfini de fichiers pour la réplication DFS.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: a7a14b1a1e0f91002b201869e4c68187ffaf3f8f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865080"
---
# <a name="use-robocopy-to-preseed-files-for-dfs-replication"></a>Utilisez Robocopy pour prédéfini de fichiers pour la réplication DFS

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Cette rubrique explique comment utiliser l’outil de ligne de commande, **Robocopy.exe**pour prédéfini de fichiers lorsque vous configurez la réplication pour la réplication de système de fichiers distribués (DFS, Distributed File System) (également appelé DFSR ou DFS-R) dans Windows Server. Par un preseed des fichiers avant de configurer la réplication DFS, ajouter un nouveau partenaire de réplication, ou remplacer un serveur, vous pouvez accélérer la synchronisation initiale et activer le clonage de la base de données de la réplication DFS dans Windows Server 2012 R2. La méthode de Robocopy est une des méthodes preseeding ; pour une vue d’ensemble, consultez [étape 1 : prédéfini de fichiers pour la réplication DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495046(v%3dws.11)>).

L’utilitaire de ligne de commande Robocopy (copie de fichier robuste) est inclus avec Windows Server. L’utilitaire fournit les nombreuses options qui incluent une copie sécurité, sauvegarde API prise en charge, les fonctionnalités de nouvelle tentative et la journalisation. Versions ultérieures incluent la prise en charge multi-threading et non mis en mémoire tampon des e/s.

>[!IMPORTANT]
>Robocopy ne copie pas exclusivement les fichiers verrouillés. Si les utilisateurs ont tendance à verrouiller un grand nombre de fichiers pendant de longues périodes sur vos serveurs de fichiers, envisagez d’utiliser une autre méthode preseeding. Un preseed ne nécessite pas une correspondance parfaite entre les listes de fichiers sur les serveurs source et de destination, mais est de plus de fichiers qui n’existent pas lors de la synchronisation initiale est effectuée pour la réplication DFS, le preseed moins efficaces. Pour réduire les conflits de verrou, utilisez Robocopy pendant les heures creuses pour votre organisation. Examinez toujours les journaux de Robocopy après un preseed pour vous assurer que vous comprenez quels fichiers ont été ignorés en raison de verrous exclusifs.

Pour utiliser Robocopy pour prédéfini de fichiers pour la réplication DFS, procédez comme suit :

1. [Téléchargez et installez la dernière version de Robocopy.](#step-1:-download-and-install-the-latest-version-of-robocopy)
2. [Stabilisez les fichiers qui seront répliqués.](#step-2:-stabilize-files-that-will-be-replicated)
3. [Copier les fichiers répliqués vers le serveur de destination.](#step-3:-copy-the-replicated-files-to-the-destination-server)

## <a name="prerequisites"></a>Prérequis

Comme un preseed n’implique pas directement de la réplication DFS, il vous suffit de répondre à la configuration requise pour effectuer une copie de fichiers avec Robocopy.

- Vous avez besoin d’un compte qui est membre du groupe Administrateurs local sur les serveurs source et de destination.

- Installer la version la plus récente de Robocopy sur le serveur que vous utiliserez pour copier les fichiers, le serveur source ou le serveur de destination ; Vous devez installer la version la plus récente pour la version de système d’exploitation. Pour obtenir des instructions, consultez [étape 2 : Stabiliser les fichiers qui seront répliqués](#step-2:-stabilize-files-that-will-be-replicated). À moins que vous sont un preseed des fichiers à partir d’un serveur exécutant Windows Server 2003 R2, vous pouvez exécuter Robocopy sur le serveur source ou de destination. Le serveur de destination, ce qui suit généralement la plus récente version de système d’exploitation, vous donne accès à la version la plus récente de Robocopy.

- Assurez-vous que l’espace de stockage suffisant est disponible sur le lecteur de destination. Ne créez pas un dossier sur le chemin d’accès que vous souhaitez copier vers : Robocopy doit créer le dossier racine.
    
    >[!NOTE]
    >Lorsque vous décidez de la quantité d’espace à allouer pour les fichiers preseeded, prendre en compte la croissance des données attendue au fil du temps et stockage requise pour la réplication DFS. Pour l’aide de la planification, consultez [modifier la taille du Quota du dossier intermédiaire et conflit d’et supprimé le dossier](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754229(v=ws.11)) dans [la gestion de la réplication DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754771(v=ws.11)>).

- Sur le serveur source, vous pouvez également installer Process Monitor ou Process Explorer, que vous pouvez utiliser pour vérifier pour les applications qui verrouillent des fichiers. Pour télécharger des informations, consultez [Process Monitor](https://docs.microsoft.com/sysinternals/downloads/procmon) et [Process Explorer](https://docs.microsoft.com/sysinternals/downloads/process-explorer).

## <a name="step-1-download-and-install-the-latest-version-of-robocopy"></a>Étape 1 : Téléchargez et installez la dernière version de Robocopy

Avant d’utiliser Robocopy pour prédéfini de fichiers, vous devez télécharger et installer la dernière version de **Robocopy.exe**. Cela garantit que la réplication DFS n’ignorer les fichiers en raison de problèmes dans les versions de Robocopy.

La source de la dernière version compatible de Robocopy dépend de la version de Windows Server qui s’exécute sur le serveur. Pour plus d’informations sur le téléchargement du correctif avec la version la plus récente de Robocopy pour Windows Server 2008 R2 ou Windows Server 2008, consultez [liste des correctifs actuellement disponibles pour les technologies de système de fichiers distribués (DFS, Distributed File System) dans Windows Server 2008 et dans Windows Server 2008 R2](https://support.microsoft.com/help/968429/list-of-currently-available-hotfixes-for-distributed-file-system-dfs-t).

Vous pouvez également rechercher et installer le dernier correctif pour un système d’exploitation en procédant comme suit.

### <a name="locate-and-install-the-latest-version-of-robocopy-for-a-specific-version-of-windows-server"></a>Recherchez et installez la dernière version de Robocopy pour une version spécifique de Windows Server

1. Dans un navigateur web, ouvrez [ https://support.microsoft.com ](https://support.microsoft.com/).

2. Dans **prise en charge de la recherche**, entrez les informations suivantes en chaîne, en remplaçant `<operating system version>` avec le système d’exploitation approprié, puis appuyez sur ENTRÉE :
    
    ```robocopy.exe kbqfe "<operating system version>"```
    
    Par exemple, entrez **robocopy.exe kbqfe « Windows Server 2008 R2 »**.

3. Recherchez et téléchargez le correctif avec le numéro d’ID la plus élevé (autrement dit, la dernière version).

4. Installez le correctif logiciel sur le serveur.

## <a name="step-2-stabilize-files-that-will-be-replicated"></a>Étape 2 : Stabiliser les fichiers qui seront répliqués

Après avoir installé la dernière version de Robocopy sur le serveur, vous devez empêcher fichiers verrouillés de copier blocage à l’aide des méthodes décrites dans le tableau suivant. La plupart des applications ne verrouillent pas exclusivement les fichiers. Toutefois, pendant les opérations normales, un petit pourcentage de fichiers est peut-être verrouillé sur les serveurs de fichiers.

|Source du verrou|Explication|Solution|
|---|---|---|
|Les utilisateurs ouvrir à distance des fichiers sur des partages.|Employés de se connecter à un serveur de fichiers standard et modifier des documents, contenu multimédia ou autres fichiers. Parfois appelé le dossier de base traditionnel ou charges de travail de données partagée.|Effectuer des opérations de Robocopy pendant uniquement creuses, les heures non ouvrées. Cela réduit le nombre de fichiers Robocopy doit ignorer pendant un preseed.<br><br>Envisagez de définir temporairement des accès en lecture seule sur les partages de fichiers qui seront répliqués à l’aide de la commande Windows PowerShell **Grant-SmbShareAccess** et **SmbSession de fermeture** applets de commande. Si vous définissez des autorisations pour un groupe communs tels que tout le monde ou utilisateurs authentifiés à lire, les utilisateurs standard peuvent être moins susceptibles d’ouvrir des fichiers avec les verrous exclusifs (si leurs applications détectent l’accès en lecture seule lorsque les fichiers sont ouverts).<br><br>Vous pouvez également envisager de définir une règle de pare-feu temporaire pour le port SMB 445 en entrée pour ce serveur afin de bloquer l’accès aux fichiers ou utilisez le **SmbShareAccess de bloc** applet de commande. Toutefois, ces deux méthodes sont très dangereuses pour opérations de l’utilisateur.|
|Applications ouvrir des fichiers locaux.|Des charges de travail s’exécutant sur un serveur de fichiers parfois verrouille les fichiers.|Temporairement désactiver ou désinstaller les applications qui verrouillent des fichiers. Vous pouvez utiliser Process Monitor ou Process Explorer pour déterminer les applications qui verrouillent des fichiers. Pour télécharger Process Monitor ou Process Explorer, visitez le [Process Monitor](https://docs.microsoft.com/sysinternals/downloads/procmon) et [Process Explorer](https://docs.microsoft.com/sysinternals/downloads/process-explorer) pages.|

## <a name="step-3-copy-the-replicated-files-to-the-destination-server"></a>Étape 3 : Copiez les fichiers répliqués sur le serveur de destination

Après la réduction des verrous sur les fichiers qui seront répliqués, vous pouvez prédéfinissez les fichiers à partir du serveur source vers le serveur de destination.

>[!NOTE]
>Vous pouvez exécuter Robocopy sur l’ordinateur source ou de l’ordinateur de destination. La procédure suivante décrit l’exécution de Robocopy sur le serveur de destination qui exécute généralement un système d’exploitation plus récent, pour tirer parti de toutes les capacités Robocopy supplémentaires susceptibles de fournir le système d’exploitation plus récent.

### <a name="preseed-the-replicated-files-onto-the-destination-server-with-robocopy"></a>Prédéfinir les fichiers répliqués sur le serveur de destination avec Robocopy

1. Connectez-vous au serveur de destination avec un compte qui est membre du groupe Administrateurs local sur les serveurs source et de destination.

2. Ouvrez une invite de commandes avec élévation de privilèges.

3. Pour prédéfini les fichiers à partir de la source au serveur de destination, exécutez la commande suivante, en remplaçant votre propre source, destination et chemins d’accès du fichier journal pour les valeurs entre crochets :
    
    ```PowerShell
    robocopy "<source replicated folder path>" "<destination replicated folder path>" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:<log file path> /v
    ```
    
    Cette commande copie tout le contenu du dossier source dans le dossier de destination, avec les paramètres suivants :
    
    |Paramètre|Description|
    |---|---|
    |«\<source répliquées de chemin d’accès du dossier\>»|Spécifie le dossier source à prédéfini sur le serveur de destination.|
    |«\<destination répliquées de chemin d’accès du dossier\>»|Spécifie le chemin d’accès au dossier qui stockera les fichiers preseeded.<br><br>Le dossier de destination ne doit pas déjà exister sur le serveur de destination. Pour obtenir les hachages de fichier correspondant, Robocopy doit créer le dossier racine lorsqu’il preseeds les fichiers.|
    |/e|Copie les sous-répertoires et leurs fichiers, mais aussi les sous-répertoires vides.|
    |/b|Copie les fichiers en mode de sauvegarde.|
    |/copyal|Copie toutes les informations de fichier, y compris les données, attributs, horodatages, la liste de contrôle d’accès NTFS (ACL), les informations relatives au propriétaire et informations d’audit.|
    |/r:6|Retente l’opération six fois lorsqu’une erreur se produit.|
    |/w:5|Attend 5 secondes entre chaque tentative.|
    |MT:64|Copie les 64 fichiers simultanément.|
    |/xd DfsrPrivate|Exclut le dossier DfsrPrivate.|
    |/tee|Écrit la sortie de l’état dans la fenêtre de console, ainsi que dans le fichier journal.|
    |/ log \<chemin du fichier journal >|Spécifie le fichier journal dans lequel écrire. Remplace le contenu du fichier existant. (Pour ajouter les entrées dans le fichier journal existant, utilisez `/log+ <log file path>`.)|
    |/v|Génère une sortie détaillée qui inclut les fichiers ignorés.|
    
    Par exemple, la commande suivante réplique les fichiers à partir du dossier source sont répliquées, E:\\RF01, sur le lecteur de données D sur le serveur de destination :
    
    ```PowerShell
    robocopy.exe "\\srv01\e$\rf01" "d:\rf01" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:c:\temp\preseedsrv02.log
    ```
    
    >[!NOTE]
    >Nous vous recommandons d’utiliser les paramètres décrits ci-dessus, lorsque vous utilisez Robocopy pour prédéfini de fichiers pour la réplication DFS. Toutefois, vous pouvez modifier certains de leurs valeurs, ou ajouter des paramètres supplémentaires. Par exemple, vous constaterez peut-être et de test que vous avez la capacité à définir une valeur plus élevée (nombre de threads) pour la */MT* paramètre. En outre, si vous allez principalement répliquer les fichiers plus volumineux, vous pourrez peut-être augmenter les performances de copie en ajoutant le **/j** option pour les e/s sans tampon. Pour plus d’informations sur les paramètres de Robocopy, consultez le [Robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy) référence de ligne de commande.

    >[!WARNING]
    >Pour éviter la perte de données lorsque vous utilisez Robocopy pour prédéfini de fichiers pour la réplication DFS, ne modifiez pas le suivantes aux paramètres recommandés :
    >- N’utilisez pas le */mir* paramètre (ce qui reflète une arborescence de répertoires) ou le */mov* paramètre (qui déplace les fichiers, puis les supprime de la source).
    >-  Ne supprimez pas le **/e**, **/b**, et **/copyall** options.

4. Une fois la copie terminée, examinez le journal des erreurs ou les fichiers ignorés. Utilisez Robocopy pour copier des fichiers ignorés individuellement au lieu de la recopie de l’ensemble des fichiers. Si les fichiers ont été ignorés en raison de verrous exclusifs, essayez de copier des fichiers avec Robocopy ou accepter que ces fichiers nécessitera réplication over-the-wire par la réplication DFS lors de la synchronisation initiale.

## <a name="next-step"></a>Étape suivante

Après avoir terminé la copie initiale et que vous utilisez Robocopy pour résoudre les problèmes avec les fichiers ignorés autant que possible, vous allez utiliser le **Get-DfsrFileHash** applet de commande dans Windows PowerShell ou le **Dfsrdiag** commande validez les fichiers preseeded en comparant les hachages de fichier sur les serveurs source et de destination. Pour obtenir des instructions détaillées, consultez [étape 2 : Valider les fichiers Preseeded pour la réplication DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495042(v%3dws.11)>).