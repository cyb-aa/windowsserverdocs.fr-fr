---
ms.assetid: 638c89bd-87e6-484b-9d2e-8ae2a74227e5
title: Définir un certificat de communication du service
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 0d630372d82cf1519da82397a9c90d6a28903ed0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888470"
---
# <a name="set-a-service-communications-certificate"></a>Définir un certificat de communication du service

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Serveurs de fédération dans Active Directory Federation Services \(AD FS\) utiliser le certificat de communications de service pour sécuriser le trafic des services Web pour Secure Sockets Layer \(SSL\) communication avec le Web les clients ou avec des serveurs proxy de fédération. Il s’agit du même certificat qu’un serveur de fédération utilise en tant que le certificat SSL dans Internet Information Services \(IIS\).  
  
Vous pouvez utiliser la procédure suivante pour modifier le certificat de communications de service avec le composant logiciel enfichable Gestion AD FS\-dans.  
  
> [!NOTE]  
> Le composant logiciel enfichable Gestion AD FS\-dans fait référence aux certificats d’authentification serveur pour les serveurs de fédération en tant que certificats de communication de service.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’aide de comptes appropriés et les appartenances au groupe [locaux et les groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) \(http :\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-set-a-service-communications-certificate"></a>Pour définir un certificat de communications de service  
  
1.  Sur le **Démarrer** , tapez**gestion AD FS**, puis appuyez sur ENTRÉE.  
  
2.  Dans l’arborescence de la console, double-cliquez\-cliquez sur **Service**, puis cliquez sur **certificats**.  
  
3.  Dans le **Actions** volet, cliquez sur le **définir le certificat de communication du Service** lien.  
  
4.  Dans le **sélectionner un certificat de communications de service** boîte de dialogue, accédez au fichier de certificat que vous souhaitez définir en tant que le certificat de communications de service, sélectionnez le fichier de certificat, puis cliquez sur **ouvrir**.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Configuration requise des certificats pour les serveurs de fédération](https://technet.microsoft.com/library/dd807040.aspx)  
  

