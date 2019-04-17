---
ms.assetid: 80b5335b-fa02-4944-900c-5fe4f5c6111d
title: "Interopérabilité améliorée avec SAML 2.0"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4f55eaacec8ee0eb41e1980f1aa15c6256f8b979
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="improved-interoperability-with-saml-20"></a>Interopérabilité améliorée avec SAML 2.0

>S’applique à: Windows Server2016

  
ADFS dans Windows Server2016 contient SAML protocole prise en charge supplémentaire, y compris la prise en charge pour l’importation des approbations de fonction de métadonnées qui contient plusieurs entités.  Cela vous permet de configurer ADFS pour participer confédérations de l’industrie telles que la fédération InCommon et d’autres implémentations conformes à la norme 2.0 eGov.   
  
La nouvelle fonctionnalité est basée sur des groupes de partie de confiance ou des approbations de fournisseur de revendications. Chaque groupe est un élément de EntitiesDescriptor (< md:EntitiesDescriptor >) comme spécifié dans l’eGov profil 2.0, qui contient un ou plusieurs éléments EntityDescriptor.  Les groupes ont des règles d’autorisation communes, et toutes les autres propriétés peuvent être modifiées comme objets d’approbation individuels.  
  
Une fois les groupes d’approbation sont importés dans ADFS, ADFS met automatiquement à jour les relations d’approbation en tant que groupe basé sur le document de métadonnées.  
  
L’activation de ces scénarios est aussi simple que les nouvelles applets de commande PowerShell qui ajouter et supprimer des AdfsClaimsProviderTrustsGroup et AdfsRelyingPartyTrustsGroup des objets. Cela est possible à l’aide d’une URL de métadonnées ou d’un fichier, comme illustré dans les exemples ci-dessous.  
  
En outre, ADFS2016 prend en charge le paramètre d’étendue, comme décrit dans la spécification Core SAML, section3.4.1.2. Cet élément permet de partie de confiance parties pour spécifier un ou plusieurs fournisseurs d’identité pour une authentification de la demande.  
  
## <a name="examples"></a>Exemples  
  
```  
Add-AdfsClaimsProviderTrustsGroup -MetadataUrl "https://www.contosoconsortium.com/metadata/metadata.xml"   
```  
  
  
  
```  
Add-AdfsClaimsProviderTrustsGroup -MetadataFile "C:\metadata.xml"   
```  
  
## <a name="references"></a>Références  
  
Vous pouvez trouver le profil eGov 2.0 [ici.](https://kantarainitiative.org/confluence/download/attachments/60817482/kantara-report-egov-saml2-profile-2.0.pdf?version=1&modificationDate=1345580916000&api=v2)  
  
La spécification SAML Core trouverez [ici.](https://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf)   


