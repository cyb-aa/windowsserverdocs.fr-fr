---
ms.assetid: 10d6723e-c857-43da-9d2d-acb5641d3da8
title: Joindre un ordinateur à un domaine
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 02df9659ee3a1121c0cee3f7c5fa21b91c36b87c
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192055"
---
# <a name="join-a-computer-to-a-domain"></a>Joindre un ordinateur à un domaine

Pour les Services de fédération Active Directory \(AD FS\) de fonctionner, chaque ordinateur qui fonctionne comme un serveur de fédération doit être joint à un domaine. serveurs proxy de fédération peut-être être joints à un domaine, mais cela n’est pas obligatoire.  
  
Vous n’êtes pas obligé de joindre un serveur Web à un domaine, si le serveur Web héberge revendications\-uniquement les applications prenant en charge.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-join-a-computer-to-a-domain"></a>Pour joindre un ordinateur à un domaine  
  
1.  Sur le **Démarrer** , tapez **le panneau de configuration**, puis appuyez sur ENTRÉE.  
  
2.  Accédez à **système et sécurité**, puis cliquez sur **système**.  
  
3.  Sous **Paramètres de nom d'ordinateur, de domaine et de groupe de travail**, cliquez sur **Modifier les paramètres**.  
  
4.  Sous l’onglet **Nom de l’ordinateur**, cliquez sur **Modifier**.  
  
5.  Sous **membre**, cliquez sur **domaine**, tapez le nom du domaine que vous souhaitez que cet ordinateur à joindre, puis cliquez sur **OK**.  
  
6.  Cliquez sur **OK**, puis redémarrez l’ordinateur.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Liste de vérification : configuration d’un serveur de fédération proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

