---
ms.assetid: d555041a-709e-42c7-920a-8dfd2c7e0488
title: Vérifier qu’un serveur proxy de fédération est opérationnel
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 26b0ae4f331607d83c6b94a2655ddc9eded8a356
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191866"
---
# <a name="verify-that-a-federation-server-proxy-is-operational"></a>Vérifier qu’un serveur proxy de fédération est opérationnel


Vous pouvez utiliser la procédure suivante pour vérifier que le serveur proxy de fédération peut communiquer avec le Service de fédération dans Active Directory Federation Services \(AD FS\). Vous exécutez cette procédure après avoir exécuté le **Assistant Configuration Proxy du serveur de fédération AD FS** pour configurer l’ordinateur pour exécuter le rôle de proxy de serveur de fédération. Pour plus d’informations sur l’exécution de cet Assistant, consultez [configurer un ordinateur pour le rôle de Proxy de serveur de fédération](Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).  
  
> [!IMPORTANT]  
> Le résultat du test est la génération réussie d’un événement spécifique dans l’Observateur d’événements sur le serveur proxy de fédération.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-verify-that-a-federation-server-proxy-is-operational"></a>Pour vérifier qu’un serveur proxy de fédération est opérationnel  
  
1.  Ouvrez une session le serveur proxy de fédération en tant qu’administrateur.  
  
2.  Sur le **Démarrer** , tapez**Observateur d’événements**, puis appuyez sur ENTRÉE.  
  
3.  Dans le volet de détails, double-cliquez\-cliquez sur **journaux des Applications et Services**, double\-cliquez sur **événements AD FS**, puis cliquez sur **administrateur**.  
  
4.  Dans la colonne **ID de l’événement**, recherchez l’ID 198.  
  
    Si le serveur proxy de fédération est correctement configuré, vous voyez un nouvel événement dans le journal des applications de l’Observateur d’événements, avec l’événement ID 198. Cet événement vérifie que le service de proxy de serveur de fédération a été démarré avec succès et est désormais en ligne.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : configuration d’un serveur de fédération proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

