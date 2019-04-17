---
title: "Avant d’installer WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8d0893bd-e2b7-4494-9537-02b1cbbcd57a
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e7eb1b7bed780b41f1a87589add4ab015f41624a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="before-you-install-windows-server-essentials"></a>Avant d’installer WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

##  <a name="BKMK_BeforeYouBegin"></a>Avant de commencer votre installation de WindowsServerEssentials, effectuer les tâches suivantes:  

-   **Assurez-vous que votre ordinateur répond à la configuration matérielle minimale requise**. Cela inclut de déterminer si vous avez besoin de matériel supplémentaire et vérifier que les pilotes pour votre matériel sont pris en charge par WindowsServerEssentials. Pour plus d’informations, voir [System Configuration requise pour WindowsServerEssentials](../get-started/system-requirements.md).   

  
    > [!IMPORTANT]
    >  Avant d’installer WindowsServerEssentials sur un ordinateur pré-existant, nous recommandons un formatage complet et puis repartitionner les disques durs de l’ordinateur pré-existant. Par la mise en forme et repartitionner les disques durs, vous supprimez la possibilité que des partitions cachées restent sur les disques durs.  
  
-   **Préparer votre réseau** pour préparer votre réseau pour installer WindowsServerEssentials, procédez comme suit:  
    
  
    -   **Mise à niveau du système d’exploitation sur vos ordinateurs clients** WindowsServerEssentials prend en charge les systèmes d’exploitation suivants: Windows8, Windows7, Windows10 et Macintosh OS X Lion ou version ultérieure. Ces systèmes d’exploitation fournissent des fonctionnalités de sécurité nécessaires, de fiabilité, de performances et de fonctionnalités pour le réseau local.  
  
    -   **Configurer votre routeur** vérifier que votre routeur est configuré comme suit:  
  
        -   L’infrastructure UPnP est activé sur votre routeur.  
  
        -   Le service serveur DHCP Dynamic Host Configuration Protocol () pour le LAN peut être activé ou désactivé.  WindowsServerEssentials permet de s’assurer que DHCP n’est pas en cours d’exécution sur le serveur et le routeur? Lorsque le protocole DHCP est activé sur le routeur, DHCP n’est pas activé sur le serveur lors de l’installation.  
  
        -   Vous avez une adresse IP pour l’interface externe de votre routeur, qui est fournie par votre fournisseur de services Internet (ISP). L’adresse IP peut être affectée dynamiquement par le service serveur DHCP à votre fournisseur de services Internet, ou vous devez configurer manuellement une adresse IP statique à l’aide de la console de gestion du routeur.  
  
        -   Si votre connexion Internet nécessite un nom d’utilisateur et un mot de passe, également appelé protocole PPP sur Ethernet (PPPoE), ces paramètres sont configurés sur votre routeur, même si le périphérique prend en charge l’infrastructure UPnP.  
  
        -   Le routeur est connecté au réseau local et à Internet, il est activé et fonctionne correctement.  
  
     Si votre routeur ne prend pas en charge l’infrastructure UPnP, ou si votre routeur ne peut pas être configuré pendant l’installation, vous devez manuellement le configurer avec les paramètres de votre réseau. Vérifiez que les ports suivants sont ouverts et sont dirigés vers l’adresse IP du serveur de Destination:  
  
    |Numéro de port|Application|  
    |-----------------|-----------------|  
    |Port 80|Trafic HTTP Web|  
    |Port 443|Trafic Web HTTPS|  
  

-   **Lire la documentation de version de WindowsServerEssentials**. La documentation de version contient les dernières informations pouvant être critiques pour l’installation et la configuration de WindowsServerEssentials. Pour afficher ou imprimer la documentation de version, voir [Release Documentation for WindowsServerEssentials](../get-started/release-notes.md).  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Installer WindowsServerEssentials](Install-Windows-Server-Essentials.md)

