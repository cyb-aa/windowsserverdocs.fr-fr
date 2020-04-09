---
ms.assetid: 9cafa3e1-8118-4a75-a7c2-1dbe40b1a444
title: Configurer les règles de revendication
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 7051040feb023ec9ab647d6e368136fb04b7db34
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817142"
---
# <a name="configure-claim-rules"></a>Configurer les règles de revendication

Dans un modèle d’identité basé sur des revendications\-, la fonction de Services ADFS \(AD FS\) en tant que services de Fédération consiste à émettre un jeton qui contient un ensemble de revendications. Les règles de revendication régissent les décisions en ce qui concerne les revendications qui AD FS problèmes. Les règles de revendication et toutes les données de configuration du serveur sont stockées dans la base de données de configuration AD FS.  
  
AD FS prend des décisions d’émission basées sur les informations d’identité qui lui sont fournies sous la forme de revendications et d’autres informations contextuelles. À un niveau élevé, AD FS agit comme un processeur de règles en acceptant un ensemble de revendications comme entrée, effectue un certain nombre de transformations, puis retourne un ensemble différent de revendications en sortie. 

Les rubriques suivantes vous aideront à créer les règles que AD FS traitera : 
  
-   [Créer une règle pour transmettre ou filtrer une revendication entrante](Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [Créer une règle pour autoriser tous les utilisateurs](Create-a-Rule-to-Permit-All-Users.md)  
  
-   [Créer une règle pour autoriser ou refuser des utilisateurs en fonction d’une demande entrante](Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [Créer une règle pour envoyer des attributs LDAP en tant que revendications](Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [Créer une règle pour envoyer l’appartenance à un groupe en tant que revendication](Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [Créer une règle pour transformer une revendication entrante](Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [Créer une règle pour envoyer une revendication de méthode d’authentification](Create-a-Rule-to-Send-an-Authentication-Method-Claim.md) 
-   [Créer une règle pour envoyer une revendication compatible AD FS 1. x](Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md) 
  
-   [Créer une règle pour envoyer des revendications à l’aide d’une règle personnalisée](Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule.md)  

## <a name="see-also"></a>Voir aussi  
[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
