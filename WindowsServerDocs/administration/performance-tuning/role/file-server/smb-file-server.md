---
title: Réglage des performances pour les serveurs de fichiers SMB
description: Réglage des performances pour les serveurs de fichiers SMB
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
author: phstee
ms.author: NedPyle; Danlo; DKruse
ms.date: 4/14/2017
ms.openlocfilehash: 337716792a4bb3cf730b723df3abe1029631426b
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222501"
---
# <a name="performance-tuning-for-smb-file-servers"></a>Réglage des performances pour les serveurs de fichiers SMB

## <a name="smb-configuration-considerations"></a>Considérations relatives à la configuration SMB
N’activez pas tous les services ou fonctionnalités qui ne nécessitent pas votre serveur de fichiers et les clients. Il peut s’agir de la signature SMB, la mise en cache côté client, mini-filtres de système de fichiers, service de recherche, les tâches planifiées, chiffrement NTFS, la compression NTFS, IPSEC, les filtres de pare-feu, Teredo et SMB chiffrement.

Assurez-vous que le BIOS et les modes de gestion d’alimentation de système d’exploitation sont définies en fonction des besoins, ce qui peut inclure le mode hautes performances ou modifié C-State. Vérifiez que le stockage plus récente, plus robuste et plus rapide et des pilotes de périphériques réseau sont installés.

Copie de fichiers est une opération courante effectuée sur un serveur de fichiers. Windows Server a plusieurs utilitaires de copie de fichier intégrées que vous pouvez exécuter à l’aide d’une invite de commandes. Robocopy est recommandé. Introduite dans Windows Server 2008 R2, le **/MT** option de Robocopy peut améliorer considérablement la vitesse sur les transferts de fichiers à distance à l’aide de plusieurs threads lors de la copie de plusieurs petits fichiers. Nous vous recommandons également d’utiliser le **/log** option pour réduire la sortie de la console en redirigeant les journaux sur un appareil NUL ou dans un fichier. Lorsque vous utilisez Xcopy, nous vous recommandons d’ajouter le **/q** et **/k** options à vos paramètres existants. La première option réduit les frais de processeur en réduisant la sortie de la console, et cette dernière réduit le trafic réseau.

## <a name="smb-performance-tuning"></a>Réglage des performances de SMB


Performances du serveur de fichiers et des réglages disponibles selon le protocole SMB est négocié entre chaque client et le serveur et sur les fonctionnalités de serveur de fichier déployé. La version du protocole le plus élevée actuellement disponible est SMB 3.1.1 dans Windows Server 2016 et Windows 10. Vous pouvez vérifier la version de SMB est en cours d’utilisation sur votre réseau à l’aide de Windows PowerShell **Get-SMBConnection** sur les clients et **Get-SMBSession | FL** sur les serveurs.

### <a name="smb-30-protocol-family"></a>Famille de protocoles SMB 3.0

