---
title: Réglage des performances pour les serveurs de fichiers SMB
description: Réglage des performances pour les serveurs de fichiers SMB
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
author: phstee
ms.author: nedpyle; danlo; dkruse
ms.date: 4/14/2017
ms.openlocfilehash: 89017686801501593c51245d44bf88a6ecf4baf6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851822"
---
# <a name="performance-tuning-for-smb-file-servers"></a>Réglage des performances pour les serveurs de fichiers SMB

## <a name="smb-configuration-considerations"></a>Considérations relatives à la configuration SMB
N’activez pas les services ou fonctionnalités dont votre serveur de fichiers et vos clients n’ont pas besoin. Celles-ci peuvent inclure la signature SMB, la mise en cache côté client, les mini-filtres du système de fichiers, le service de recherche, les tâches planifiées, le chiffrement NTFS, la compression NTFS, IPSEC, les filtres de pare-feu, Teredo et le chiffrement SMB.

Assurez-vous que les modes de gestion de l’alimentation du BIOS et du système d’exploitation sont définis en fonction des besoins, ce qui peut inclure le mode hautes performances ou la modification de l’État C. Assurez-vous que les pilotes de périphérique réseau et de stockage les plus récents, les plus fiables et les plus rapides sont installés.

La copie de fichiers est une opération courante effectuée sur un serveur de fichiers. Windows Server dispose de plusieurs utilitaires de copie de fichier intégrés que vous pouvez exécuter à l’aide d’une invite de commandes. Robocopy est recommandé. Introduite dans Windows Server 2008 R2, l’option **/MT** de Robocopy peut améliorer considérablement la vitesse des transferts de fichiers à distance à l’aide de plusieurs threads lors de la copie de plusieurs petits fichiers. Nous vous recommandons également d’utiliser l’option **/log** pour réduire la sortie de la console en redirigeant les journaux vers un appareil nul ou vers un fichier. Lorsque vous utilisez xcopy, nous vous recommandons d’ajouter les options **/q** et **/k** à vos paramètres existants. La première option réduit la surcharge du processeur en réduisant la sortie de la console et la seconde réduit le trafic réseau.

## <a name="smb-performance-tuning"></a>Réglage des performances SMB


Les performances du serveur de fichiers et les réglages disponibles dépendent du protocole SMB négocié entre chaque client et le serveur, ainsi que des fonctionnalités du serveur de fichiers déployé. La version de protocole la plus récente actuellement disponible est SMB 3.1.1 dans Windows Server 2016 et Windows 10. Vous pouvez vérifier quelle version de SMB est utilisée sur votre réseau à l’aide de Windows PowerShell **SMBConnection** sur les clients et de la fonction **SMBSession | FL** sur les serveurs.

### <a name="smb-30-protocol-family"></a>Famille de protocoles SMB 3,0

