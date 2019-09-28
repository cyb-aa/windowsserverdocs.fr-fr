---
title: Réglage des performances des serveurs de fichiers
description: Réglage des performances pour les serveurs de fichiers exécutant Windows Server
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
author: phstee
ms.author: NedPyle; Danlo; DKruse
ms.date: 4/14/2017
ms.openlocfilehash: 5f772d2333acb2d48bf27168aca42754013dd8be
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370224"
---
# <a name="performance-tuning-for-file-servers"></a>Réglage des performances des serveurs de fichiers

Vous devez sélectionner le matériel approprié pour répondre à la charge attendue du serveur de fichiers, en tenant compte de la charge moyenne, des pics de charge, de la capacité, des plans de croissance et des temps de réponse. Les goulots d’étranglement matériels limitent l’efficacité du paramétrage de logiciels.

## <a name="general-tuning-parameters-for-clients"></a>Paramétrage général des clients

Les paramètres de Registre REG\_DWORD suivants peuvent affecter les performances des ordinateurs clients qui interagissent avec des serveurs de fichiers SMB :

-   **ConnectionCountPerNetworkInterface**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\ConnectionCountPerNetworkInterface
    ```

    S’applique à : Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

    La valeur par défaut est 1. Nous vous recommandons fortement d’utiliser cette valeur. La plage valide est comprise entre 1 et 16. Nombre maximal de connexions par interface à établir avec un serveur pour les interfaces non-RSS.


-   **ConnectionCountPerRssNetworkInterface**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\ConnectionCountPerRssNetworkInterface
    ```

    S’applique à : Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

    La valeur par défaut est 4. Nous vous recommandons fortement d’utiliser cette valeur. La plage valide est comprise entre 1 et 16. Nombre maximal de connexions par interface à établir avec un serveur pour les interfaces RSS.

-   **ConnectionCountPerRdmaNetworkInterface**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\ConnectionCountPerRdmaNetworkInterface
    ```

    S’applique à : Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

    La valeur par défaut est 2. Nous vous recommandons fortement d’utiliser cette valeur. La plage valide est comprise entre 1 et 16. Nombre maximal de connexions par interface à établir avec un serveur pour les interfaces RDMA.

-   **MaximumConnectionCountPerServer**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\MaximumConnectionCountPerServer
    ```

    S’applique à : Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

    La valeur par défaut est de 32, avec une plage valide allant de 1 à 64. Nombre maximal de connexions par interface à établir avec un serveur unique exécutant Windows Server 2012 sur toutes les interfaces.

-   **DormantDirectoryTimeout**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DormantDirectoryTimeout
    ```

    S’applique à : Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

    La valeur par défaut est 600 secondes. Temps maximal pendant lequel les handles de répertoire de serveur sont maintenus ouverts avec des baux de répertoire.

-   **FileInfoCacheLifetime**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileInfoCacheLifetime
    ```

    S’applique à : Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008

    La valeur par défaut est 10 secondes. Période d’expiration du cache des informations de fichiers.

-   **DirectoryCacheLifetime**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DirectoryCacheLifetime
    ```

    S’applique à : Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008

    La valeur par défaut est 10 secondes. Il s’agit du délai d’expiration du cache de répertoire.

    > [!NOTE]
    > Ce paramètre contrôle la mise en cache des métadonnées de répertoire en l'absence de baux de répertoire.
     

-   **DirectoryCacheEntrySizeMax**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DirectoryCacheEntrySizeMax
    ```

    S’applique à : Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008

    La valeur par défaut est 64. Il s’agit de la taille maximale des entrées de cache de répertoire.

-   **FileNotFoundCacheLifetime**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileNotFoundCacheLifetime
    ```

    S’applique à : Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008

    La valeur par défaut est 5 secondes. Période d’expiration du cache des fichiers non trouvés.

-   **CacheFileTimeout**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\CacheFileTimeout
    ```

    S’applique à : Windows 8.1, Windows 8, Windows Server 2012, Windows Server 2012 R2 et Windows 7

    La valeur par défaut est 10 secondes. Ce paramètre contrôle la durée (en secondes) pendant laquelle le redirecteur conserve les données mises en cache d’un fichier une fois que le dernier handle au fichier est fermé par une application.

-   **DisableBandwidthThrottling**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DisableBandwidthThrottling
    ```

    S’applique à : Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008

    La valeur par défaut est 0. Par défaut, le redirecteur SMB limite le débit sur les connexions réseau à latence élevée, dans certains cas afin d’éviter des délais d’attente liés au réseau. Définir cette valeur de Registre sur 1 désactive cette limitation, en activant un débit de transfert de fichiers plus élevé sur des connexions réseau à latence élevée.

-   **DisableLargeMtu**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DisableLargeMtu
    ```

    S’applique à : Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008

    La valeur par défaut est 0 pour Windows 8 uniquement. Dans Windows 8, le redirecteur SMB transfère des charges utiles jusqu’à 1 Mo par demande, ce qui peut améliorer la vitesse de transfert de fichiers. La définition de cette valeur de Registre sur 1 limite à 64 Ko la taille de requête. Vous devez évaluer l’impact de ce paramètre avant de l’appliquer.

