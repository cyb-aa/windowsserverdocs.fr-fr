---
title: Dépanner la connectivité d’accès web à distance dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d3642575-b3ee-4488-b654-5bf9d3b8c935
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6db623308184c5be2968fa1d8991de2b48eef5b7
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318636"
---
# <a name="troubleshoot-remote-web-access-connectivity-in-windows-server-essentials"></a>Dépanner la connectivité d’accès web à distance dans Windows Server Essentials
 
>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 En général, Windows Server Essentials peut configurer automatiquement un routeur à large bande si le routeur est un appareil certifié UPnP et si le paramètre UPnP est activé sur le routeur.  
  
## <a name="possible-issues"></a>Problèmes possibles  
 Vous pouvez rencontrer les problèmes suivants avec la connectivité de l’accès web à distance :  
  
-   Votre routeur n’est pas activé ou n’est pas connecté au réseau.  
  
-   Le paramètre UPnP du routeur est désactivé.  
  
-   Le routeur ne peut pas entièrement prendre en charge la norme UPnP. Microsoft possède une liste de routeurs qui fonctionnent avec les systèmes d’exploitation Windows. Pour afficher la liste de routeurs (y compris les routeurs sans fil) qui sont compatibles avec Windows Server Essentials, visitez le [Centre de compatibilité Windows](https://www.microsoft.com/windows/compatibility/CompatCenter/Home).  
  
## <a name="possible-fixes"></a>Solutions possibles  
 Les actions suivantes peuvent résoudre ces problèmes :  
  
- Vérifiez que le routeur est activé et fonctionne correctement.  
  
- Assurez-vous que le serveur est connecté directement au routeur ou qu’il est connecté à un commutateur connecté au routeur.  
  
- Vérifiez que le périphérique large bande qui se connecte à votre fournisseur de services Internet est activé, fonctionne correctement, et que le routeur est connecté au périphérique large bande.  
  
- Activez le paramètre UPnP du routeur. Connectez-vous à la page web de configuration du routeur pour activer le paramètre UPnP. Pour plus d’informations sur la connexion au routeur et sur l’activation du paramètre UPnP, consultez la documentation de votre routeur. Une fois que vous avez activé le paramètre UPnP, exécutez à nouveau l’Assistant activer l’Accès web à distance pour configurer votre routeur.  
  
- Si le routeur ne prend pas entièrement en charge la norme UPnP, il ne peut pas être configuré automatiquement. Vous devez configurer manuellement le routeur ou acheter un routeur qui prend en charge la norme UPnP.  
  
   Pour configurer manuellement le routeur, effectuez les tâches suivantes :  
  
  - Créez la réservation d’adresse IP de votre serveur Windows Server Essentials.  
  
     Avant de configurer manuellement le routeur pour transférer les ports requis à Windows Server Essentials, vous devez configurer une réservation DHCP (Dynamic Host Configuration Protocol) pour le serveur qui exécute Windows Server Essentials sur le routeur. Cette étape garantit que l’adresse IP vers laquelle vous comptez transférer les ports ne change pas.  
  
     Pour plus d’informations sur la façon de configurer manuellement une réservation DHCP pour votre serveur sur votre routeur, consultez la documentation du fabricant de votre routeur.  
  
  - Configurez le transfert de ports sur votre routeur pour les ports suivants :  
  
    |Service ou protocole|Port|  
    |-------------------------|----------|  
    |HTTP|TCP 80|  
    |HTTPS|TCP 443|  
  
    Pour plus d’informations sur la configuration manuelle du réacheminement de port sur votre routeur, consultez la documentation du fabricant.  
  
    Une page classique de configuration d’un routeur inclut un tableau semblable au suivant.  
  
  > [!NOTE]
  >  Dans ce tableau, l’adresse IP de l’ordinateur qui exécute Windows Server Essentials est 192.168.0.100. Vous devez déterminer l’adresse IP de votre ordinateur et remplacez cette adresse par l’adresse IP affichée dans le tableau.  
  
  |Adresse IP|Protocole (TCP/UDP)|Schedule|Filtre d’entrée|  
  |----------------|---------------------------|--------------|--------------------|  
  |192.168.0.100|TCP 80|Toujours|Autoriser tout|  
  |192.168.0.100|TCP 443|Toujours|Autoriser tout|  
  
   Après avoir configuré manuellement votre routeur, exécutez l’Assistant activer l’Accès web à distance, en veillant à sélectionner l’option **ignorer le programme d’installation du routeur** sur la page **mise en route** .  
  
- Achetez un nouveau routeur si le vôtre ne prend pas entièrement en charge la norme UPnP.  
  
> [!TIP]
>  Assurez-vous que le dernier microprogramme BIOS est installé sur le routeur. Vous pouvez souvent mettre à jour le microprogramme BIOS du routeur à partir de la page web de configuration du routeur. Pour plus d’informations, consultez la documentation de votre routeur. Après la mise à jour du routeur, exécutez l’Assistant Configurer l’Accès en tout lieu.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Utiliser l’Accès web à distance](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gérer les Accès web distantes](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gérer l’accès en tout lieu](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Gérer Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  

-   [Prendre en charge Windows Server Essentials](Support-Windows-Server-Essentials.md)

-   [Prendre en charge Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

