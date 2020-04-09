---
title: Résoudre les problèmes liés à votre pare-feu dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 51d94b67-8b9b-4159-80dd-f652d73a43cb
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 1ff27915f3d6c92416ed22b7e143bdc1cf3b385f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853182"
---
# <a name="troubleshoot-your-firewall-in-windows-server-essentials"></a>Résoudre les problèmes liés à votre pare-feu dans Windows Server Essentials
 
>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Si vous rencontrez des problèmes d’accès à distance, exécutez l’Assistant Réparer l’Accès en tout lieu.  
  
### <a name="to-run-the-repair-anywhere-access-wizard"></a>Pour exécuter l’Assistant Réparer l’Accès en tout lieu  
  
1. Ouvrez le Tableau de bord.  
  
2. Cliquez sur **Paramètres**, sur l’onglet **Accès en tout lieu**, puis sur **Réparer**.  
  
3. Suivez les instructions de l’Assistant Réparer l’Accès en tout lieu.  
  
   Si vous utilisez une configuration de réseau avancée ou utilisez un pare-feu non-Microsoft, vous devrez peut-être ouvrir des ports supplémentaires sur le pare-feu. Les ports dans le tableau suivant sont enregistrés auprès de l’Internet Assigned Numbers Authority (IANA).  
  
|Numéro de port|Description|  
|-----------------|-----------------|  
|65500|Service web Certificat|  
|65510 et 65515|Site web Déploiement d’ordinateurs clients|  
|65520|Service web pour les ordinateurs client Mac|  
|65532|Infrastructure de fournisseurs pour les communications de bouclage de serveur|  
|6602|Infrastructure de fournisseurs pour la communication entre les ordinateurs client et le serveur|  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Utiliser l’Accès web à distance](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gérer les Accès web distantes](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gérer l’accès en tout lieu](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Gérer Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  

-   [Prendre en charge Windows Server Essentials](Support-Windows-Server-Essentials.md)

-   [Prendre en charge Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

