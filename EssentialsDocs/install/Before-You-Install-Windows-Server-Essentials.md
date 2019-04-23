---
title: Avant d'installer Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828720"
---
# <a name="before-you-install-windows-server-essentials"></a>Avant d'installer Windows Server Essentials

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_BeforeYouBegin"></a> Avant de commencer votre installation de Windows Server Essentials, procédez comme suit :  

-   **Assurez-vous que votre ordinateur répond aux exigences matérielles minimum**. Cela inclut la détermination de si vous avez besoin de matériel supplémentaire et vérifier que les pilotes pour votre matériel sont pris en charge par Windows Server Essentials. Pour plus d’informations, consultez [System Configuration requise pour Windows Server Essentials](../get-started/system-requirements.md).   

  
    > [!IMPORTANT]
    >  Avant d’installer Windows Server Essentials sur un ordinateur pré-existant, nous recommandons un formatage complet et de répartition des disques durs de l’ordinateur pré-existant. En effectuant un formatage et une répartition des disques durs, vous évitez que des partitions cachées restent sur les disques durs.  
  
-   **Préparer votre réseau** pour préparer votre réseau pour installer Windows Server Essentials, procédez comme suit :  
    
  
    -   **Mettre à niveau le système d’exploitation sur vos ordinateurs clients** Windows Server Essentials prend en charge les systèmes d’exploitation suivants :  Windows 8, Windows 7, Windows 10 et Macintosh OS X Lion ou version ultérieure. Ces systèmes d'exploitation fournissent des fonctions de sécurité nécessaires, la fiabilité, la performance et la fonctionnalité pour le réseau local.  
  
    -   **Configurez votre routeur** Vérifiez que votre routeur est configuré comme suit :  
  
        -   L'infrastructure UPnP est activée sur votre routeur.  
  
        -   Le service de serveur DHCP (Dynamic Host Configuration Protocol) pour le LAN peut être activé ou désactivé.  Windows Server Essentials permet de s’assurer que DHCP n’est pas en cours d’exécution sur le serveur et le routeur ? Lorsque le protocole DHCP est activé sur le routeur, DHCP n’est pas activé sur le serveur pendant l’installation.  
  
        -   Vous avez une adresse IP pour l'interface externe de votre ordinateur qui est fournie par votre fournisseur de services Internet (ISP). L'adresse IP peut être affectée dynamiquement par le service de serveur DHCP auprès de votre fournisseur de services Internet, ou vous pouvez configurer manuellement une adresse UP en utilisant la console de gestion du routeur.  
  
        -   Si votre connexion Internet (ou protocole PPPoE (Point-to-Point Protocol over Ethernet)) requiert un nom d'utilisateur et un mot de passe, ces paramètres sont configurés sur votre routeur, même si le périphérique prend en charge l'infrastructure UPnP.  
  
        -   Le routeur est connecté au LAN et à Internet, il est mis sous tension et fonctionne correctement.  
  
     Si votre routeur ne prend pas en charge l'infrastructure UPnP, ou si votre ordinateur ne peut pas être configuré pendant l'installation, vous devez la configurer manuellement pour votre réseau. Vérifiez que les ports suivants sont ouverts et qu'ils sont dirigés vers l'adresse IP du serveur de destination :  
  
    |Numéro de port|Application|  
    |-----------------|-----------------|  
    |Port 80|Trafic Web HTTP|  
    |Port 443|Trafic Web HTTPS|  
  

-   **Lire la documentation de version de Windows Server Essentials**. La documentation de version contient les dernières informations pouvant être critiques pour l’installation et configuration de Windows Server Essentials correctement. Pour afficher ou imprimer la documentation de version, consultez [Release Documentation for Windows Server Essentials](../get-started/release-notes.md).  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Installer Windows Server Essentials](Install-Windows-Server-Essentials.md)

