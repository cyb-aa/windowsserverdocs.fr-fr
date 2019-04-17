---
title: "Dépanner votre pare-feu dans WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 51d94b67-8b9b-4159-80dd-f652d73a43cb
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 3c48d2abb7fd8431f40f76f8eece5c4142be4c75
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="troubleshoot-your-firewall-in-windows-server-essentials"></a>Dépanner votre pare-feu dans WindowsServerEssentials
 
>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials
  
 Si vous rencontrez des problèmes d’accès à distance, exécutez l’Assistant Réparation de n’importe quel endroit accès.  
  
### <a name="to-run-the-repair-anywhere-access-wizard"></a>Pour exécuter l’Assistant Réparation de n’importe quel endroit accès  
  
1.  Ouvrez le tableau de bord.  
  
2.  Cliquez sur **paramètres**, cliquez sur le **accès en tout lieu** onglet, puis cliquez sur **réparation**.  
  
3.  Suivez les instructions de l’Assistant Réparation de n’importe quel endroit accès.  
  
 Si vous utilisez une configuration de réseau avancée ou à l’aide d’un pare-feu non Microsoft, vous devrez peut-être ouvrir des ports supplémentaires sur le pare-feu. Les ports dans le tableau suivant sont enregistrés avec Internet IANA Assigned Numbers Authority ().  
  
|Numéro de port|Description|  
|-----------------|-----------------|  
|65500|Service web de certificat|  
|65510 et 65515|Site Web déploiement d’ordinateur client|  
|65520|Service Web pour les ordinateurs client Mac|  
|65532|Infrastructure de fournisseurs pour les communications de bouclage de serveur|  
|6602|Infrastructure de fournisseurs pour la communication entre le serveur et les ordinateurs clients|  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Utiliser l’accès Web à distance](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gérer l’accès Web à distance](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gérer l’accès en tout lieu](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Gérer WindowsServerEssentials](../manage/Manage-Windows-Server-Essentials.md)  
  

-   [Prise en charge Windows Server Essentials](Support-Windows-Server-Essentials.md)

-   [Prise en charge Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