SMB 3.0 a été introduit dans Windows Server 2012 et encore amélioré dans Windows Server 2012 R2 (3.02 SMB) et Windows Server 2016 (SMB 3.1.1). Cette version introduit des technologies qui peuvent améliorer considérablement les performances et la disponibilité du serveur de fichiers. Pour plus d’informations, consultez [SMB dans Windows Server 2012 et 2012 R2 2012](https://aka.ms/smb3plus) et [quelles sont les nouveautés dans SMB 3.1.1](https://aka.ms/smb311).



### <a name="smb-direct"></a>SMB Direct

SMB Direct a introduit la possibilité d’utiliser les interfaces de réseau RDMA pour un débit élevé à faible latence et de faible utilisation du processeur.

Chaque fois que SMB détecte un réseau prenant en charge RDMA, il essaie automatiquement d’utiliser la fonction RDMA. Toutefois, si pour une raison quelconque, le client SMB ne parvient pas à se connecter en utilisant le chemin d’accès RDMA, il sera simplement continuer à utiliser les connexions TCP/IP à la place. Toutes les interfaces RDMA qui sont compatibles avec SMB Direct sont tenues d’implémenter une pile TCP/IP, et SMB Multichannel est informée.

SMB Direct n’est pas obligatoire dans n’importe quelle configuration SMB, mais il ' s toujours recommandée pour ceux qui souhaitent une latence plus faible et l’utilisation du processeur inférieure.

Pour plus d’informations sur SMB Direct, consultez [améliorer les performances d’un serveur de fichiers avec SMB Direct](https://aka.ms/smbdirect).

### <a name="smb-multichannel"></a>SMB Multichannel

SMB Multichannel permet aux serveurs de fichiers à utiliser plusieurs connexions réseau simultanément et fournit un débit plus élevé.

Pour plus d’informations sur SMB Multichannel, consultez [déployer SMB Multichannel](https://aka.ms/smbmulti).

### <a name="smb-scale-out"></a>SMB Scale-Out

SMB Scale-out permet de SMB 3.0 dans une configuration de cluster pour afficher un partage dans tous les nœuds d’un cluster. Cette configuration actif/actif rend possible pour les clusters de serveur des fichiers de mise à l’échelle de plus, sans une configuration complexe comportant plusieurs volumes, partages et les ressources de cluster. La bande passante maximale de partage est la bande passante totale de tous les nœuds de cluster de serveurs de fichiers. La bande passante totale n’est plus limitée par la bande passante d’un seul nœud du cluster, mais au lieu de cela dépend de la capacité du système de stockage de sauvegarde. Vous pouvez augmenter la bande passante totale en ajoutant des nœuds.

Pour plus d’informations sur SMB montée en puissance, consultez [Scale-Out File Server pour une vue d’ensemble des données Application](https://technet.microsoft.com/library/hh831349.aspx) et le billet de blog [pour monter en charge ou pas monter en puissance, là est la question](http://blogs.technet.com/b/filecab/archive/2013/12/05/to-scale-out-or-not-to-scale-out-that-is-the-question.aspx).

### <a name="performance-counters-for-smb-30"></a>Compteurs de performances de SMB 3.0

Les compteurs de performances SMB suivants ont été introduits dans Windows Server 2012, et ils sont considérés comme un ensemble de compteurs de base lorsque vous surveillez l’utilisation des ressources de SMB 2 et versions ultérieures. Journaliser les compteurs de performances à une variable locale, journal de compteur de performances brutes (.blg). Il est plus économique collecter toutes les instances en utilisant le caractère générique (\*), puis extrayez des instances au cours du post-traitement à l’aide de Relog.exe.

-   **Partages de Client SMB**

    Ces compteurs affichent des informations sur les partages de fichiers sur le serveur qui sont ouverts par un client qui est à l’aide de SMB 2.0 ou versions ultérieures.

    Si vous « connaissance avec les compteurs de disque standard de Windows, vous pouvez remarquer un certain ressemblance. Que ' s pas par accident. Les compteurs de performances de partages SMB client ont été conçus pour correspondre exactement les compteurs de disque. De cette façon, vous pouvez facilement réutiliser tout obtenir des conseils sur le réglage des performances de disque d’application est actuellement. Pour plus d’informations sur le mappage de compteur, consultez [par blog des compteurs de performances de partage de client](http://blogs.technet.com/b/josebda/archive/2012/11/19/windows-server-2012-file-server-tip-new-per-share-smb-client-performance-counters-provide-great-insight.aspx).

-   **Partages de serveur SMB**

    Ces compteurs affichent des informations sur le SMB 2.0 ou supérieur de partages de fichiers sur le serveur.

-   **Sessions SMB Server**

    Ces compteurs affichent des informations sur les sessions de serveur SMB qui sont à l’aide de SMB 2.0 ou version ultérieure.

    Tension compteurs côté serveur (partages de serveur ou les sessions de serveur) peut affecter considérablement les performances pour les charges de travail d’e/s élevées.

-   **Reprendre le filtre de clé**

    Ces compteurs affichent des informations sur le filtre de clé de reprise.

-   **Connexion directe de SMB**

    Ces compteurs mesurent différents aspects de l’activité de connexion. Un ordinateur peut avoir plusieurs connexions SMB Direct. Les compteurs de connexion directe SMB représentent chaque connexion en tant que paire d’adresses IP et ports, sachant que la première adresse IP et port représentent le point de terminaison local de la connexion, et la deuxième adresse IP et port représentent le point de terminaison de la connexion à distance.

-   **Disque physique, SMB, les relations de compteurs de performances de volume partagé de cluster FS**

    Pour plus d’informations sur la relation entre des compteurs de disque physique, SMB et CSV FS (système de fichiers), consultez le blog suivant : [Compteurs de performances de Volume partagé de cluster](http://blogs.msdn.com/b/clustering/archive/2014/06/05/10531462.aspx).

## <a name="tuning-parameters-for-smb-file-servers"></a>Paramètres de réglage pour les serveurs de fichiers SMB


Le REG suivant\_les paramètres de Registre DWORD peuvent affecter les performances des serveurs de fichiers SMB :

-   **Smb2CreditsMin** et **Smb2CreditsMax**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\Smb2CreditsMin
    ```

    ```
    HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\Smb2CreditsMax
    ```

    Les valeurs par défaut sont 512 et 8192, respectivement. Ces paramètres permettent au serveur limiter la simultanéité d’opération de client dynamiquement dans les limites spécifiées. Certains clients peuvent obtenir un débit plus élevé avec les limites de concurrence plus élevées, par exemple, la copie de fichiers sur des liaisons à large bande passante, latence élevée.
    
    >[!TIP]
    > Avant Windows 10 et Windows Server 2016, le nombre de crédits octroyées au client variait dynamiquement Smb2CreditsMin et Smb2CreditsMax selon un algorithme qui a tenté de déterminer le nombre optimal de crédits pour accorder en fonction de la latence du réseau et l’utilisation de crédit. Dans Windows 10 et Windows Server 2016, le serveur SMB a été modifié pour accorder des crédits à la demande jusqu’au nombre maximal configuré de crédits sans condition. Dans le cadre de cette modification, le crédit mécanisme, ce qui réduit la taille de fenêtre de crédit de chaque connexion lorsque le serveur est soumis à une sollicitation de la mémoire, de limitation a été supprimé. Événement d’insuffisance de mémoire du noyau qui a déclenché la limitation est signalé uniquement lorsque le serveur est par conséquent, un mémoire insuffisante (< quelques Mo) à être inutile. Étant donné que le serveur n’est plus réduit que crédit windows le paramètre Smb2CreditsMin n’est plus nécessaire et est désormais ignoré.

    > Vous pouvez surveiller les partages de clients SMB\\crédit entrave/s pour voir s’il existe des problèmes avec des crédits.

- **AdditionalCriticalWorkerThreads**

    ```
    HKLM\System\CurrentControlSet\Control\Session Manager\Executive\AdditionalCriticalWorkerThreads
    ```

    La valeur par défaut est 0, ce qui signifie qu’aucun thread de travail de noyau critique supplémentaires n’est ajoutés. Cette valeur affecte le nombre de threads utilisés par le cache du système de fichiers pour les demandes de lecture anticipée et d’écriture. Augmenter cette valeur permet l’utilisation de plusieurs en attente d’e/s du sous-système de stockage, et elle peut améliorer les performances d’e/s, en particulier sur les systèmes avec un grand nombre de processeurs logiques et de matériel de stockage puissant.

    >[!TIP]
    > La valeur peut-être être augmentée si la quantité de gestionnaire de cache incorrectes des données (compteur de performance du Cache\\Pages incorrectes) augmente pour consommer une grande partie (plus environ 25 %) de mémoire ou si le système effectue un grand nombre de synchrone e/s de lecture.

-   **MaxThreadsPerQueue**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\MaxThreadsPerQueue
    ```

    La valeur par défaut est 20. Augmentation de cette valeur déclenche le nombre de threads que le serveur de fichiers peut utiliser pour traiter les demandes simultanées. Quand un grand nombre de connexions actives a besoin d’être traitées et les ressources matérielles, telles que la bande passante de stockage, sont suffisantes, augmenter la valeur peut améliorer l’évolutivité du serveur, les performances et les temps de réponse.

    >[!TIP]
    > Indique la valeur devant être augmentée est si les files d’attente de travail SMB2 progressent très volumineux (compteur de performances « serveur files d’attente\\longueur de file d’attente\\SMB2 non bloquant \*' est supérieure à environ 100).

    >[!Note]
    >Dans Windows 10 et Windows Server 2016, MaxThreadsPerQueue n’est pas disponible. Le nombre de threads pour un pool de threads sera « 20 * le nombre de processeurs dans un nœud NUMA ».  

-   **AsynchronousCredits**

    ``` 
    HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\AsynchronousCredits
    ```

    La valeur par défaut est 512. Ce paramètre limite le nombre de commandes SMB asynchrones simultanées sont autorisées sur une seule connexion. Certains cas (par exemple quand il existe un serveur frontal avec un serveur IIS de back-end) nécessitent une grande quantité d’accès concurrentiel (pour le fichier de demandes de modification notification, en particulier). La valeur de cette entrée peut être augmentée pour prendre en charge de ces cas.

### <a name="smb-server-tuning-example"></a>Exemple de paramétrage de serveur SMB

Les paramètres suivants peuvent optimiser un ordinateur pour des performances de serveur de fichiers dans de nombreux cas. Les paramètres ne sont pas optimaux ni appropriés sur tous les ordinateurs. Vous devez évaluer l’impact de ces paramètres spécifiques avant de les appliquer.

| Paramètre                       | Value | Par défaut |
|---------------------------------|-------|---------|
| AdditionalCriticalWorkerThreads | 64    | 0       |
| MaxThreadsPerQueue              | 64    | 20      |


### <a name="smb-client-performance-monitor-counters"></a>Compteurs Analyseur de performances de client SMB

Pour plus d’informations sur les compteurs de client SMB, consultez [Conseil de Windows Server 2012 fichier serveur : Nouveaux compteurs de performances client par partage SMB fournissent une meilleure visibilité](http://blogs.technet.com/b/josebda/archive/2012/11/19/windows-server-2012-file-server-tip-new-per-share-smb-client-performance-counters-provide-great-insight.aspx).
