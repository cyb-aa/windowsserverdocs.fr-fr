---
ms.assetid: e1f2ce2d-b24f-4ccd-8add-9e69419fc6c1
title: Importer un certificat d'authentification serveur sur le site web par défaut
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 3f02358167b024247f934a46218028575e393ba9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855402"
---
# <a name="import-a-server-authentication-certificate-to-the-default-web-site"></a>Importer un certificat d'authentification serveur sur le site web par défaut

Après avoir obtenu un certificat d’authentification serveur auprès d’une autorité de certification \(\)de l’autorité de certification, vous devez installer manuellement ce certificat sur le site Web par défaut pour chaque serveur de Fédération ou serveur proxy de Fédération dans une batterie de serveurs.  
  
Pour les serveurs web, vous devez installer le certificat d'authentification serveur sur le site web ou le répertoire virtuel où se trouve votre application fédérée.  
  
Si vous configurez une batterie de serveurs, assurez-vous d'effectuer exactement la même procédure, en utilisant des paramètres identiques, sur chacun des serveurs.  
  
> [!NOTE]  
> Le\-du composant logiciel enfichable Gestion de AD FS dans fait référence aux certificats d’authentification serveur pour les serveurs de Fédération en tant que certificats de communication de service.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs** ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-import-a-server-authentication-certificate-to-the-default-web-site"></a>Pour importer un certificat d’authentification serveur sur le site Web par défaut  
  
1.  Dans l’écran d' **Accueil** , tapez**Internet Information Services \(IIS\) Manager**, puis appuyez sur entrée.  
  
2.  Dans l'arborescence de la console, cliquez sur **Nom_Ordinateur**.  
  
3.  Dans le volet central, double\-cliquez sur **certificats de serveur**.  
  
4.  Dans le volet **Actions**, cliquez sur **Importer**.  
  
5.  Dans la boîte de dialogue **Importer un certificat** , cliquez sur **...** de l'ordinateur.  
  
6.  Recherchez le fichier de certificat pfx, mettez-le en surbrillance, puis cliquez sur **Ouvrir**.  
  
7.  Tapez le mot de passe du certificat, puis cliquez sur **OK**.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : configuration d’un serveur de Fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Liste de vérification : configuration d’un serveur proxy de Fédération](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Certificats requis pour les serveurs de fédération](https://technet.microsoft.com/library/dd807040.aspx)  
  
[Certificats requis pour les serveurs proxy de fédération](https://technet.microsoft.com/library/dd807054.aspx)  
   
  

