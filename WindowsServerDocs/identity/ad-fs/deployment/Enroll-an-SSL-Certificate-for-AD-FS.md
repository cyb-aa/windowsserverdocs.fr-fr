---
ms.assetid: 3095e6a7-b562-4c6a-bf29-13b32c133cac
title: Inscrire un certificat SSL pour AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6f7af40f23c3fa3bd0a31ecb74b11013133a4b32
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855432"
---
# <a name="enroll-an-ssl-certificate-for-ad-fs"></a>Inscrire un certificat SSL pour AD FS

Services ADFS \(AD FS\) requiert un certificat pour le protocole Secure Socket Layer \(l’authentification de serveur SSL\) sur chaque serveur de Fédération de votre batterie de serveurs de Fédération. Le même certificat peut être utilisé sur chaque serveur de Fédération d’une batterie de serveurs. Vous devez avoir accès à la fois au certificat et à sa clé privée. Par exemple, si le certificat et sa clé privée se trouvent dans un fichier .pfx, vous pouvez importer le fichier directement dans l’Assistant Configuration des services AD FS (Active Directory Federation Services). Ce certificat SSL doit contenir les éléments suivants :  
  
1.  Le nom d’objet et l’autre nom de l’objet doivent contenir le nom de votre service de Fédération, par exemple fs.contoso.com.  
  
2.  L’autre nom de l’objet doit contenir la valeur **enterpriseregistration** suivie du nom d’utilisateur principal \(suffixe UPN\) de votre organisation, par exemple, **enterpriseregistration.Corp.contoso.com**.  
  
    > [!WARNING]  
    > Spécifiez l’autre nom de l’objet si vous envisagez d’activer le service Device Registration service \(DRS\) pour Workplace Join.  
  
> [!IMPORTANT]  
> Si votre organisation utilise plusieurs suffixes UPN et que vous envisagez d’activer le DRS, le certificat SSL doit contenir une entrée d’autre nom de l’objet pour chaque suffixe.  
  
## <a name="see-also"></a>Voir aussi
[Déploiement d’AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guide de déploiement de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Déploiement d’une batterie de serveurs de fédération](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  
  

