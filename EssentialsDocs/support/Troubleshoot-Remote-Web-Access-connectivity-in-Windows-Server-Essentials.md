---
title: "Résoudre les problèmes de connectivité d’accès Web à distance dans WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d3642575-b3ee-4488-b654-5bf9d3b8c935
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: af4725fd3b1861c847434e3ed62c3da030689fb5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-remote-web-access-connectivity-in-windows-server-essentials"></a>Résoudre les problèmes de connectivité d’accès Web à distance dans WindowsServerEssentials
 
>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials
  
 En règle générale, WindowsServerEssentials peut configurer automatiquement un routeur à large bande si le routeur est un appareil certifié UPnP et si le paramètre UPnP est activé sur le routeur.  
  
## <a name="possible-issues"></a>Problèmes possibles  
 Vous pouvez rencontrer les problèmes suivants avec une connectivité de l’accès Web à distance:  
  
-   Votre routeur n’est pas activé ou n’est pas connecté à votre réseau.  
  
-   Le paramètre UPnP du routeur est désactivé.  
  
-   Votre routeur ne peut pas entièrement en charge la norme UPnP. Microsoft gère une liste de routeurs qui fonctionnent avec les systèmes d’exploitation Windows. Pour afficher la liste de routeurs (y compris les routeurs sans fil) qui sont compatibles avec WindowsServerEssentials, visitez le [centre de compatibilité Windows](https://www.microsoft.com/windows/compatibility/CompatCenter/Home).  
  
## <a name="possible-fixes"></a>Solutions possibles  
 Les actions suivantes peuvent résoudre ces problèmes:  
  
-   Vérifiez que votre routeur est activé et fonctionne correctement.  
  
-   Assurez-vous que votre serveur est connecté directement à votre routeur ou qu’il est connecté à un commutateur qui est connecté à votre routeur.  
  
-   Vérifiez que le périphérique large bande qui se connecte à votre fournisseur de services Internet (ISP) est activé, fonctionne correctement, et que votre routeur est connecté au périphérique large bande.  
  
-   Activez le paramètre UPnP du routeur. Se connecter à la page web de configuration de votre routeur pour activer le paramètre UPnP. Pour plus d’informations sur comment ouvrez une session sur votre routeur et comment activer le paramètre UPnP, consultez la documentation de votre routeur. Une fois que vous activez le paramètre UPnP, exécutez l’activer sur Web Assistant accès à distance pour configurer votre routeur.  
  
-   Si votre routeur ne prend pas entièrement en charge la norme UPnP, il ne peut pas être configuré automatiquement. Vous devez manuellement configurer votre routeur ou acheter un routeur qui prend en charge la norme UPnP.  
  
     Pour configurer manuellement le routeur, effectuez les tâches suivantes:  
  
    -   Créez la réservation d’adresse IP de votre serveur WindowsServerEssentials.  
  
         Avant de configurer manuellement le routeur pour transférer les ports requis pour WindowsServerEssentials, vous devez configurer une réservation DHCP Dynamic Host Configuration Protocol () pour votre serveur qui exécute WindowsServerEssentials sur le routeur. Cette étape garantit que l’adresse IP que vous comptez transférer les ports à ne pas modifier.  
  
         Pour plus d’informations sur la façon de configurer manuellement une réservation DHCP pour votre serveur sur votre routeur, consultez la documentation s du fabricant de votre routeur.  
  
    -   Configurez le réacheminement de port sur votre routeur pour les ports suivants:  
  
        |Service ou protocole|Port|  
        |-------------------------|----------|  
        |HTTP|TCP 80|  
        |HTTPS|TCP 443|  
  
     Pour plus d’informations sur la façon de configurer manuellement le transfert de ports sur votre routeur, consultez la documentation du fabricant s.  
  
     Page de configuration d’un routeur classique inclut un tableau semblable au suivant.  
  
    > [!NOTE]
    >  Dans ce tableau, l’adresse IP de l’ordinateur qui exécute WindowsServerEssentials est 192.168.0.100. Vous devez déterminer l’adresse IP de votre ordinateur et remplacez cette adresse pour l’adresse IP affichée dans le tableau.  
  
    |Adresse IP|Protocole (TCP/UDP)|Calendrier|Filtre de trafic entrant|  
    |----------------|---------------------------|--------------|--------------------|  
    |192.168.0.100|TCP 80|Toujours|Autoriser tout|  
    |192.168.0.100|TCP 443|Toujours|Autoriser tout|  
  
     Après avoir configuré manuellement votre routeur, exécutez l’activer sur Web Assistant accès à distance, en sélectionnant le **ignorer la configuration du routeur** option sur le **prise en main** page.  
  
-   Acheter un nouveau routeur si votre routeur ne prend pas entièrement en charge la norme UPnP.  
  
> [!TIP]
>  Vérifiez que le dernier microprogramme BIOS est installé sur votre routeur. Vous pouvez souvent mettre à jour le microprogramme BIOS du routeur à partir de la page web de configuration pour le routeur. Pour plus d’informations, consultez la documentation de votre routeur. Après la mise à jour de votre routeur, exécutez l’Assistant Configuration de n’importe quel endroit accès.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Utiliser l’accès Web à distance](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gérer l’accès Web à distance](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gérer l’accès en tout lieu](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Gérer WindowsServerEssentials](../manage/Manage-Windows-Server-Essentials.md)  
  

-   [Prise en charge Windows Server Essentials](Support-Windows-Server-Essentials.md)

-   [Prise en charge Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

