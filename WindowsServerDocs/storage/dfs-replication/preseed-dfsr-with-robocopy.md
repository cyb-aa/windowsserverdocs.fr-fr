---
title: Utiliser Robocopy pour prédéfinir des fichiers pour la réplication DFS
description: Guide pratique de l’utilisation de Robocopy pour prédéfinir des fichiers pour la réplication DFS.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: ea5cd954dde6d4fa8fcaa7874f75cb9588115ab1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402129"
---
# <a name="use-robocopy-to-preseed-files-for-dfs-replication"></a>Utiliser Robocopy pour prédéfinir des fichiers pour la réplication DFS

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Cette rubrique explique comment utiliser l’outil en ligne de commande **Robocopy.exe** pour prédéfinir les fichiers lors de la configuration de la réplication du système de fichiers DFS (également appelée DFSR ou DFS-R) dans Windows Server. En prédéfinissant les fichiers avant de configurer la réplication DFS, d’ajouter un nouveau partenaire de réplication ou de remplacer un serveur, vous pouvez accélérer la synchronisation initiale et activer le clonage de la base de données de réplication DFS dans Windows Server 2012 R2. La méthode Robocopy est l’une des nombreuses méthodes de prédéfinition. Pour obtenir une vue d’ensemble, consultez [Étape 1 : Prédéfinir des fichiers pour la réplication DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495046(v%3dws.11)>).

L’utilitaire en ligne de commande Robocopy (Robust File Copy, copie de fichiers robuste) est inclus avec Windows Server. Cet utilitaire fournit des options complètes qui incluent la sécurité de la copie, la prise en charge des API de sauvegarde, des fonctionnalités de nouvelle tentative et la journalisation. Les versions ultérieures incluent la prise en charge du multithreading et des E/S non mises en mémoire tampon.

>[!IMPORTANT]
>Robocopy ne copie pas exclusivement les fichiers verrouillés. Si les utilisateurs ont tendance à verrouiller de nombreux fichiers pendant de longues durées sur vos serveurs de fichiers, envisagez d’utiliser une autre méthode de prédéfinition. La prédéfinition ne nécessite pas une correspondance parfaite entre les listes de fichiers sur le serveur source et sur le serveur destination, mais plus le nombre de fichiers qui n’existent pas quand la synchronisation initiale est effectuée pour la réplication DFS, moins la prédéfinition est efficace. Pour réduire les conflits de verrous, utilisez Robocopy pendant les heures creuses de votre organisation. Examinez toujours les journaux Robocopy après la prédéfinition pour vérifier que vous comprenez quels fichiers ont été ignorés en raison de verrous exclusifs.

Pour utiliser Robocopy afin de prédéfinir des fichiers pour la réplication DFS, effectuez les étapes suivantes :

