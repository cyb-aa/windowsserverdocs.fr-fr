---
title: Réglage des performances pour les serveurs de fichiers NFS
description: Réglage des performances pour les serveurs de fichiers NFS
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
author: phstee
ms.author: RoopeshB, NedPyle
ms.date: 10/16/2017
ms.openlocfilehash: 06a2a7206d3673046bd5a926a657bac91b02bf5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879010"
---
# <a name="performance-tuning-nfs-file-servers"></a>Serveurs de fichiers NFS de réglage des performances

## <a href="" id="servicesnfs"></a>Services pour le modèle NFS


Les sections suivantes fournissent des informations sur les Services Microsoft pour le modèle de système de fichiers réseau (NFS) pour la communication client-serveur. Étant donné que NFS v2 et v3 NFS sont toujours le plus largement déployé des versions du protocole, toutes les clés de Registre à l’exception MaxConcurrentConnectionsPerIp s’appliquent à NFS v2 et v3 NFS uniquement.

Aucun réglage de Registre n’est requis pour le protocole NFS version 4.1.

### <a name="service-for-nfs-model-overview"></a>Service pour une vue d’ensemble du modèle NFS

Microsoft Services pour NFS fournit une solution de partage de fichiers pour les entreprises qui disposent d’un environnement mixte Windows et UNIX. Ce modèle de communication se compose d’ordinateurs clients et un serveur. Applications sur les fichiers de demande du client qui sont trouvent sur le serveur via le redirecteur (Rdbss.sys) et le mini-redirecteur de NFS (Nfsrdr.sys). Le mini-redirecteur utilise le protocole NFS pour envoyer sa demande via TCP/IP. Le serveur reçoit plusieurs demandes des clients via TCP/IP et achemine les demandes vers le système de fichiers local (Ntfs.sys), qui accède à la pile de stockage.

La figure suivante montre le modèle de communication pour NFS.

![modèle de communication NFS](../../media/perftune-guide-nfs-model.png)

### <a name="tuning-parameters-for-nfs-file-servers"></a>Réglage des paramètres pour les serveurs de fichiers NFS

Le REG suivant\_les paramètres de Registre DWORD peuvent affecter les performances des serveurs de fichiers NFS :

-   **OptimalReads**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\OptimalReads
    ```

    La valeur par défaut est 0. Ce paramètre détermine si les fichiers sont ouverts pour le fichier\_RANDOM\_accès ou fichier\_séquentiel\_uniquement, en fonction des caractéristiques de charge de travail d’e/s. Définissez cette valeur sur 1 pour forcer les fichiers à ouvrir pour le fichier\_RANDOM\_accès. FICHIER\_RANDOM\_accès empêche le Gestionnaire de fichiers système et le cache de lecture anticipée.

    >[!NOTE]
    > Ce paramètre doit être évalué avec précaution, car elle peut avoir un impact potentiel sur le cache du système de fichier croître.


-   **RdWrHandleLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrHandleLifeTime
    ```

    La valeur par défaut est 5. Ce paramètre contrôle la durée de vie d’une entrée de cache NFS dans le cache de handle de fichier. Le paramètre fait référence aux entrées du cache qui ont un fichier NTFS ouvert associé à gérer. Durée de vie réelle est approximativement égale à RdWrHandleLifeTime multipliée par RdWrThreadSleepTime. La valeur minimale est 1 et la valeur maximale est de 60.

-   **RdWrNfsHandleLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsHandleLifeTime
    ```

    La valeur par défaut est 5. Ce paramètre contrôle la durée de vie d’une entrée de cache NFS dans le cache de handle de fichier. Le paramètre fait référence aux entrées du cache qui n’ont pas de gérer un fichier NTFS ouvert associé. Services pour NFS utilise ces entrées de cache pour stocker des attributs de fichier pour un fichier sans conserver un handle ouvert avec le système de fichiers. Durée de vie réelle est approximativement égale à RdWrNfsHandleLifeTime multipliée par RdWrThreadSleepTime. La valeur minimale est 1 et la valeur maximale est de 60.

-   **RdWrNfsReadHandlesLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsReadHandlesLifeTime
    ```

    La valeur par défaut est 5. Ce paramètre contrôle la durée de vie d’une lecture de l’entrée de cache dans le cache de handle de fichier NFS. Durée de vie réelle est approximativement égale à RdWrNfsReadHandlesLifeTime multipliée par RdWrThreadSleepTime. La valeur minimale est 1 et la valeur maximale est de 60.

-   **RdWrThreadSleepTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrThreadSleepTime
    ```

    La valeur par défaut est 5. Ce paramètre contrôle le délai d’attente avant d’exécuter le thread de nettoyage sur le cache de handle de fichier. La valeur est en cycles, et il n’est pas déterministe. Une graduation est équivalente à environ 100 nanosecondes. La valeur minimale est 1 et la valeur maximale est de 60.

-   **FileHandleCacheSizeinMB**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\FileHandleCacheSizeinMB
    ```

    La valeur par défaut est 4. Ce paramètre spécifie la mémoire maximale à être consommés par les entrées de cache de handle de fichier. La valeur minimale est 1 et la valeur maximale est 1\*1024\*1024\*1024 (1073741824).

