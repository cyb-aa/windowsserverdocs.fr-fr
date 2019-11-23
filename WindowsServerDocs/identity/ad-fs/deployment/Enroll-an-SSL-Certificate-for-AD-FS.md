---
ms.assetid: 3095e6a7-b562-4c6a-bf29-13b32c133cac
title: Inscrire un certificat SSL pour AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: efa7c7aee848a5bbb68d3ce7140e135d37c2161d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408368"
---
# <a name="enroll-an-ssl-certificate-for-ad-fs"></a>Inscrire un certificat SSL pour AD FS

Services ADFS \(AD FS\) requiert un certificat pour le protocole Secure Socket Layer \(l’authentification de serveur SSL\) sur chaque serveur de Fédération de votre batterie de serveurs de Fédération. Le même certificat peut être utilisé sur chaque serveur de Fédération d’une batterie de serveurs. Le certificat et sa clé privée doivent être disponibles. Par exemple, si le certificat et sa clé privée se trouvent dans un fichier .pfx, vous pouvez importer le fichier directement dans l’Assistant Configuration des services AD FS (Active Directory Federation Services). Ce certificat SSL doit contenir les éléments suivants :  
  
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
  
  

