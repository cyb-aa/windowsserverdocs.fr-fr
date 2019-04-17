---
ms.assetid: 638c89bd-87e6-484b-9d2e-8ae2a74227e5
title: "Définir un certificat de Communications de Service"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 0d630372d82cf1519da82397a9c90d6a28903ed0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="set-a-service-communications-certificate"></a>Définir un certificat de Communications de Service

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Les serveurs de fédération dans ActiveDirectory Federation Services \(ADFS\) utilisent le certificat de communications de service pour sécuriser le trafic des services Web pour la communication \(SSL\) Secure Sockets Layer avec les clients Web ou serveurs proxy de fédération. Il s’agit du même certificat par un serveur de fédération que le certificat SSL dans Internet Information Services \(IIS\).  
  
Vous pouvez utiliser la procédure suivante pour changer le certificat de communications de service avec la gestion ADFS de composants.  
  
> [!NOTE]  
> La gestion ADFS enfichable fait référence aux certificats d’authentification serveur pour les serveurs de fédération en tant que certificats de communication du service.  
  
L’appartenance au groupe **administrateurs**, ou équivalente, sur l’ordinateur local est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
### <a name="to-set-a-service-communications-certificate"></a>Pour définir un certificat de communications de service  
  
1.  Sur le **Démarrer**, tapez**gestion ADFS**, puis appuyez sur ENTRÉE.  
  
2.  Dans l’arborescence de la console, double-cliquant sur **Service**, puis cliquez sur **certificats**.  
  
3.  Dans le **Actions** volet, cliquez sur le **définir le certificat de communication du Service** lien.  
  
4.  Dans le **sélectionner un certificat de communications de service** boîte de dialogue zone, accédez au fichier de certificat que vous souhaitez définir en tant que certificat de communication du service, sélectionnez le fichier de certificat, puis cliquez sur **ouvrir**.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification: Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Certificats requis pour les serveurs de fédération](https://technet.microsoft.com/library/dd807040.aspx)  
  

