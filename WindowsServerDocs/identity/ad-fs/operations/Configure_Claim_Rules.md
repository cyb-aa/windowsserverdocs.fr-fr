---
ms.assetid: 20d48afc-2623-43e9-8ed9-aeb9a0505630
title: Configurer les règles de revendication
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b46e228f202eeae7f8cbcf4c1a6851686f905e48
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407634"
---
# <a name="configure-claim-rules"></a>Configurer les règles de revendication

Dans un modèle d’identité claims @ no__t-0based, la fonction de Services ADFS (AD FS) en tant que services de Fédération consiste à émettre un jeton qui contient un ensemble de revendications. Les règles de revendication régissent les décisions en ce qui concerne les revendications qui AD FS problèmes. Les règles de revendication et toutes les données de configuration du serveur sont stockées dans la base de données de configuration AD FS.  
  
AD FS prend des décisions d’émission basées sur les informations d’identité qui lui sont fournies sous la forme de revendications et d’autres informations contextuelles. À un niveau élevé, AD FS agit comme un processeur de règles en acceptant un ensemble de revendications comme entrée, effectue un certain nombre de transformations, puis retourne un ensemble différent de revendications en sortie. 

Les rubriques suivantes vous aideront à créer les règles que AD FS traitera : 
  
-   [Créer une règle pour transmettre ou filtrer une revendication entrante](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [Créer une règle pour autoriser tous les utilisateurs](../../ad-fs/operations/Create-a-Rule-to-Permit-All-Users.md)  
  
-   [Créer une règle pour autoriser ou refuser des utilisateurs en fonction d’une demande entrante](../../ad-fs/operations/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [Créer une règle pour envoyer des attributs LDAP en tant que revendications](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [Créer une règle pour envoyer l’appartenance à un groupe en tant que revendication](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [Créer une règle pour transformer une revendication entrante](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [Créer une règle pour envoyer une revendication de méthode d’authentification](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)  
  
-   [Créer une règle pour envoyer des revendications à l’aide d’une règle personnalisée](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-rule.md)  

## <a name="see-also"></a>Voir aussi  
[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
