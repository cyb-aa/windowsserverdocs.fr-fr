---
ms.assetid: d555041a-709e-42c7-920a-8dfd2c7e0488
title: Vérifier qu’un serveur proxy de fédération est opérationnel
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 24f4fe2a152244dc904be82c4c10abe71abffcc4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359967"
---
# <a name="verify-that-a-federation-server-proxy-is-operational"></a>Vérifier qu’un serveur proxy de fédération est opérationnel


Vous pouvez utiliser la procédure suivante pour vérifier que le serveur proxy de Fédération peut communiquer avec le service FS (Federation Service) dans Services ADFS \(AD FS\). Vous exécutez cette procédure après avoir exécuté l' **Assistant Configuration du serveur proxy de fédération AD FS** pour configurer l’ordinateur afin qu’il s’exécute dans le rôle de serveur proxy de Fédération. Pour plus d’informations sur l’exécution de cet Assistant, consultez [configurer un ordinateur pour le rôle de serveur proxy de Fédération](Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).  
  
> [!IMPORTANT]  
> Le résultat du test est la génération réussie d’un événement spécifique dans l’Observateur d’événements sur le serveur proxy de fédération.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-verify-that-a-federation-server-proxy-is-operational"></a>Pour vérifier qu’un serveur proxy de Fédération est opérationnel  
  
1.  Connectez-vous au serveur proxy de Fédération en tant qu’administrateur.  
  
2.  Dans l’écran d' **Accueil** , tapez**Observateur d’événements**, puis appuyez sur entrée.  
  
3.  Dans le volet d’informations, double\-cliquez sur **journaux des applications et des services**, double\-cliquez sur **AD FS des événements**, puis cliquez sur **admin**.  
  
4.  Dans la colonne **ID de l’événement**, recherchez l’ID 198.  
  
    Si le serveur proxy de Fédération est correctement configuré, vous voyez un nouvel événement dans le journal des applications de observateur d’événements, avec l’ID d’événement 198. Cet événement vérifie que le service de serveur proxy de Fédération a été démarré avec succès et qu’il est maintenant en ligne.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : configuration d’un serveur proxy de Fédération](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

