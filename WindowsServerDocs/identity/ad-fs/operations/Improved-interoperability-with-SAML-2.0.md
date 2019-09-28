---
ms.assetid: 80b5335b-fa02-4944-900c-5fe4f5c6111d
title: Interopérabilité améliorée avec SAML 2.0
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d72636d77fe3240caab66dcab8657225d291bec6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407542"
---
# <a name="improved-interoperability-with-saml-20"></a>Interopérabilité améliorée avec SAML 2.0



  
AD FS dans Windows Server 2016 contient une prise en charge du protocole SAML supplémentaire, notamment la prise en charge de l’importation d’approbations basées sur des métadonnées contenant plusieurs entités.  Cela vous permet de configurer AD FS pour participer à des conversions telles que la Fédération inhabituelle et d’autres implémentations conformes à la norme eGov 2,0.   
  
La nouvelle fonctionnalité est basée sur des groupes d’approbations de partie de confiance ou de fournisseur de revendications. Chaque groupe est un élément EntitiesDescriptor (< MD : EntitiesDescriptor >) tel que spécifié dans le profil eGov 2,0, contenant un ou plusieurs éléments EntityDescriptor.  Les groupes ont des règles d’autorisation communes et toutes les autres propriétés peuvent être modifiées comme des objets d’approbation individuels.  
  
Une fois les groupes d’approbation importés dans AD FS, AD FS met automatiquement à jour les approbations en tant que groupe en fonction du document de métadonnées.  
  
L’activation de ces scénarios est aussi simple que l’utilisation des nouveaux applets PowerShell qui ajoutent et suppriment des objets AdfsClaimsProviderTrustsGroup et AdfsRelyingPartyTrustsGroup. Cela peut être fait à l’aide d’une URL de métadonnées ou d’un fichier, comme indiqué dans les exemples ci-dessous.  
  
En outre, AD FS 2016 prend en charge le paramètre d’étendue, comme décrit dans la spécification SAML Core, section 3.4.1.2. Cet élément permet aux parties de confiance de spécifier un ou plusieurs fournisseurs d’identité pour une demande d’authentification.  
  
## <a name="examples"></a>Exemples  
  
```  
Add-AdfsClaimsProviderTrustsGroup -MetadataUrl "https://www.contosoconsortium.com/metadata/metadata.xml"   
```  
  
  
  
```  
Add-AdfsClaimsProviderTrustsGroup -MetadataFile "C:\metadata.xml"   
```  
  
## <a name="references"></a>Références  
  
Le profil eGov 2,0 est disponible [ici.](https://kantarainitiative.org/confluence/download/attachments/60817482/kantara-report-egov-saml2-profile-2.0.pdf?version=1&modificationDate=1345580916000&api=v2)  
  
La spécification SAML Core est disponible [ici.](https://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf)   


