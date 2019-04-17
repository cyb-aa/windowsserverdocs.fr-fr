---
ms.assetid: e1f2ce2d-b24f-4ccd-8add-9e69419fc6c1
title: "Importer un certificat d’authentification serveur sur le site Web par défaut"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 7bc890c744de5cd86d4e8b0418e75512518f656c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="import-a-server-authentication-certificate-to-the-default-web-site"></a>Importer un certificat d’authentification serveur sur le site Web par défaut

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Après avoir obtenu un serveur de certificat d’authentification à partir d’une autorité de certification \(CA\), vous devez installer manuellement ce certificat sur le site Web par défaut pour chaque serveur de fédération ou un serveur proxy de fédération dans une batterie de serveurs.  
  
Pour les serveurs Web, vous devez installer manuellement le certificat d’authentification serveur sur le site Web approprié ou le répertoire virtuel dans lequel réside votre application fédérée.  
  
Si vous configurez une batterie de serveurs, veillez à effectuer cette procédure identique, à l’aide des paramètres identiques — sur chacun des serveurs dans votre batterie de serveurs.  
  
> [!NOTE]  
> La gestion ADFS enfichable fait référence aux certificats d’authentification serveur pour les serveurs de fédération en tant que certificats de communication du service.  
  
L’appartenance au groupe **administrateurs**, ou équivalente, sur l’ordinateur local est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-import-a-server-authentication-certificate-to-the-default-web-site"></a>Pour importer un certificat d’authentification serveur sur le site Web par défaut  
  
1.  Sur le **Démarrer**, tapez**\(IIS\) Internet Information Services Manager**, puis appuyez sur ENTRÉE.  
  
2.  Dans l’arborescence de la console, cliquez sur **Nom_Ordinateur**.  
  
3.  Dans le volet central, double-cliquant sur **certificats de serveur**.  
  
4.  Dans le **Actions** volet, cliquez sur **Import**.  
  
5.  Dans le **importer un certificat** boîte de dialogue, cliquez sur le **...** bouton.  
  
6.  Accédez à l’emplacement du fichier de certificat pfx, mettez-la en surbrillance, puis cliquez sur **ouvrir**.  
  
7.  Tapez un mot de passe pour le certificat, puis cliquez sur **OK**.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification: Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md)  
  
[Liste de vérification: Configuration d’un serveur Proxy de fédération](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Certificats requis pour les serveurs de fédération](https://technet.microsoft.com/library/dd807040.aspx)  
  
[Certificats requis pour les serveurs proxy de fédération](https://technet.microsoft.com/library/dd807054.aspx)  
   
  

