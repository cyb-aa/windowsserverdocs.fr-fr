---
title: Réglage des performances pour les serveurs de fichiers NFS
description: Réglage des performances pour les serveurs de fichiers NFS
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
author: phstee
ms.author: roopeshb, nedpyle
ms.date: 10/16/2017
ms.openlocfilehash: 9bee396532c3319e43d10012e098533495cf0b03
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851852"
---
# <a name="performance-tuning-nfs-file-servers"></a>Réglage des performances pour les serveurs de fichiers NFS

## <a name="services-for-nfs-model"></a><a href="" id="servicesnfs"></a>Modèle des services pour NFS


Les sections suivantes fournissent des informations sur le modèle NFS (Microsoft Services for Network File System) pour la communication client-serveur. Étant donné que NFS v2 et NFS v3 sont toujours les versions les plus largement déployées du protocole, toutes les clés de Registre, à l’exception de MaxConcurrentConnectionsPerIp, s’appliquent uniquement à NFS v2 et NFS v3.

Aucun réglage du Registre n’est nécessaire pour le protocole NFS v 4.1.

### <a name="service-for-nfs-model-overview"></a>Vue d’ensemble du modèle service for NFS

Microsoft Services for NFS fournit une solution de partage de fichiers pour les entreprises disposant d’un environnement mixte Windows et UNIX. Ce modèle de communication se compose d’ordinateurs clients et d’un serveur. Les applications sur le client demandent des fichiers qui se trouvent sur le serveur via le redirecteur (Rdbss. sys) et le mini-redirecteur NFS (Nfsrdr. sys). Le mini-redirecteur utilise le protocole NFS pour envoyer sa demande via TCP/IP. Le serveur reçoit plusieurs demandes des clients via TCP/IP et achemine les demandes vers le système de fichiers local (NTFS. sys), qui accède à la pile de stockage.

L’illustration suivante montre le modèle de communication pour NFS.

![modèle de communication NFS](../../media/perftune-guide-nfs-model.png)

### <a name="tuning-parameters-for-nfs-file-servers"></a>Paramétrage des paramètres pour les serveurs de fichiers NFS

Les paramètres de Registre REG\_DWORD suivants peuvent affecter les performances des serveurs de fichiers NFS :

-   **OptimalReads**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\OptimalReads
    ```

    La valeur par défaut est 0. Ce paramètre détermine si les fichiers sont ouverts pour un accès de fichier\_aléatoire\_ou pour un fichier\_séquentiel\_uniquement, en fonction des caractéristiques d’e/s de la charge de travail. Définissez cette valeur sur 1 pour forcer l’ouverture des fichiers pour le fichier\_un accès\_aléatoire. Le fichier\_RANDOM\_ACCESS empêche le système de fichiers et le gestionnaire de cache de prérécupérer.

    >[!NOTE]
    > Ce paramètre doit être évalué avec soin, car il peut avoir un impact potentiel sur la croissance du cache des fichiers système.


-   **RdWrHandleLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrHandleLifeTime
    ```

    La valeur par défaut est 5. Ce paramètre contrôle la durée de vie d’une entrée de cache NFS dans le cache de handles de fichiers. Le paramètre fait référence aux entrées de cache qui ont un handle de fichier NTFS ouvert associé. La durée de vie réelle est approximativement égale à RdWrHandleLifeTime multipliée par RdWrThreadSleepTime. La valeur minimale est 1 et la valeur maximale est 60.

-   **RdWrNfsHandleLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsHandleLifeTime
    ```

    La valeur par défaut est 5. Ce paramètre contrôle la durée de vie d’une entrée de cache NFS dans le cache de handles de fichiers. Le paramètre fait référence aux entrées de cache qui n’ont pas de descripteur de fichier NTFS ouvert associé. Les services pour NFS utilisent ces entrées de cache pour stocker les attributs de fichier d’un fichier sans conserver un handle ouvert avec le système de fichiers. La durée de vie réelle est approximativement égale à RdWrNfsHandleLifeTime multipliée par RdWrThreadSleepTime. La valeur minimale est 1 et la valeur maximale est 60.

-   **RdWrNfsReadHandlesLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsReadHandlesLifeTime
    ```

    La valeur par défaut est 5. Ce paramètre contrôle la durée de vie d’une entrée de cache de lecture NFS dans le cache de handles de fichiers. La durée de vie réelle est approximativement égale à RdWrNfsReadHandlesLifeTime multipliée par RdWrThreadSleepTime. La valeur minimale est 1 et la valeur maximale est 60.

-   **RdWrThreadSleepTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrThreadSleepTime
    ```

    La valeur par défaut est 5. Ce paramètre contrôle l’intervalle d’attente avant d’exécuter le thread de nettoyage sur le cache de handles de fichiers. La valeur est en graduations et n’est pas déterministe. Un battement est équivalent à environ 100 nanosecondes. La valeur minimale est 1 et la valeur maximale est 60.

-   **FileHandleCacheSizeinMB**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\FileHandleCacheSizeinMB
    ```

    La valeur par défaut est 4. Ce paramètre spécifie la mémoire maximale à consommer par les entrées du cache des handles de fichiers. La valeur minimale est 1 et la valeur maximale est 1\*1024\*1024\*1024 (1073741824).