-   **RequireSecuritySignature**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\RequireSecuritySignature
    ```

    S’applique à : Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008

    La valeur par défaut est 0 et désactive la signature SMB. Définir cette valeur sur 1 active la signature SMB pour toutes les communications SMB, en empêchant la communication SMB avec des ordinateurs sur lesquels la signature SMB est désactivée. La signature SMB peut augmenter le coût de l’UC et les allers-retours réseau, mais vous aide à bloquer les attaques de type man-in-the-middle. Si la signature SMB n’est pas obligatoire, assurez-vous que cette valeur de Registre est définie sur 0 sur tous les clients et serveurs. 
    
    Pour plus d’informations, consultez l’article sur les [bases de la signature SMB](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/).

-   **FileInfoCacheEntriesMax**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileInfoCacheEntriesMax
    ```

    S’applique à : Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008

    La valeur par défaut est de 64, avec une plage valide allant de 1 à 65 536. Cette valeur permet de déterminer la quantité de métadonnées de fichier pouvant être mises en cache par le client. Augmenter la valeur peut réduire le trafic réseau et améliorer les performances lorsqu’un grand nombre de fichiers est consulté.

-   **DirectoryCacheEntriesMax**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DirectoryCacheEntriesMax
    ```

    S’applique à : Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008

    La valeur par défaut est de 16, avec une plage valide allant de 1 à 4 096. Cette valeur permet de déterminer la quantité d’informations de répertoire pouvant être mises en cache par le client. Augmenter la valeur peut réduire le trafic réseau et améliorer les performances lorsque des répertoires volumineux sont consultés.

-   **FileNotFoundCacheEntriesMax**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileNotFoundCacheEntriesMax
    ```

    S’applique à : Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008

    La valeur par défaut est de 128, avec une plage valide allant de 1 à 65 536. Cette valeur permet de déterminer la quantité d’informations de nom de fichier pouvant être mises en cache par le client. Augmenter la valeur peut réduire le trafic réseau et améliorer les performances lorsqu’un grand nombre de noms de fichiers est consulté.

-   **MaxCmds**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\MaxCmds
    ```

    S’applique à : Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008

    La valeur par défaut est 15. Ce paramètre limite le nombre de demandes en attente sur une session. Augmenter la valeur peut utiliser plus de mémoire, mais peut améliorer les performances en activant un pipeline de demande plus profond. L’augmentation de la valeur en conjonction avec MaxMpxCt peut également éliminer les erreurs qui se produisent en raison du grand nombre de demandes de fichiers à long terme en suspens, par exemple les appels FindFirstChangeNotification. Ce paramètre n’affecte pas les connexions avec les serveurs SMB 2.0.

-   **DormantFileLimit est**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DormantFileLimit
    ```

    S’applique à : Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008

    La valeur par défaut est 1 023. Ce paramètre spécifie le nombre maximal de fichiers devant être laissés ouverts sur une ressource partagée une fois que l’application a fermé le fichier.

### <a name="client-tuning-example"></a>Exemple de paramétrage de client

Les paramètres généraux pour les ordinateurs clients peuvent optimiser un ordinateur pour l’accès aux partages de fichiers distants, en particulier sur certains réseaux à latence élevée (par exemple, les succursales, les communications entre centres de données, les bureaux à domicile et le haut débit mobile). Les paramètres ne sont pas optimaux ni appropriés sur tous les ordinateurs. Vous devez évaluer l’impact de ces paramètres spécifiques avant de les appliquer.

| Paramètre                   | Valeur | Default |
|-----------------------------|-------|---------|
| DisableBandwidthThrottling  | 1     | 0       |
| FileInfoCacheEntriesMax     | 32768 | 64      |
| DirectoryCacheEntriesMax    | 4 096  | 16      |
| FileNotFoundCacheEntriesMax | 32768 | 128     |
| MaxCmds                     | 32768 | 15      |

 

À partir de Windows 8, vous pouvez configurer bon nombre de ces paramètres SMB à l’aide des applets de commande Windows PowerShell **Set-SmbClientConfiguration** et **Set-SmbServerConfiguration**. Les paramètres applicables au Registre uniquement sont également configurés avec Windows PowerShell.

```
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters" RequireSecuritySignature -Value 0 -Force
```
