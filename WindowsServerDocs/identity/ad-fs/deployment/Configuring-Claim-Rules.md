---
ms.assetid: 46dce9d4-7293-4b1c-9710-78b04f2e347a
title: Configuration de règles de revendication
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6494f584edd5f84a5987707953f79edbce15cc02
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814660"
---
# <a name="configuring-claim-rules"></a>Configuration de règles de revendication

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dans un revendications\-modèle d’identité basé sur, la fonction d’Active Directory Federation Services \(AD FS\) federation services consiste à émettre un jeton qui contient un ensemble de revendications. Règles de revendications régissent la décision en ce qui concerne des revendications AD FS émet. Les règles de revendication et toutes les données de configuration de serveur sont stockées dans la base de données de configuration AD FS.  
  
AD FS prend des décisions d’émission qui reposent sur les informations d’identité qui sont fournies à ce dernier sous la forme de revendications et d’autres informations contextuelles. À un niveau élevé, AD FS fonctionne qu’un processeur de règles en effectuant l’une des revendications en tant qu’entrée, effectue un certain nombre de transformations, puis retourne un ensemble différent de revendications en tant que sortie.  
  
-   [Créer une règle pour passer ou filtrer une revendication entrante](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [Créer une règle pour autoriser tous les utilisateurs](../../ad-fs/operations/Create-a-Rule-to-Permit-All-Users.md)  

-   [Créer une règle pour envoyer une revendication Compatible de AD FS 1.x](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)
  
-   [Créer une règle pour autoriser ou refuser des utilisateurs en fonction d’une revendication entrante](../../ad-fs/operations/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [Créer une règle pour envoyer les attributs LDAP en tant que revendications](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [Créer une règle Envoyer l’appartenance au groupe en tant que revendication](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [Créer une règle pour transformer une revendication entrante](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [Créer une règle pour envoyer une revendication de méthode d’authentification](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)  
  
-   [Créer une règle pour envoyer des revendications à l’aide d’une règle personnalisée](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule.md)  

## <a name="additional-references"></a>Références supplémentaires  

[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md)