-   **LockFileHandleCacheInMemory**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\LockFileHandleCacheInMemory
    ```

    La valeur par défaut est 0. Ce paramètre spécifie si les pages physiques allouées pour la taille de cache spécifiée par FileHandleCacheSizeInMB sont verrouillées en mémoire. Si cette valeur est définie sur 1, cette activité est active. Les pages sont verrouillées en mémoire (non paginées sur le disque), ce qui améliore les performances de résolution des handles de fichiers, mais réduit la mémoire disponible pour les applications.

-   **MaxIcbNfsReadHandlesCacheSize**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\MaxIcbNfsReadHandlesCacheSize
    ```

    La valeur par défaut est 64. Ce paramètre spécifie le nombre maximal de descripteurs par volume pour le cache de données en lecture. Les entrées de cache de lecture sont créées uniquement sur les systèmes qui ont plus de 1 Go de mémoire. La valeur minimale est 0 et la valeur maximale est 0xFFFFFFFF.

-   **HandleSigningEnabled**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\HandleSigningEnabled
    ```

    La valeur par défaut est 1. Ce paramètre contrôle si les descripteurs fournis par le serveur de fichiers NFS sont signés par chiffrement. Si la valeur 0 est désactivée, la signature de handle est désactivée.

-   **RdWrNfsDeferredWritesFlushDelay**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsDeferredWritesFlushDelay
    ```

    La valeur par défaut est 60. Ce paramètre est un délai d’attente logiciel qui contrôle la durée de la mise en cache des données d’écriture instables NFS v3. La valeur minimale est 1 et la valeur maximale est 600. La durée de vie réelle est approximativement égale à RdWrNfsDeferredWritesFlushDelay multipliée par RdWrThreadSleepTime.

-   **CacheAddFromCreateAndMkDir**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\CacheAddFromCreateAndMkDir
    ```

    La valeur par défaut est 1 (activé). Ce paramètre contrôle si les descripteurs qui sont ouverts pendant les gestionnaires de procédures RPC et de création et MKDIR de NFS v2 sont conservés dans le cache de handles de fichiers. Définissez cette valeur sur 0 pour désactiver l’ajout d’entrées au cache dans les chemins de code CREATe et MKDIR.

-   **AdditionalDelayedWorkerThreads**

    ```
    HKLM\SYSTEM\CurrentControlSet\Control\SessionManager\Executive\AdditionalDelayedWorkerThreads
    ```

    Augmente le nombre de threads de travail retardés qui sont créés pour la file d’attente de travail spécifiée. Les threads de travail retardés traitent les éléments de travail qui ne sont pas considérés comme critiques et qui peuvent avoir leur pile de mémoire paginée pendant l’attente d’éléments de travail. Un nombre insuffisant de threads réduit la vitesse à laquelle les éléments de travail sont desservis ; une valeur trop élevée consomme inutilement des ressources système.

-   **NtfsDisable8dot3NameCreation**

    ```
    HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreation
    ```

    La valeur par défaut dans Windows Server 2012 et Windows Server 2012 R2 est 2. Dans les versions antérieures à Windows Server 2012, la valeur par défaut est 0. Ce paramètre détermine si NTFS génère un nom abrégé dans la Convention de nommage 8dot3 (MSDOS) pour les noms de fichiers longs et pour les noms de fichiers qui contiennent des caractères du jeu de caractères étendus. Si la valeur de cette entrée est 0, les fichiers peuvent avoir deux noms : le nom que l’utilisateur spécifie et le nom abrégé généré par NTFS. Si le nom spécifié par l’utilisateur respecte la Convention d’affectation de noms 8dot3, NTFS ne génère pas de nom abrégé. La valeur 2 signifie que ce paramètre peut être configuré par volume.

    >[!NOTE]
    > Par défaut, 8dot3 est activé sur le volume système. Par défaut, 8dot3 est désactivé pour tous les autres volumes de Windows Server 2012 et Windows Server 2012 R2. La modification de cette valeur ne modifie pas le contenu d’un fichier, mais elle évite la création de l’attribut de nom abrégé pour le fichier, ce qui modifie également le mode d’affichage et de gestion du fichier par NTFS. Pour la plupart des serveurs de fichiers, le paramètre recommandé est 1 (désactivé).


-   **NtfsDisableLastAccessUpdate**

    ```
    HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisableLastAccessUpdate
    ```

    La valeur par défaut est 1. Ce commutateur système réduit la charge et les latences d’e/s du disque en désactivant la mise à jour de la date et de l’heure du dernier accès au fichier ou au répertoire.

-   **MaxConcurrentConnectionsPerIp**

    ```
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Rpcxdr\Parameters\MaxConcurrentConnectionsPerIp
    ```

    La valeur par défaut du paramètre MaxConcurrentConnectionsPerIp est 16. Vous pouvez augmenter cette valeur jusqu’à un maximum de 8192 pour augmenter le nombre de connexions par adresse IP.
