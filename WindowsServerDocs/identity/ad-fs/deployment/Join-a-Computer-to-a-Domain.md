---
ms.assetid: 10d6723e-c857-43da-9d2d-acb5641d3da8
title: Joindre un ordinateur à un domaine
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 9f6d657397cb07d081a229135e3e6c97c7191164
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408340"
---
# <a name="join-a-computer-to-a-domain"></a>Joindre un ordinateur à un domaine

Par Services ADFS \(AD FS\) pour fonctionner, chaque ordinateur qui fonctionne en tant que serveur de Fédération doit être joint à un domaine. les serveurs proxys de Fédération peuvent être joints à un domaine, mais ce n’est pas obligatoire.  
  
Vous n’avez pas besoin de joindre un serveur Web à un domaine si le serveur Web héberge uniquement des revendications\-des applications prenant en charge les revendications.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-join-a-computer-to-a-domain"></a>Pour joindre un ordinateur à un domaine  
  
1.  Dans l’écran d' **Accueil** , tapez **panneau de configuration**, puis appuyez sur entrée.  
  
2.  Accédez à **système et sécurité**, puis cliquez sur **système**.  
  
3.  Sous **Paramètres de nom d'ordinateur, de domaine et de groupe de travail**, cliquez sur **Modifier les paramètres**.  
  
4.  Sous l’onglet **Nom de l’ordinateur**, cliquez sur **Modifier**.  
  
5.  Sous **membre de**, cliquez sur **domaine**, tapez le nom du domaine auquel cet ordinateur doit être joint, puis cliquez sur **OK**.  
  
6.  Cliquez sur **OK**, puis redémarrez l’ordinateur.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : configuration d’un serveur de Fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Liste de vérification : configuration d’un serveur proxy de Fédération](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

