---
ms.assetid: e1f2ce2d-b24f-4ccd-8add-9e69419fc6c1
title: Importer un certificat d'authentification serveur sur le site web par défaut
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 7bc890c744de5cd86d4e8b0418e75512518f656c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880940"
---
# <a name="import-a-server-authentication-certificate-to-the-default-web-site"></a>Importer un certificat d'authentification serveur sur le site web par défaut

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Après avoir obtenu un serveur de certificat d’authentification auprès d’une autorité de certification \(autorité de certification\), vous devez installer manuellement ce certificat sur le Site Web par défaut pour chaque serveur de fédération ou le serveur proxy de fédération dans une batterie de serveurs.  
  
Pour les serveurs web, vous devez installer le certificat d'authentification serveur sur le site web ou le répertoire virtuel où se trouve votre application fédérée.  
  
Si vous configurez une batterie de serveurs, assurez-vous d'effectuer exactement la même procédure, en utilisant des paramètres identiques, sur chacun des serveurs.  
  
> [!NOTE]  
> Le composant logiciel enfichable Gestion AD FS\-dans fait référence aux certificats d’authentification serveur pour les serveurs de fédération en tant que certificats de communication de service.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-import-a-server-authentication-certificate-to-the-default-web-site"></a>Pour importer un certificat d'authentification serveur sur le site web par défaut  
  
1.  Sur le **Démarrer** , tapez**Internet Information Services \(IIS\) Manager**, puis appuyez sur ENTRÉE.  
  
2.  Dans l'arborescence de la console, cliquez sur **Nom_Ordinateur**.  
  
3.  Dans le volet central, double-cliquez\-cliquez sur **certificats de serveur**.  
  
4.  Dans le volet **Actions**, cliquez sur **Importer**.  
  
5.  Dans le **importer un certificat** boîte de dialogue, cliquez sur le **...** .  
  
6.  Recherchez le fichier de certificat pfx, mettez-le en surbrillance, puis cliquez sur **Ouvrir**.  
  
7.  Tapez le mot de passe du certificat, puis cliquez sur **OK**.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Liste de vérification : Configuration d’un serveur Proxy de fédération](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Configuration requise des certificats pour les serveurs de fédération](https://technet.microsoft.com/library/dd807040.aspx)  
  
[Configuration requise des certificats pour les serveurs proxy de fédération](https://technet.microsoft.com/library/dd807054.aspx)  
   
  

