---
ms.assetid: 09f335bb-896a-45dd-adc2-f215b8fba828
title: "Conception SSO de Web fédéré"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b85f49ac0556bf9b3542a23514d7fcbf82d2d88e
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="federated-web-sso-design"></a>Conception SSO de Web fédéré

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

La conception \(SSO\) Single\-Sign\-On de Web fédéré dans ActiveDirectory Federation Services \(ADFS\) implique des communications sécurisées qui s’étend sur plusieurs pare-feux, réseaux de périmètre et serveurs de résolution de nom\ — en plus de l’infrastructure de routage Internet.  
  
En règle générale, cette conception est utilisée lorsque deux organisations conviennent de créer une relation d’approbation de fédération pour permettre aux utilisateurs d’une organisation \ (le compte partenaire organization\) pour accéder aux applications basées sur la console Web ou des services, qui sont sécurisés par ADFS, dans l’autre organisation \ (l’organization\ de partenaire de ressource).  
  
En d’autres termes, une relation d’approbation de fédération est l’incarnation d’un contrat de niveau de business\ ou d’un partenariat entre deux organisations. Comme indiqué dans l’illustration suivante, vous pouvez établir une relation d’approbation de fédération entre deux entreprises, ce qui se traduit dans un scénario de fédération end\ bout celle-ci.  
  
![Sso de web fédéré](media/adfs2_FederatedWebSSODesign.gif)  
  
La flèche one\ voies dans l’illustration indique le sens de la fédération confiance, ce qui, comme la direction des approbations Windows, pointe toujours vers le côté compte de la forêt. Cela signifie que l’authentification des flux à partir de l’organisation partenaire de compte vers l’organisation partenaire de ressource.  
  
Dans cette conception SSO de Web fédéré, deux serveurs de fédération \ (un dans Fabrikam et l’autre dans Contoso\) acheminer les demandes d’authentification à partir des comptes d’utilisateurs de Fabrikam aux applications basées sur la console Web ou des services de Contoso.  
  
> [!NOTE]  
> Pour renforcer la sécurité, vous pouvez utiliser des serveurs proxy de fédération pour relayer les demandes aux serveurs de fédération qui ne sont pas directement accessibles à partir d’Internet.  
  
Dans cet exemple, Fabrikam est le fournisseur d’identité ou de compte. La partie consacrée à Fabrikam de la conception SSO de Web fédéré utilise l’objectif de déploiement ADFS suivante:  
  
-   [Fournir un accès à vos utilisateurs Active Directory pour les Applications et Services d’autres organisations](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
Contoso est le fournisseur de ressources. La partie consacrée à Contoso de la conception SSO de Web fédéré atteint les objectifs de déploiement ADFS suivantes:  
  
-   [Fournir aux utilisateurs d’une autre organisation d’accéder à vos Services et Applications prenant en charge les revendications](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [Fournir vos utilisateurs Active Directory un accès à vos Services et Applications prenant en charge les revendications](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
Pour obtenir une liste détaillée des tâches que vous pouvez utiliser pour planifier et déployer la conception SSO de Web fédéré, voir [liste de vérification: implémentation d’une conception SSO de Web fédéré ](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
