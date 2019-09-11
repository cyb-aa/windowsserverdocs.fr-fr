---
title: Utilisez Robocopy pour préamorcer des fichiers pour réplication DFS
description: Comment utiliser Robocopy. exe pour préamorcer des fichiers pour des réplication DFS.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 4a0cad3c685c8609784c7096fe31d55294712c2e
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871977"
---
# <a name="use-robocopy-to-preseed-files-for-dfs-replication"></a>Utilisez Robocopy pour préamorcer des fichiers pour réplication DFS

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Cette rubrique explique comment utiliser l’outil de ligne de commande, **Robocopy. exe**, pour présemencer les fichiers lors de la configuration de la réplication pour la réplication système de fichiers DFS (DFS) (également appelée DFSR ou DFS-R) dans Windows Server. En présemencent les fichiers avant de configurer réplication DFS, d’ajouter un nouveau partenaire de réplication ou de remplacer un serveur, vous pouvez accélérer la synchronisation initiale et activer le clonage de la base de données réplication DFS dans Windows Server 2012 R2. La méthode Robocopy est l’une des nombreuses méthodes de préamorçage. pour obtenir une vue d’ensemble, consultez [étape 1 : fichiers préamorcés pour réplication DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495046(v%3dws.11)>).

L’utilitaire de ligne de commande Robocopy (copie de fichiers robuste) est inclus dans Windows Server. L’utilitaire fournit des options complètes qui incluent la copie de la sécurité, la prise en charge de l’API de sauvegarde, les fonctionnalités de nouvelle tentative et la journalisation. Les versions ultérieures incluent la prise en charge des e/s multithread et sans mise en mémoire tampon.

>[!IMPORTANT]
>Robocopy ne copie pas les fichiers exclusivement verrouillés. Si les utilisateurs ont tendance à verrouiller de nombreux fichiers sur de longues périodes sur vos serveurs de fichiers, envisagez d’utiliser une autre méthode de préamorçage. Le préamorçage ne nécessite pas une correspondance parfaite entre les listes de fichiers sur les serveurs source et de destination, mais plus les fichiers qui n’existent pas lors de la synchronisation initiale sont exécutés pour réplication DFS, le préamorçage le moins efficace est. Pour réduire les conflits de verrouillage, utilisez Robocopy pendant les heures creuses pour votre organisation. Examinez toujours les journaux Robocopy après le préamorçage pour vous assurer que vous comprenez quels fichiers ont été ignorés en raison de verrous exclusifs.

Pour utiliser Robocopy afin de préamorcer des fichiers pour réplication DFS, procédez comme suit :

