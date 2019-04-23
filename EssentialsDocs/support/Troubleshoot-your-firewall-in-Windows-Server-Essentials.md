---
title: Résoudre les problèmes liés à votre pare-feu dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846550"
---
# <a name="troubleshoot-your-firewall-in-windows-server-essentials"></a>Résoudre les problèmes liés à votre pare-feu dans Windows Server Essentials
 
>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Si vous rencontrez des problèmes d’accès à distance, exécutez l’Assistant Réparer l’Accès en tout lieu.  
  
### <a name="to-run-the-repair-anywhere-access-wizard"></a>Pour exécuter l’Assistant Réparer l’Accès en tout lieu  
  
1.  Ouvrez le tableau de bord.  
  
2.  Cliquez sur **Paramètres**, sur l’onglet **Accès en tout lieu** , puis sur **Réparer**.  
  
3.  Suivez les instructions de l’Assistant Réparer l’Accès en tout lieu.  
  
 Si vous utilisez une configuration de réseau avancée ou utilisez un pare-feu non-Microsoft, vous devrez peut-être ouvrir des ports supplémentaires sur le pare-feu. Les ports dans le tableau suivant sont enregistrés auprès de l’Internet Assigned Numbers Authority (IANA).  
  
|Numéro de port|Description|  
|-----------------|-----------------|  
|65500|Service web Certificat|  
|65510 et 65515|Site web Déploiement d’ordinateurs clients|  
|65520|Service web pour les ordinateurs client Mac|  
|65532|Infrastructure de fournisseurs pour les communications de bouclage de serveur|  
|6602|Infrastructure de fournisseurs pour la communication entre les ordinateurs client et le serveur|  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Utiliser l’accès Web à distance](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gérer l’accès Web à distance](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gérer l’accès en tout lieu](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Gérer Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  

-   [Prise en charge Windows Server Essentials](Support-Windows-Server-Essentials.md)

-   [Prise en charge Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

