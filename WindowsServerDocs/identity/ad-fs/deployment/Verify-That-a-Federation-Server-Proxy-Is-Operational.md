---
ms.assetid: d555041a-709e-42c7-920a-8dfd2c7e0488
title: "Vérifier qu’un serveur Proxy de fédération est opérationnel"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 4900d8621b94a514a07bba55b2f7f3df5dd36353
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="verify-that-a-federation-server-proxy-is-operational"></a>Vérifier qu’un serveur Proxy de fédération est opérationnel

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Vous pouvez utiliser la procédure suivante pour vérifier que le serveur proxy de fédération peut communiquer avec le Service de fédération dans Active Directory Federation Services \(AD FS\). Vous exécutez cette procédure après avoir exécuté la **Assistant Configuration de Proxy du serveur de fédération AD FS** pour configurer l’ordinateur pour exécuter le rôle de proxy de serveur de fédération. Pour plus d’informations sur l’exécution de cet Assistant, consultez [configurer un ordinateur pour le rôle de Proxy de serveur de fédération](Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).  
  
> [!IMPORTANT]  
> Le résultat de ce test est la génération réussie d’un événement spécifique dans l’Observateur d’événements sur l’ordinateur de proxy de serveur de fédération.  
  
L’appartenance au groupe **administrateurs**, ou équivalente, sur l’ordinateur local est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-verify-that-a-federation-server-proxy-is-operational"></a>Pour vérifier qu’un serveur proxy de fédération est opérationnel  
  
1.  Ouvrez une session sur le serveur proxy de fédération en tant qu’administrateur.  
  
2.  Sur le **Démarrer** , tapez**l’Observateur d’événements**, puis appuyez sur ENTRÉE.  
  
3.  Dans le volet détails, double-cliquant sur **journaux des Applications et Services**, double-cliquant sur **AD FS Eventing**, puis cliquez sur **Admin**.  
  
4.  Dans le **ID d’événement** colonne, recherchez l’ID 198.  
  
    Si le serveur proxy de fédération est correctement configuré, vous voyez un nouvel événement dans le journal des applications de l’Observateur d’événements, avec l’événement ID 198. Cet événement vérifie que le service de proxy de serveur de fédération a démarré et qu’il est désormais en ligne.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification: Configuration d’un serveur Proxy de fédération](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