1. [Téléchargez et installez la dernière version de Robocopy.](#step-1-download-and-install-the-latest-version-of-robocopy)
2. [Stabilisez les fichiers qui seront répliqués.](#step-2-stabilize-files-that-will-be-replicated)
3. [Copiez les fichiers répliqués sur le serveur de destination.](#step-3-copy-the-replicated-files-to-the-destination-server)

## <a name="prerequisites"></a>Prérequis

Étant donné que le préamorçage n’implique pas directement réplication DFS, vous devez uniquement répondre aux conditions requises pour effectuer une copie de fichiers avec Robocopy.

- Vous avez besoin d’un compte membre du groupe Administrateurs local sur les serveurs source et de destination.

- Installez la version la plus récente de Robocopy sur le serveur que vous allez utiliser pour copier les fichiers, soit le serveur source, soit le serveur de destination. vous devez installer la version la plus récente pour la version du système d’exploitation. Pour obtenir des instructions [, consultez étape 2 : Stabilisez les fichiers qui seront répliqués](#step-2-stabilize-files-that-will-be-replicated). À moins que vous ne présemencez des fichiers à partir d’un serveur exécutant Windows Server 2003 R2, vous pouvez exécuter Robocopy sur le serveur source ou de destination. Le serveur de destination, qui a généralement la version la plus récente du système d’exploitation, vous donne accès à la version la plus récente de Robocopy.

- Assurez-vous que l’espace de stockage disponible est suffisant sur le lecteur de destination. Ne créez pas de dossier sur le chemin d’accès que vous envisagez de copier : Robocopy doit créer le dossier racine.
    
    >[!NOTE]
    >Lorsque vous déterminez la quantité d’espace à allouer aux fichiers préamorcés, pensez à la croissance attendue des données au fil du temps et des besoins de stockage pour réplication DFS. Pour obtenir de l’aide sur la planification, consultez [modifier la taille du quota du dossier intermédiaire et le dossier des fichiers en conflit et supprimés](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754229(v=ws.11)) dans [gestion des réplication DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754771(v=ws.11)>).

- Sur le serveur source, vous pouvez éventuellement installer process Monitor ou Process Explorer, que vous pouvez utiliser pour rechercher les applications qui verrouillent des fichiers. Pour obtenir des informations sur le téléchargement, consultez [Process Monitor](https://docs.microsoft.com/sysinternals/downloads/procmon) and [Process Explorer](https://docs.microsoft.com/sysinternals/downloads/process-explorer).

## <a name="step-1-download-and-install-the-latest-version-of-robocopy"></a>Étape 1 : Télécharger et installer la dernière version de Robocopy

Avant d’utiliser Robocopy pour préamorcer des fichiers, vous devez télécharger et installer la dernière version de **Robocopy. exe**. Cela permet de s’assurer que réplication DFS n’ignore pas les fichiers en raison de problèmes au sein des versions d’expédition de Robocopy.

La source de la dernière version de Robocopy compatible dépend de la version de Windows Server qui s’exécute sur le serveur. Pour plus d’informations sur le téléchargement du correctif logiciel avec la version la plus récente de Robocopy pour Windows Server 2008 R2 ou Windows Server 2008, consultez la [liste des correctifs logiciels actuellement disponibles pour les technologies système de fichiers DFS (DFS) dans Windows server 2008 et dans Windows Server 2008 R2](https://support.microsoft.com/help/968429/list-of-currently-available-hotfixes-for-distributed-file-system-dfs-t).

Vous pouvez également localiser et installer le correctif logiciel le plus récent pour un système d’exploitation en procédant comme suit.

### <a name="locate-and-install-the-latest-version-of-robocopy-for-a-specific-version-of-windows-server"></a>Localiser et installer la dernière version de Robocopy pour une version spécifique de Windows Server

1. Dans un navigateur Web, ouvrez [https://support.microsoft.com](https://support.microsoft.com/).

2. Dans la **prise en charge**de la recherche, entrez `<operating system version>` la chaîne suivante, en remplaçant par le système d’exploitation approprié, puis appuyez sur la touche entrée :
    
    ```robocopy.exe kbqfe "<operating system version>"```
    
    Par exemple, entrez **Robocopy. exe kbqfe « Windows Server 2008 R2 »** .

3. Recherchez et téléchargez le correctif avec le numéro d’ID le plus élevé (c’est-à-dire, la dernière version).

4. Installez le correctif logiciel sur le serveur.

## <a name="step-2-stabilize-files-that-will-be-replicated"></a>Étape 2 : Stabiliser les fichiers qui seront répliqués

Une fois que vous avez installé la dernière version de Robocopy sur le serveur, vous devez empêcher les fichiers verrouillés de bloquer la copie à l’aide des méthodes décrites dans le tableau suivant. La plupart des applications ne verrouillent pas exclusivement les fichiers. Toutefois, pendant les opérations normales, un petit pourcentage de fichiers peut être verrouillé sur les serveurs de fichiers.

|Source du verrou|Explication|Atténuation|
|---|---|---|
|Les utilisateurs ouvrent à distance des fichiers sur les partages.|Les employés se connectent à un serveur de fichiers standard et modifient des documents, du contenu multimédia ou d’autres fichiers. Parfois appelé dossier de travail traditionnel ou charges de travail de données partagées.|Effectuez uniquement des opérations Robocopy pendant les heures creuses, hors des heures de bureau. Cela réduit le nombre de fichiers que Robocopy doit ignorer pendant le préamorçage.<br><br>Envisagez de définir temporairement l’accès en lecture seule sur les partages de fichiers qui seront répliqués à l’aide des applets de commande **Grant-SmbShareAccess** et **Close-SmbSession** de Windows PowerShell. Si vous définissez des autorisations pour un groupe commun comme tout le monde ou les utilisateurs authentifiés pour la lecture, les utilisateurs standard peuvent être moins enclins à ouvrir des fichiers avec des verrous exclusifs (si leurs applications détectent l’accès en lecture seule lorsque des fichiers sont ouverts).<br><br>Vous pouvez également envisager de définir une règle de pare-feu temporaire pour le port SMB 445 entrant sur ce serveur pour bloquer l’accès aux fichiers ou utiliser l’applet **de commande Block-SmbShareAccess** . Toutefois, ces deux méthodes sont très perturbatrices pour les opérations de l’utilisateur.|
|Les applications ouvrent fichiers locaux.|Les charges de travail d’application exécutées sur un serveur de fichiers verrouillent parfois des fichiers.|Désactivez ou désinstallez temporairement les applications qui verrouillent des fichiers. Vous pouvez utiliser process Monitor ou Process Explorer pour déterminer les applications qui verrouillent des fichiers. Pour télécharger process Monitor ou Process Explorer, visitez les pages [Process Monitor](https://docs.microsoft.com/sysinternals/downloads/procmon) et [Process Explorer](https://docs.microsoft.com/sysinternals/downloads/process-explorer) .|

## <a name="step-3-copy-the-replicated-files-to-the-destination-server"></a>Étape 3 : Copier les fichiers répliqués sur le serveur de destination

Après avoir réduit les verrous sur les fichiers qui seront répliqués, vous pouvez préamorcer les fichiers du serveur source vers le serveur de destination.

>[!NOTE]
>Vous pouvez exécuter Robocopy sur l’ordinateur source ou sur l’ordinateur de destination. La procédure suivante décrit l’exécution de Robocopy sur le serveur de destination, qui exécute généralement un système d’exploitation plus récent, pour tirer parti des fonctionnalités Robocopy supplémentaires que le système d’exploitation plus récent peut fournir.

### <a name="preseed-the-replicated-files-onto-the-destination-server-with-robocopy"></a>Préamorcer les fichiers répliqués sur le serveur de destination avec Robocopy

1. Connectez-vous au serveur de destination à l’aide d’un compte membre du groupe Administrateurs local sur les serveurs source et de destination.

2. Ouvrez une invite de commandes avec élévation de privilèges.

3. Pour préamorcer les fichiers du serveur source au serveur de destination, exécutez la commande suivante, en remplaçant les chemins d’accès source, de destination et de fichier journal pour les valeurs entre crochets :
    
    ```PowerShell
    robocopy "<source replicated folder path>" "<destination replicated folder path>" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:<log file path> /v
    ```
    
    Cette commande copie tout le contenu du dossier source vers le dossier de destination, avec les paramètres suivants :
    
    |Paramètre|Description|
    |---|---|
    |«\<chemin d’accès au\>dossier répliqué source »|Spécifie le dossier source à préamorcer sur le serveur de destination.|
    |«\<chemin d’accès au\>dossier répliqué de destination »|Spécifie le chemin d’accès au dossier qui stocke les fichiers préamorcés.<br><br>Le dossier de destination ne doit pas déjà exister sur le serveur de destination. Pour atteindre les hachages de fichier correspondants, Robocopy doit créer le dossier racine lorsqu’il préamorce les fichiers.|
    |/e|Copie les sous-répertoires et leurs fichiers, ainsi que les sous-répertoires vides.|
    |/b|Copie les fichiers en mode de sauvegarde.|
    |/copyall|Copie toutes les informations de fichier, y compris les données, les attributs, les horodatages, la liste de contrôle d’accès (ACL) NTFS, les informations de propriétaire et les informations d’audit.|
    |/r : 6|Réessaie l’opération six fois lorsqu’une erreur se produit.|
    |/w : 5|Attend 5 secondes entre les nouvelles tentatives.|
    |MT : 64|Copie les fichiers 64 simultanément.|
    |/xD DfsrPrivate|Exclut le dossier DfsrPrivate.|
    |/tee|Écrit l’état de sortie dans la fenêtre de console, ainsi que dans le fichier journal.|
    |/log \<chemin d’accès du fichier journal >|Spécifie le fichier journal à écrire. Remplace le contenu existant du fichier. (Pour ajouter les entrées au fichier journal existant, utilisez `/log+ <log file path>`.)|
    |/v|Produit une sortie détaillée qui comprend des fichiers ignorés.|
    
    Par exemple, la commande suivante réplique des fichiers à partir du dossier source répliqué, E :\\RF01, sur le lecteur de données D sur le serveur de destination :
    
    ```PowerShell
    robocopy.exe "\\srv01\e$\rf01" "d:\rf01" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:c:\temp\preseedsrv02.log
    ```
    
    >[!NOTE]
    >Nous vous recommandons d’utiliser les paramètres décrits ci-dessus lorsque vous utilisez Robocopy pour préamorcer des fichiers pour des réplication DFS. Toutefois, vous pouvez modifier certaines de leurs valeurs ou ajouter des paramètres supplémentaires. Par exemple, vous pouvez vérifier que vous avez la capacité de définir une valeur supérieure (nombre de threads) pour le paramètre */MT* . En outre, si vous répliquez principalement des fichiers plus volumineux, vous pourrez peut-être augmenter les performances de copie en ajoutant l’option **/j** pour les e/s non mises en mémoire tampon. Pour plus d’informations sur les paramètres Robocopy, consultez la page de référence sur la ligne de commande [Robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy) .

    >[!WARNING]
    >Pour éviter une perte de données potentielle lorsque vous utilisez Robocopy pour préamorcer des fichiers pour des réplication DFS, n’apportez pas les modifications suivantes aux paramètres recommandés :
    >- N’utilisez pas le paramètre */Mir* (qui reflète une arborescence de répertoires) ou le paramètre */MOV* (qui déplace les fichiers, puis les supprime de la source).
    >-  Ne supprimez pas les options **/e**, **/b**et **/COPYALL** .

4. Une fois la copie terminée, examinez le journal pour y rechercher des erreurs ou des fichiers ignorés. Utilisez Robocopy pour copier tous les fichiers ignorés individuellement au lieu de recopier l’ensemble des fichiers. Si des fichiers ont été ignorés en raison de verrous exclusifs, essayez de copier des fichiers individuels avec Robocopy plus tard ou acceptez que ces fichiers requièrent la réplication sur le réseau par réplication DFS lors de la synchronisation initiale.

## <a name="next-step"></a>Étape suivante

Une fois que vous avez terminé la copie initiale et utilisé Robocopy pour résoudre les problèmes avec autant de fichiers ignorés que possible, vous allez utiliser l’applet de commande **DfsrFileHash** dans Windows PowerShell ou la commande **Dfsrdiag** pour valider les fichiers prédéfinis en comparant hachages de fichiers sur les serveurs source et de destination. Pour obtenir des instructions détaillées [, consultez étape 2 : Validez les fichiers préamorcés](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495042(v%3dws.11)>)pour réplication DFS.
