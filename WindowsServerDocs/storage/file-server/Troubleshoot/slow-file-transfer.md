---
title: Vitesse de transfert des fichiers SMB lente
description: Décrit comment résoudre les problèmes de performances de transfert de fichiers SMB.
author: Deland-Han
manager: dcscontentpm
audience: ITPro
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 0e6c049404f464eba872075a8ef5060b303920c8
ms.sourcegitcommit: 8cf04db0bc44fd98f4321dca334e38c6573fae6c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75654560"
---
# <a name="slow-smb-files-transfer-speed"></a>Vitesse de transfert des fichiers SMB lente

Cet article fournit des procédures de résolution des problèmes pour des vitesses de transfert de fichiers lentes via SMB.

## <a name="large-file-transfer-is-slow"></a>Le transfert de fichiers volumineux est lent

Si vous constatez des transferts lents de fichiers volumineux, envisagez les étapes suivantes :

- Essayez la commande de copie de fichiers pour les e/s non mises en mémoire tampon (**xcopy/j** ou **Robocopy/j**).

- Testez la vitesse de stockage. En effet, les vitesses de copie des fichiers sont limitées par la vitesse de stockage.

- Les copies de fichiers démarrent parfois rapidement, puis ralentissent. Suivez ces instructions pour vérifier cette situation :
    
  - Cela se produit généralement lorsque la copie initiale est mise en cache ou mise en mémoire tampon (soit en mémoire, soit dans le cache mémoire du contrôleur RAID) et que le cache est en cours d’exécution. Cela force l’écriture des données directement sur le disque (écriture directe). Il s’agit d’un processus plus lent.
    
  - Utilisez les compteurs de l’analyseur de performances de stockage pour déterminer si les performances de stockage se dégradent au fil du temps. Pour plus d’informations, consultez [réglage des performances pour les serveurs de fichiers SMB](https://docs.microsoft.com/windows-server/administration/performance-tuning/role/file-server/smb-file-server).

- Utilisez RAMMap (SysInternals) pour déterminer si l’utilisation du « fichier mappé » en mémoire cesse de croître en raison de l’épuisement de la mémoire disponible.

- Recherchez la perte de paquets dans la trace. Cela peut entraîner une limitation par le fournisseur de congestion TCP.

- Pour SMBv3 et versions ultérieures, assurez-vous que SMB Multichannel est activé et opérationnel.

- Sur le client SMB, activez la grande MTU dans SMB et désactivez la limitation de bande passante. Pour ce faire, exécutez la commande suivante :  
  
  ```PowerShell
  Set-SmbClientConfiguration -EnableBandwidthThrottling 0 -EnableLargeMtu 1
  ```

## <a name="small-file-transfer-is-slow"></a>Le transfert de petits fichiers est lent

Le transfert lent des petits fichiers via SMB se produit le plus souvent s’il y a de nombreux fichiers. Ce comportement est normal.

Lors du transfert de fichiers, la création de fichiers entraîne une surcharge de protocole élevée et une surcharge importante du système de fichiers. Pour les transferts de fichiers volumineux, ces coûts ne se produisent qu’une seule fois. Lors du transfert d’un grand nombre de petits fichiers, le coût est répétitif et entraîne des transferts lents.

Vous trouverez ci-dessous des détails techniques sur ce problème :

- SMB appelle une commande de création pour demander que le fichier soit créé. Du code vérifie si le fichier existe, puis crée le fichier. Ou une variante de la commande Create crée le fichier réel.

- Chaque commande CREATE génère une activité sur le système de fichiers.

- Une fois les données écrites, le fichier est fermé.

- Tout le temps, le processus souffre de la latence du réseau et de la latence du serveur SMB. Cela est dû au fait que la requête SMB est d’abord traduite en commande de système de fichiers, puis à la latence réelle du système de fichiers pour terminer l’opération.

- Si un programme antivirus est en cours d’exécution, le transfert ralentit encore davantage. Cela est dû au fait que les données sont généralement analysées une fois par le renifleur de paquets et une deuxième fois lorsqu’elles sont écrites sur le disque. Dans certains scénarios, ces actions sont répétées à des milliers de fois. Vous pouvez observer des vitesses inférieures à 1 Mo/s.

## <a name="opening-office-documents-is-slow"></a>L’ouverture des documents Office est lente

Ce problème se produit généralement sur une connexion WAN. Cela est courant et, en général, est dû à la manière dont les applications Office (Microsoft Excel, en particulier) accèdent et lisent les données.

Nous vous recommandons de vous assurer que les fichiers binaires Office et SMB sont à jour, puis de vérifier que le bail est désactivé sur le serveur SMB. Pour cela, procédez comme suit :
   
1. Exécutez la commande PowerShell suivante dans Windows 8 et Windows Server 2012 ou versions ultérieures de Windows :
      
   ```PowerShell
   Set-SmbServerConfiguration -EnableLeasing $false  
   ```
      
   Ou, exécutez la commande suivante dans une fenêtre d’invite de commandes avec élévation de privilèges :  

   ```cmd
   REG ADD HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\lanmanserver\parameters /v DisableLeasing /t REG\_DWORD /d 1 /f  
   ```
      
   > [!NOTE]
   > Une fois cette clé de Registre définie, les baux SMB2 ne sont plus accordés, mais les oplocks sont toujours disponibles. Ce paramètre est principalement utilisé pour la résolution des problèmes.
    
2. Redémarrez le serveur de fichiers ou redémarrez le service **serveur** . Pour redémarrer le service, exécutez les commandes suivantes :

   ```cmd  
   NET STOP SERVER 
   NET START SERVER
   ```

Pour éviter ce problème, vous pouvez également répliquer le fichier sur un serveur de fichiers local. Pour plus d’informations, consultez les [documents Office AVING sur un serveur réseau sont lents lors de l’utilisation de EFS](https://docs.microsoft.com/office/troubleshoot/office/saving-file-to-network-server-slow).