-   **LockFileHandleCacheInMemory**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\LockFileHandleCacheInMemory
    ```

    La valeur par défaut est 0. Ce paramètre spécifie si les pages physiques qui sont allouées pour la taille de cache spécifiée par FileHandleCacheSizeInMB sont verrouillées en mémoire. Cette activité permet de définir cette valeur sur 1. Les pages sont verrouillées en mémoire (non paginée sur le disque), ce qui améliore les performances de la résolution des descripteurs de fichiers, mais réduit la mémoire est disponible pour les applications.

-   **MaxIcbNfsReadHandlesCacheSize**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\MaxIcbNfsReadHandlesCacheSize
    ```

    La valeur par défaut est 64. Ce paramètre spécifie le nombre maximal de handles par volume pour le cache de lecture de données. Les entrées de cache de lecture sont créées uniquement sur les systèmes qui ont plus de 1 Go de mémoire. La valeur minimale est 0 et la valeur maximale est 0xFFFFFFFF.

-   **HandleSigningEnabled**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\HandleSigningEnabled
    ```

    La valeur par défaut est 1. Ce paramètre contrôle si les poignées sont octroyées par le serveur de fichiers NFS sont signées par chiffrement. La valeur 0 désactive handle de signature.

-   **RdWrNfsDeferredWritesFlushDelay**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsDeferredWritesFlushDelay
    ```

    La valeur par défaut est 60. Ce paramètre est un délai d’attente de manière réversible qui contrôle la durée de l’écriture de NFS V3 instable de la mise en cache des données. La valeur minimale est 1, et la valeur maximale est égale à 600. Durée de vie réelle est approximativement égale à RdWrNfsDeferredWritesFlushDelay multipliée par RdWrThreadSleepTime.

-   **CacheAddFromCreateAndMkDir**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\CacheAddFromCreateAndMkDir
    ```

    La valeur par défaut est 1 (activé). Ce paramètre contrôle si les descripteurs sont ouverts pendant NFS V2 et V3 créer et MKDIR RPC procédure gestionnaires sont conservés dans le fichier gèrent le cache. Définissez cette valeur sur 0 pour désactiver l’ajout d’entrées dans le cache dans les chemins de code CREATE and MKDIR.

-   **AdditionalDelayedWorkerThreads**

    ```
    HKLM\SYSTEM\CurrentControlSet\Control\SessionManager\Executive\AdditionalDelayedWorkerThreads
    ```

    Augmente le nombre de threads de travail retardé qui sont créés pour la file d’attente de travail spécifié. Retardée des éléments de travail de processus worker threads qui ne sont pas considérées comme critiques et qui peut avoir leur pile mémoire paginée en attendant que les éléments de travail. Un nombre insuffisant de threads réduit la vitesse à laquelle le travail des éléments sont pris en charge ; une valeur qui est trop élevée consomme des ressources système inutilement.

-   **NtfsDisable8dot3NameCreation**

    ```
    HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreation
    ```

    La valeur par défaut dans Windows Server 2012 et Windows Server 2012 R2 est 2. Dans les versions antérieures à Windows Server 2012, la valeur par défaut est 0. Ce paramètre détermine si NTFS génère un nom court dans le 8dot3 convention de nommage (MS-DOS) pour la durée pendant laquelle les noms de fichiers et noms de fichiers qui contiennent des caractères du jeu de caractères étendus. Si la valeur de cette entrée est 0, les fichiers peuvent avoir deux noms : le nom qui spécifie l’utilisateur et le nom court qui génère de NTFS. Si le nom spécifié par l’utilisateur suit la convention d’affectation de noms 8dot3, NTFS ne génère pas un nom court. Valeur 2 signifie que ce paramètre peut être configuré par volume.

    >[!NOTE]
    > Le volume système a 8dot3 activé par défaut. Tous les autres volumes dans Windows Server 2012 et Windows Server 2012 R2 ont 8dot3 désactivée par défaut. Modification de cette valeur ne change pas le contenu d’un fichier, mais il évite la création de l’attribut de nom court pour le fichier, ce qui modifie également la façon dont NTFS affiche et gère le fichier. Pour la plupart des serveurs de fichiers, le paramètre recommandé est de 1 (désactivée).


-   **NtfsDisableLastAccessUpdate**

    ```
    HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisableLastAccessUpdate
    ```

    La valeur par défaut est 1. Ce commutateur système globaux réduit la charge d’e/s disque et des latences en désactivant la mise à jour de l’horodatage pour le dernier accès de fichier ou répertoire.

-   **MaxConcurrentConnectionsPerIp**

    ```
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Rpcxdr\Parameters\MaxConcurrentConnectionsPerIp
    ```

    La valeur par défaut du paramètre MaxConcurrentConnectionsPerIp est 16. Vous pouvez augmenter cette valeur jusqu'à un maximum de 8 192 pour augmenter le nombre de connexions par adresse IP.
