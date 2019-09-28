---
ms.assetid: e1f2ce2d-b24f-4ccd-8add-9e69419fc6c1
title: Importer un certificat d'authentification serveur sur le site web par défaut
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: b1c602d0cdfa562469419de223f5691ec2ff4527
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359564"
---
# <a name="import-a-server-authentication-certificate-to-the-default-web-site"></a>Importer un certificat d'authentification serveur sur le site web par défaut

Une fois que vous avez obtenu un certificat d’authentification serveur auprès d’une autorité de certification \(CA @ no__t-1, vous devez installer manuellement ce certificat sur le site Web par défaut pour chaque serveur de Fédération ou serveur proxy de Fédération dans une batterie de serveurs.  
  
Pour les serveurs web, vous devez installer le certificat d'authentification serveur sur le site web ou le répertoire virtuel où se trouve votre application fédérée.  
  
Si vous configurez une batterie de serveurs, assurez-vous d'effectuer exactement la même procédure, en utilisant des paramètres identiques, sur chacun des serveurs.  
  
> [!NOTE]  
> Le composant logiciel enfichable de gestion AD FS @ no__t-0in fait référence aux certificats d’authentification serveur pour les serveurs de Fédération en tant que certificats de communication de service.  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-import-a-server-authentication-certificate-to-the-default-web-site"></a>Pour importer un certificat d'authentification serveur sur le site web par défaut  
  
1.  Dans l’écran d' **Accueil** , tapez**Internet Information Services \(IIS @ No__t-3 Manager**, puis appuyez sur entrée.  
  
2.  Dans l'arborescence de la console, cliquez sur **Nom_Ordinateur**.  
  
3.  Dans le volet central, doublez les certificats de **serveur**@ no__t-0click.  
  
4.  Dans le volet **Actions**, cliquez sur **Importer**.  
  
5.  Dans la boîte de dialogue **Importer un certificat** , cliquez sur **...** .  
  
6.  Recherchez le fichier de certificat pfx, mettez-le en surbrillance, puis cliquez sur **Ouvrir**.  
  
7.  Tapez le mot de passe du certificat, puis cliquez sur **OK**.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Liste de vérification : configuration d’un serveur de fédération proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Certificats requis pour les serveurs de fédération](https://technet.microsoft.com/library/dd807040.aspx)  
  
[Certificats requis pour les serveurs proxy de fédération](https://technet.microsoft.com/library/dd807054.aspx)  
   
  

