---
ms.assetid: 3095e6a7-b562-4c6a-bf29-13b32c133cac
title: Inscrire un certificat SSL pour ADFS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 12f544ad0d037c4ae7a9789238186b7ded311bdf
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="enroll-an-ssl-certificate-for-ad-fs"></a>Inscrire un certificat SSL pour ADFS

>S’applique à: Windows Server2016, Windows Server2012R2

ActiveDirectory Federation Services \(ADFS\) nécessite un certificat pour l’authentification du serveur \(SSL\) (Secure Socket Layer) sur chaque serveur de fédération dans votre batterie de serveurs de fédération. Le même certificat peut être utilisé sur chaque serveur de fédération dans une batterie de serveurs. Vous devez disposer du certificat et sa clé privée disponible. Par exemple, si vous avez le certificat et sa clé privée dans un fichier .pfx, vous pouvez importer le fichier directement dans l’Assistant Configuration des Services de fédération ActiveDirectory. Ce certificat SSL doit contenir les éléments suivants:  
  
1.  Le nom du sujet et autre nom d’objet doivent contenir votre nom de service de fédération, par exemple, fs.contoso.com.  
  
2.  L’autre nom d’objet doit contenir la valeur **enterpriseregistration** qui est suivie par le suffixe de nom d’utilisateur Principal \(UPN\) de votre organisation, par exemple, **enterpriseregistration.corp.contoso.com**.  
  
    > [!WARNING]  
    > Spécifiez le nom alternatif d’objet si vous prévoyez d’activer la \(DRS\) Device Registration Service pour la jonction d’espace de travail.  
  
> [!IMPORTANT]  
> Si votre organisation utilise plusieurs suffixes UPN, et que vous prévoyez d’activer le service DRS, le certificat SSL doit contenir une entrée d’autre nom de l’objet pour chaque suffixe.  
  
## <a name="see-also"></a>Voir aussi
[Déploiement d’ADFS](../../ad-fs/AD-FS-Deployment.md)  

[Guide de déploiement de Windows Server2012R2 ADFS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Déploiement d’une batterie de serveurs de fédération](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  
  

