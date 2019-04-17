---
ms.assetid: 20d48afc-2623-43e9-8ed9-aeb9a0505630
title: "Configurer des règles de revendications"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 259e2b266b64a3b34c237cfe209a3558124c8ef2
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="configure-claim-rules"></a>Configurer des règles de revendication

>S’applique à: Windows Server2016, Windows Server2012R2

Dans un modèle d’identité basée sur les claims\, la fonction d’Active Directory Federation Services (ADFS) en tant que les services de fédération consiste à émettre un jeton qui contient un ensemble de revendications. Les règles de revendication régissent les décisions en ce qui concerne les revendications qui émet des services AD FS. Les règles de revendication et toutes les données de configuration du serveur sont stockées dans la base de données de configuration ADFS.  
  
ADFS prend les décisions d’émission qui sont basées sur les informations d’identité qui sont fournies à sous la forme de revendications et d’autres informations contextuelles. À un niveau élevé, ADFS fonctionne qu’un processeur de règles en effectuant l’une des revendications en entrée, effectue un certain nombre de transformations, puis retourne un ensemble différent de revendications en tant que sortie. 

Les rubriques suivantes vous aideront à la création de règles qui traite les services AD FS: 
  
-   [Créer une règle pour transmettre ou filtrer une revendication entrante](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [Créer une règle pour autoriser tous les utilisateurs](../../ad-fs/operations/Create-a-Rule-to-Permit-All-Users.md)  
  
-   [Créer une règle pour autoriser ou refuser les utilisateurs selon une revendication entrante](../../ad-fs/operations/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [Créer une règle pour envoyer les attributs LDAP en tant que revendications](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [Créer une règle à un groupe d’envoi de l’appartenance comme une revendication](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [Créer une règle pour transformer une revendication entrante](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [Créer une règle pour envoyer une revendication de méthode d’authentification](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)  
  
-   [Créer une règle pour envoyer des revendications à l’aide d’une règle personnalisée](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-rule.md)  

## <a name="see-also"></a>Voir aussi  
[Opérations ADFS](../../ad-fs/AD-FS-2016-Operations.md) 