SMB 3,0 a été introduit dans Windows Server 2012 et amélioré dans Windows Server 2012 R2 (SMB 3,02) et Windows Server 2016 (SMB 3.1.1). Cette version a introduit des technologies qui peuvent améliorer considérablement les performances et la disponibilité du serveur de fichiers. Pour plus d’informations, consultez [SMB dans Windows Server 2012 et 2012 R2 2012](https://aka.ms/smb3plus) et [Nouveautés de SMB 3.1.1](https://aka.ms/smb311).



### <a name="smb-direct"></a>SMB Direct

SMB direct a introduit la possibilité d’utiliser des interfaces réseau RDMA pour un débit élevé avec une faible latence et une faible utilisation du processeur.

Chaque fois que SMB détecte un réseau compatible RDMA, il tente automatiquement d’utiliser la fonctionnalité RDMA. Toutefois, si pour une raison quelconque, le client SMB ne parvient pas à se connecter à l’aide du chemin d’accès RDMA, il continue à utiliser des connexions TCP/IP à la place. Toutes les interfaces RDMA compatibles avec SMB direct sont requises pour implémenter également une pile TCP/IP, et SMB Multichannel en est conscient.

SMB direct n’est requis dans aucune configuration SMB, mais il est toujours recommandé pour ceux qui souhaitent une latence inférieure et une utilisation moindre du processeur.

Pour plus d’informations sur SMB direct, consultez [améliorer les performances d’un serveur de fichiers avec SMB direct](https://aka.ms/smbdirect).

### <a name="smb-multichannel"></a>SMB Multichannel

SMB Multichannel permet aux serveurs de fichiers d’utiliser plusieurs connexions réseau simultanément et fournit un débit accru.

Pour plus d’informations sur SMB Multichannel, consultez [déployer SMB Multichannel](https://aka.ms/smbmulti).

### <a name="smb-scale-out"></a>Montée en charge SMB

La montée en charge SMB permet à SMB 3,0 dans une configuration de cluster d’afficher un partage dans tous les nœuds d’un cluster. Cette configuration active/active permet de mettre à l’échelle les clusters de serveurs de fichiers davantage, sans une configuration complexe avec plusieurs volumes, des partages et des ressources de cluster. La bande passante de partage maximale correspond à la bande passante totale de tous les nœuds de cluster de serveurs de fichiers. La bande passante totale n’est plus limitée par la bande passante d’un seul nœud de cluster, mais dépend de la capacité du système de stockage de sauvegarde. Vous pouvez augmenter la bande passante totale en ajoutant des nœuds.

Pour plus d’informations sur la montée en charge SMB, consultez [serveur de fichiers avec montée en puissance parallèle pour plus d'](https://technet.microsoft.com/library/hh831349.aspx) informations sur les données d’application et le billet [de blog pour monter en charge ou non pour monter en charge, c’est la question](https://blogs.technet.com/b/filecab/archive/2013/12/05/to-scale-out-or-not-to-scale-out-that-is-the-question.aspx).

### <a name="performance-counters-for-smb-30"></a>Compteurs de performances pour SMB 3,0

Les compteurs de performances SMB suivants ont été introduits dans Windows Server 2012 et sont considérés comme un ensemble de base de compteurs lorsque vous surveillez l’utilisation des ressources de SMB 2 et versions ultérieures. Consignez les compteurs de performances dans un journal de compteur de performances local (. BLG). Il est moins coûteux de collecter toutes les instances à l’aide du caractère générique (\*), puis d’extraire des instances particulières pendant le traitement après le traitement à l’aide de relog. exe.

-   **Partages clients SMB**

    Ces compteurs affichent des informations sur les partages de fichiers sur le serveur qui sont accessibles par un client qui utilise SMB 2,0 ou des versions ultérieures.

    Si vous êtes familiarisé avec les compteurs de disque standard dans Windows, vous remarquerez peut-être une certaine ressemblance. Ce n’est pas par accident. Le client SMB partage des compteurs de performances qui ont été conçus pour correspondre exactement aux compteurs de disque. De cette façon, vous pouvez facilement réutiliser n’importe quel guide sur le réglage des performances du disque de l’application. Pour plus d’informations sur le mappage des compteurs, consultez le blog sur les [compteurs de performances du client par partage](https://blogs.technet.com/b/josebda/archive/2012/11/19/windows-server-2012-file-server-tip-new-per-share-smb-client-performance-counters-provide-great-insight.aspx).

-   **Partages de serveur SMB**

    Ces compteurs affichent des informations sur les partages de fichiers SMB 2,0 ou version ultérieure sur le serveur.

-   **Sessions de serveur SMB**

    Ces compteurs affichent des informations sur les sessions de serveur SMB qui utilisent SMB 2,0 ou une version ultérieure.

    L’activation des compteurs sur le côté serveur (partages de serveur ou sessions serveur) peut avoir un impact significatif sur les performances pour les charges de travail d’e/s élevées.

-   **Reprendre le filtre de clé**

    Ces compteurs affichent des informations sur le filtre de clé de reprise.

-   **Connexion SMB direct**

    Ces compteurs mesurent différents aspects de l’activité de connexion. Un ordinateur peut avoir plusieurs connexions directes SMB. Les compteurs de connexion SMB direct représentent chaque connexion comme une paire d’adresses IP et de ports, où la première adresse IP et le port représentent le point de terminaison local de la connexion, tandis que la deuxième adresse IP et le port représentent le point de terminaison distant de la connexion.

-   **Relations des compteurs de performance des disques physiques, SMB et CSV FS**

    Pour plus d’informations sur la façon dont les compteurs de disque physique, de SMB et de système de fichiers CSV sont liés, consultez le billet de blog suivant : [volume partagé de cluster des compteurs de performances](https://blogs.msdn.com/b/clustering/archive/2014/06/05/10531462.aspx).

## <a name="tuning-parameters-for-smb-file-servers"></a>Paramétrage des paramètres pour les serveurs de fichiers SMB


Les paramètres de Registre REG\_DWORD suivants peuvent affecter les performances des serveurs de fichiers SMB :

- **Smb2CreditsMin** et **Smb2CreditsMax**

  ```
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\Smb2CreditsMin
  ```

  ```
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\Smb2CreditsMax
  ```

  Les valeurs par défaut sont 512 et 8192, respectivement. Ces paramètres permettent au serveur de limiter de manière dynamique la concurrence des opérations du client dans les limites spécifiées. Certains clients peuvent augmenter le débit avec des limites de concurrence plus élevées, par exemple, en copiant des fichiers sur des liaisons à bande passante élevée et à latence élevée.
    
  > [!TIP]
  > Avant Windows 10 et Windows Server 2016, le nombre de crédits accordés au client variait de façon dynamique entre Smb2CreditsMin et Smb2CreditsMax en fonction d’un algorithme qui tentait de déterminer le nombre optimal de crédits à accorder en fonction de la latence du réseau et de l’utilisation du crédit. Dans Windows 10 et Windows Server 2016, le serveur SMB a été remplacé par le fait d’accorder sans condition des crédits à la demande jusqu’au nombre maximal de crédits configuré. Dans le cadre de cette modification, le mécanisme de limitation des crédits, qui réduit la taille de la fenêtre de crédit de chaque connexion lorsque le serveur est soumis à une sollicitation de la mémoire, a été supprimé. L’événement de mémoire insuffisante du noyau qui a déclenché la limitation est signalé uniquement lorsque la mémoire du serveur est trop faible (< quelques Mo) comme étant inutile. Étant donné que le serveur ne réduit plus les fenêtres de crédit, le paramètre Smb2CreditsMin n’est plus nécessaire et est maintenant ignoré.
  > 
  > Vous pouvez surveiller les partages de client SMB\\les blocages de crédit pour voir s’il existe des problèmes avec les crédits.

- **AdditionalCriticalWorkerThreads**

    ```
    HKLM\System\CurrentControlSet\Control\Session Manager\Executive\AdditionalCriticalWorkerThreads
    ```

    La valeur par défaut est 0, ce qui signifie qu’aucun thread de travail noyau critique supplémentaire n’est ajouté. Cette valeur affecte le nombre de threads utilisés par le cache du système de fichiers pour les demandes d’écriture anticipée et de lecture anticipée. L’augmentation de cette valeur peut permettre d’obtenir plus d’e/s en file d’attente dans le sous-système de stockage, et elle peut améliorer les performances d’e/s, en particulier sur les systèmes avec de nombreux processeurs logiques et un matériel de stockage puissant.

    >[!TIP]
    > La valeur doit peut-être être augmentée si la quantité de données modifiées du gestionnaire de cache (cache de compteur de performance\\pages de modifications) augmente pour consommer une grande partie (plus de ~ 25%). de mémoire ou si le système exécute un grand nombre d’e/s de lecture synchrones.

- **MaxThreadsPerQueue**

  ```
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\MaxThreadsPerQueue
  ```

  La valeur par défaut est 20. L’augmentation de cette valeur augmente le nombre de threads que le serveur de fichiers peut utiliser pour traiter les demandes simultanées. Lorsqu’un grand nombre de connexions actives doivent être desservies et que les ressources matérielles, telles que la bande passante de stockage, sont suffisantes, l’augmentation de la valeur peut améliorer l’évolutivité, les performances et les temps de réponse du serveur.

  >[!TIP]
  > Une indication que la valeur devra peut-être être augmentée est si les files d’attente de travail SMB2 deviennent de plus en plus volumineuses (les files d’attente de travail du serveur du compteur de performances\\longueur de la file d’attente\\SMB2 \*non bloquant» se trouvent toujours au-dessus de ~ 100).

  >[!Note]
  >Dans Windows 10 et Windows Server 2016, MaxThreadsPerQueue n’est pas disponible. Le nombre de threads pour un pool de threads sera « 20 * le nombre de processeurs dans un nœud NUMA ».
     

- **AsynchronousCredits**

  ``` 
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\AsynchronousCredits
  ```

  La valeur par défaut est 512. Ce paramètre limite le nombre de commandes SMB asynchrones simultanées autorisées sur une seule connexion. Certains cas (par exemple, lorsqu’il existe un serveur frontal avec un serveur IIS principal) requièrent une grande quantité d’accès concurrentiel (pour les demandes de notification de modification de fichier, en particulier). La valeur de cette entrée peut être augmentée pour prendre en charge ces cas.

### <a name="smb-server-tuning-example"></a>Exemple de paramétrage de serveur SMB

Les paramètres suivants peuvent optimiser un ordinateur pour les performances des serveurs de fichiers dans de nombreux cas. Les paramètres ne sont pas optimaux ni appropriés sur tous les ordinateurs. Vous devez évaluer l’impact de ces paramètres spécifiques avant de les appliquer.

| Paramètre                       | Valeur | Default |
|---------------------------------|-------|---------|
| AdditionalCriticalWorkerThreads | 64    | 0       |
| MaxThreadsPerQueue              | 64    | 20      |


### <a name="smb-client-performance-monitor-counters"></a>Compteurs de l’analyseur de performances des clients SMB

Pour plus d’informations sur les compteurs du client SMB, consultez [Astuce du serveur de fichiers Windows server 2012 : les nouveaux compteurs de performances de client SMB par partage fournissent](https://blogs.technet.com/b/josebda/archive/2012/11/19/windows-server-2012-file-server-tip-new-per-share-smb-client-performance-counters-provide-great-insight.aspx)des informations intéressantes.