1. [Télécharger et installer la dernière version de Robocopy.](#step-1-download-and-install-the-latest-version-of-robocopy)
2. [Stabiliser les fichiers qui seront répliqués.](#step-2-stabilize-files-that-will-be-replicated)
3. [Copier les fichiers répliqués sur le serveur de destination.](#step-3-copy-the-replicated-files-to-the-destination-server)

## <a name="prerequisites"></a>Prérequis

Étant donné que la prédéfinition n’implique pas directement la réplication DFS, vous devez uniquement satisfaire les exigences relatives à l’exécution d’une copie de fichiers avec Robocopy.

- Vous avez besoin d’un compte qui est membre du groupe Administrateurs local à la fois sur le serveur source et sur le serveur de destination.

- Installez la version la plus récente de Robocopy sur le serveur que vous allez utiliser pour copier les fichiers (soit le serveur source, soit le serveur de destination). Vous devez installer la version la plus récente pour la version du système d’exploitation. Pour obtenir des instructions, consultez [Étape 2 : Stabiliser les fichiers qui seront répliqués](#step-2-stabilize-files-that-will-be-replicated). À moins que vous ne prédéfinissiez des fichiers à partir d’un serveur exécutant Windows Server 2003 R2, vous pouvez exécuter Robocopy sur le serveur source ou sur le serveur de destination. Le serveur de destination, qui a généralement la version du système d’exploitation la plus récente, vous donne accès à la version la plus récente de Robocopy.

- Vérifiez que l’espace de stockage disponible sur le lecteur de destination est suffisant. Ne créez pas de dossier sur le chemin où vous envisagez d’effectuer la copie : Robocopy doit créer le dossier racine.
    
    >[!NOTE]
    >Quand vous déterminez la quantité d’espace à allouer aux fichiers prédéfinis, prenez en compte la croissance attendue des données au fil du temps et les besoins en stockage pour la réplication DFS. Pour obtenir de l’aide sur la planification, consultez [Modifier la taille du quota du dossier intermédiaire et du dossier des fichiers en conflit et supprimés](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754229(v=ws.11)) dans [Gestion de la réplication DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754771(v=ws.11)>).

- Sur le serveur source, vous pouvez éventuellement installer Process Monitor ou l’Explorateur de processus, et les utiliser pour rechercher les applications qui verrouillent des fichiers. Pour obtenir des informations sur le téléchargement, consultez [Process Monitor](https://docs.microsoft.com/sysinternals/downloads/procmon) et [Explorateur de processus](https://docs.microsoft.com/sysinternals/downloads/process-explorer).

## <a name="step-1-download-and-install-the-latest-version-of-robocopy"></a>Étape 1 : Télécharger et installer la dernière version de Robocopy

Avant d’utiliser Robocopy pour prédéfinir des fichiers, vous devez télécharger et installer la dernière version de **Robocopy.exe**. Cela garantit que la réplication DFS n’ignore pas des fichiers en raison de problèmes dans les versions finales de Robocopy.

La source de la dernière version compatible de Robocopy dépend de la version de Windows Server en cours d’exécution sur le serveur. Pour plus d’informations sur le téléchargement du correctif logiciel comportant la version la plus récente de Robocopy pour Windows Server 2008 R2 ou Windows Server 2008, consultez [Liste des correctifs actuellement disponibles pour les technologies de système de fichiers distribués (DFS, Distributed File System) dans Windows Server 2008 et Windows Server 2008 R2](https://support.microsoft.com/help/968429/list-of-currently-available-hotfixes-for-distributed-file-system-dfs-t).

Vous pouvez également localiser et installer le dernier correctif logiciel pour un système d’exploitation en effectuant les étapes suivantes.

### <a name="locate-and-install-the-latest-version-of-robocopy-for-a-specific-version-of-windows-server"></a>Localiser et installer la dernière version de Robocopy pour une version spécifique de Windows Server

1. Dans un navigateur web, ouvrez [https://support.microsoft.com](https://support.microsoft.com/).

2. Dans **Rechercher dans le support technique**, entrez la chaîne suivante, en remplaçant `<operating system version>` par le système d’exploitation approprié, puis appuyez sur la touche Entrée :
    
    ```robocopy.exe kbqfe "<operating system version>"```
    
    Par exemple, entrez **robocopy.exe kbqfe "Windows Server 2008 R2"** .

3. Recherchez et téléchargez le correctif logiciel avec le numéro d’ID le plus élevé (c’est-à-dire, la dernière version).

4. Installez le correctif logiciel sur le serveur.

## <a name="step-2-stabilize-files-that-will-be-replicated"></a>Étape 2 : Stabiliser les fichiers qui seront répliqués

Une fois que vous avez installé la dernière version de Robocopy sur le serveur, vous devez empêcher les fichiers verrouillés de bloquer la copie à l’aide des méthodes décrites dans le tableau suivant. La plupart des applications ne verrouillent pas les fichiers de manière exclusive. Toutefois, au cours d’opérations normales, un petit pourcentage de fichiers peuvent être verrouillés sur les serveurs de fichiers.

|Source du verrou|Explication|Solution|
|---|---|---|
|Les utilisateurs ouvrent à distance des fichiers sur des partages.|Les employés se connectent à un serveur de fichiers standard et modifient des documents, du contenu multimédia ou d’autres fichiers. Parfois appelé « dossier de base traditionnel » ou « charges de travail de données partagées ».|Effectuez les opérations Robocopy uniquement pendant les heures creuses, en dehors des heures de travail. Cela réduit le nombre de fichiers que Robocopy doit ignorer pendant la prédéfinition.<br><br>Envisagez de définir temporairement l’accès en lecture seule sur les partages de fichiers qui seront répliqués à l’aide des applets de commande Windows PowerShell **Grant-SmbShareAccess** et **Close-SmbSession**. Si vous définissez des autorisations pour un groupe commun comme Tout le monde ou Utilisateurs authentifiés sur LECTURE, les utilisateurs standard sont peut-être moins susceptibles d’ouvrir des fichiers avec des verrous exclusifs (si leurs applications détectent l’accès en lecture seule lors de l’ouverture de fichiers).<br><br>Vous pouvez également envisager de définir une règle de pare-feu temporaire pour le port SMB 445 entrant sur ce serveur afin de bloquer l’accès aux fichiers ou d’utiliser l’applet de commande **Block-SmbShareAccess**. Toutefois, ces deux méthodes perturbent beaucoup les opérations de l’utilisateur.|
|Les applications ouvrent des fichiers locaux.|Les charges de travail d’application qui s’exécutent sur un serveur de fichiers verrouillent parfois des fichiers.|Désactivez ou désinstallez temporairement les applications qui verrouillent des fichiers. Vous pouvez utiliser Process Monitor ou l’Explorateur de processus pour déterminer les applications qui verrouillent des fichiers. Pour télécharger Process Monitor ou l’Explorateur de processus, consultez les pages [Process Monitor](https://docs.microsoft.com/sysinternals/downloads/procmon) et [Explorateur de processus](https://docs.microsoft.com/sysinternals/downloads/process-explorer).|

## <a name="step-3-copy-the-replicated-files-to-the-destination-server"></a>Étape 3 : Copier les fichiers répliqués sur le serveur de destination

Après avoir réduit les verrous sur les fichiers qui seront répliqués, vous pouvez prédéfinir les fichiers du serveur source sur le serveur de destination.

>[!NOTE]
>Vous pouvez exécuter Robocopy sur l’ordinateur source ou sur l’ordinateur de destination. La procédure suivante décrit l’exécution de Robocopy sur le serveur de destination, qui exécute généralement un système d’exploitation plus récent, pour tirer parti des fonctionnalités Robocopy supplémentaires que le système d’exploitation plus récent peut fournir.

### <a name="preseed-the-replicated-files-onto-the-destination-server-with-robocopy"></a>Prédéfinir les fichiers répliqués sur le serveur de destination avec Robocopy

1. Connectez-vous au serveur de destination avec un compte qui est membre du groupe Administrateurs local à la fois sur le serveur source et sur le serveur de destination.

2. Ouvrez une invite de commandes avec des privilèges élevés.

3. Pour prédéfinir les fichiers du serveur source sur le serveur de destination, exécutez la commande suivante, en remplaçant les valeurs entre crochets par le chemin de vos propres source, destination et fichier journal :
    
    ```PowerShell
    robocopy "<source replicated folder path>" "<destination replicated folder path>" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:<log file path> /v
    ```
    
    Cette commande copie tout le contenu du dossier source vers le dossier de destination, avec les paramètres suivants :
    
    |Paramètre|Description|
    |---|---|
    |« \<source replicated folder path\> » (chemin du dossier répliqué source)|Spécifie le dossier source à prédéfinir sur le serveur de destination.|
    |« \<destination replicated folder path\> » (chemin du dossier répliqué de destination)|Spécifie le chemin du dossier qui stockera les fichiers prédéfinis.<br><br>Le dossier de destination ne doit pas déjà exister sur le serveur de destination. Pour obtenir des hachages de fichiers correspondants, Robocopy doit créer le dossier racine quand il prédéfinit les fichiers.|
    |/e|Copie les sous-répertoires et leurs fichiers, ainsi que les sous-répertoires vides.|
    |/b|Copie les fichiers en mode de sauvegarde.|
    |/copyall|Copie toutes les informations de fichier, notamment les données, les attributs, les horodatages, la liste de contrôle d’accès (ACL) NTFS, les informations sur le propriétaire et les informations d’audit.|
    |/r:6|Retente l’opération six fois quand une erreur se produit.|
    |/w:5|Attend 5 secondes entre les nouvelles tentatives.|
    |MT:64|Copie 64 fichiers simultanément.|
    |/xd DfsrPrivate|Exclut le dossier DfsrPrivate.|
    |/tee|Écrit la sortie de l’état dans la fenêtre de console, ainsi que dans le fichier journal.|
    |/log \<chemin du fichier journal>|Spécifie le fichier journal à écrire. Remplace le contenu existant du fichier. (Pour ajouter les entrées au fichier journal existant, utilisez `/log+ <log file path>`.)|
    |/v|Produit une sortie détaillée qui inclut les fichiers ignorés.|
    
    Par exemple, la commande suivante réplique des fichiers à partir du dossier source répliqué, E:\\RF01, sur le lecteur de données D situé sur le serveur de destination :
    
    ```PowerShell
    robocopy.exe "\\srv01\e$\rf01" "d:\rf01" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:c:\temp\preseedsrv02.log
    ```
    
    >[!NOTE]
    >Nous vous recommandons d’utiliser les paramètres décrits ci-dessus quand vous utilisez Robocopy en vue de prédéfinir des fichiers pour la réplication DFS. Toutefois, vous pouvez changer certaines de leurs valeurs ou ajouter des paramètres supplémentaires. Par exemple, vous pouvez découvrir, par le biais de test, que vous avez la possibilité de définir une valeur supérieure (nombre de threads) pour le paramètre  */MT*. De plus, si vous allez principalement répliquer des fichiers plus volumineux, vous pourrez éventuellement augmenter les performances de copie en ajoutant l’option  **/j** pour les E/S non mises en mémoire tampon. Pour plus d’informations sur les paramètres Robocopy, consultez la référence de la ligne de commande [Robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy).

    >[!WARNING]
    >Pour éviter une perte de données potentielle quand vous utilisez Robocopy afin de prédéfinir des fichiers pour la réplication DFS, n’apportez pas les changements suivants aux paramètres recommandés :
    >- N’utilisez pas le paramètre */mir* (qui reflète une arborescence de répertoires) ou le paramètre */mov* (qui déplace les fichiers, puis les supprime de la source).
    >-  Ne supprimez pas les options  **/e**, **/b** et **/copyall**.

4. Une fois la copie terminée, examinez le journal pour vérifier s’il contient des erreurs ou des fichiers ignorés. Utilisez Robocopy pour copier individuellement tous les fichiers ignorés au lieu de recopier la totalité de l’ensemble des fichiers. Si des fichiers ont été ignorés en raison de verrous exclusifs, essayez de copier des fichiers individuels avec Robocopy ultérieurement ou acceptez que ces fichiers nécessiteront une réplication sur le réseau par la réplication DFS lors de la synchronisation initiale.

## <a name="next-step"></a>Étape suivante

Après avoir terminé la copie initiale et utilisé Robocopy pour résoudre les problèmes avec le plus grand nombre de fichiers ignorés possible, vous allez utiliser l’applet de commande **Get-DfsrFileHash** dans Windows PowerShell ou la commande **Dfsrdiag** pour valider les fichiers prédéfinis en comparant les hachages de fichiers sur les serveurs source et de destination. Pour obtenir des instructions détaillées, consultez [Étape 2 : Valider les fichiers prédéfinis pour la réplication DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495042(v%3dws.11)>).
