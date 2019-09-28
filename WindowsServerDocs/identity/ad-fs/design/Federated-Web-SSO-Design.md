---
ms.assetid: 09f335bb-896a-45dd-adc2-f215b8fba828
title: Conception SSO de Web fédéré
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6a3e7eb6c42c8190da799c88c1e947e6aef1c29f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408102"
---
# <a name="federated-web-sso-design"></a>Conception SSO de Web fédéré

La conception unique Web fédérée @ no__t-0Sign @ no__t-1On \(SSO @ no__t-3 dans Services ADFS \(AD FS @ no__t-5 implique des communications sécurisées qui s’étendent sur plusieurs pare-feu, réseaux de périmètre et nom @ no__t-6resolution serveurs : en plus de l’ensemble de l’infrastructure de routage Internet.  
  
En règle générale, cette conception est utilisée lorsque deux organisations conviennent de créer une relation d’approbation de Fédération pour permettre aux utilisateurs d’une organisation \(the-organisation partenaire de compte @ no__t-1 d’accéder aux applications ou services Web @ no__t-2based, qui sont sécurisés par AD FS, dans l’autre organisation \(La organisation du partenaire de ressource @ no__t-4.  
  
En d’autres termes, une relation d’approbation de Fédération est l’incarnation d’un accord Business @ no__t-0level ou d’un partenariat entre deux organisations. Comme indiqué dans l’illustration suivante, vous pouvez établir une relation d’approbation de Fédération entre deux entreprises, ce qui se traduit par un scénario de Fédération de fin @ no__t-0to @ no__t-1fin.  
  
![SSO Web fédéré](media/adfs2_FederatedWebSSODesign.gif)  
  
La flèche un @ no__t-0way dans l’illustration indique la direction de l’approbation de Fédération, qui, comme la direction des approbations Windows, pointe toujours vers le côté compte de la forêt. Cela signifie que le cheminement de l’authentification s’effectue de l’organisation partenaire de compte vers l’organisation partenaire de ressource.  
  
Dans cette conception SSO de Web fédéré, deux serveurs de Fédération \(one dans Fabrikam et l’autre dans contoso @ no__t-1 acheminent les demandes d’authentification des comptes d’utilisateur de Fabrikam vers les applications ou services Web @ no__t-2based dans contoso.  
  
> [!NOTE]  
> Pour renforcer la sécurité, vous pouvez utiliser des proxies de serveur de Fédération pour relayer les demandes aux serveurs de Fédération qui ne sont pas directement accessibles à partir d’Internet.  
  
Dans cet exemple, Fabrikam est le fournisseur d’identité ou de compte. La partie Fabrikam de la conception SSO de Web fédéré utilise les AD FS objectif de déploiement suivants :  
  
-   [Fournir à vos utilisateurs Active Directory un accès aux applications et services d’autres organisations](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
Contoso est le fournisseur de ressources. La partie contoso de la conception SSO de Web fédéré atteint les objectifs de déploiement AD FS suivants :  
  
-   [Fournir aux utilisateurs d’une autre organisation un accès à vos applications et services prenant en charge les revendications](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [Fournir à vos utilisateurs Active Directory un accès à vos applications et services prenant en charge les revendications](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
Pour obtenir la liste des tâches détaillées que vous pouvez utiliser pour planifier et déployer la conception SSO de Web fédéré, voir [Checklist : Implémentation d’une conception SSO de Web fédéré @ no__t-0.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
