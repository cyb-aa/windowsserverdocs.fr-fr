---
ms.assetid: 10d6723e-c857-43da-9d2d-acb5641d3da8
title: "Joindre un ordinateur à un domaine"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 641a1541143206d06973a6a0f11c689390abea21
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="join-a-computer-to-a-domain"></a>Joindre un ordinateur à un domaine

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Pour ActiveDirectory Federation Services \(ADFS\) de la fonction, chaque ordinateur qui fonctionne comme un serveur de fédération doit être joint à un domaine. Serveurs proxy de fédération doivent être joints à un domaine, mais ce n’est pas obligatoire.  
  
Vous n’êtes pas obligé de joindre un serveur Web à un domaine, si le serveur Web héberge uniquement les applications prenant en charge claims\.  
  
L’appartenance au groupe **administrateurs**, ou équivalente, sur l’ordinateur local est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-join-a-computer-to-a-domain"></a>Pour joindre un ordinateur à un domaine  
  
1.  Sur le **Démarrer**, tapez**le panneau de configuration**, puis appuyez sur ENTRÉE.  
  
2.  Accédez à **système et sécurité**, puis cliquez sur **système**.  
  
3.  Sous **paramètres de groupe de travail, le domaine et nom de l’ordinateur**, cliquez sur **modifier les paramètres**.  
  
4.  Sur le **nom de l’ordinateur**, cliquez sur **modification**.  
  
5.  Sous **membre**, cliquez sur **domaine**, tapez le nom du domaine auquel joindre cet ordinateur et puis cliquez sur **OK**.  
  
6.  Cliquez sur **OK**, puis redémarrez l’ordinateur.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification: Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Liste de vérification: Configuration d’un serveur Proxy de fédération](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

