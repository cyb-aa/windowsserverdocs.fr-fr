---
ms.assetid: 09f335bb-896a-45dd-adc2-f215b8fba828
title: Conception SSO de Web fédéré
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9915a2942c9336d5aeb7776169d2e51491c22909
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853142"
---
# <a name="federated-web-sso-design"></a>Conception SSO de Web fédéré

Le\-de signature\-unique Web fédéré sur \(\) services ADFS \(AD FS\)\-des communications sécurisées qui s’étendent sur plusieurs pare-feu, réseaux de périmètre et serveurs de résolution de, en plus de la totalité de l’infrastructure de routage Internet.  
  
En règle générale, cette conception est utilisée lorsque deux organisations conviennent de créer une relation d’approbation de Fédération pour permettre aux utilisateurs d’une organisation \(l’organisation partenaire de compte\) d’accéder aux applications ou services Web\-, qui sont sécurisés par AD FS, dans l’autre organisation \(l’organisation partenaire de ressource\).  
  
En d’autres termes, une relation d’approbation de Fédération est l’incarnation d’un accord ou d’un partenariat de niveau\-professionnel entre deux organisations. Comme indiqué dans l’illustration suivante, vous pouvez établir une relation d’approbation de Fédération entre deux entreprises, ce qui aboutit à un\-final à\-scénario final de Fédération.  
  
![SSO Web fédéré](media/adfs2_FederatedWebSSODesign.gif)  
  
La flèche d’un\-moyen de l’illustration indique le sens de l’approbation de Fédération, qui, comme le sens des approbations Windows, pointe toujours vers le côté compte de la forêt. Cela signifie que le cheminement de l’authentification s’effectue de l’organisation partenaire de compte vers l’organisation partenaire de ressource.  
  
Dans cette conception SSO de Web fédéré, deux serveurs de Fédération \(un dans Fabrikam et l’autre dans contoso\) router les demandes d’authentification des comptes d’utilisateur de Fabrikam vers les applications ou services Web\-s dans contoso.  
  
> [!NOTE]  
> Pour renforcer la sécurité, vous pouvez utiliser des proxies de serveur de Fédération pour relayer les demandes aux serveurs de Fédération qui ne sont pas directement accessibles à partir d’Internet.  
  
Dans cet exemple, Fabrikam est le fournisseur d’identité ou de compte. La partie Fabrikam de la conception SSO de Web fédéré utilise les AD FS objectif de déploiement suivants :  
  
-   [Fournir à vos utilisateurs Active Directory un accès aux applications et services d’autres organisations](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
Contoso est le fournisseur de ressources. La partie contoso de la conception SSO de Web fédéré atteint les objectifs de déploiement AD FS suivants :  
  
-   [Fournir aux utilisateurs d’une autre organisation un accès à vos applications et services prenant en charge les revendications](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [Fournir à vos utilisateurs Active Directory un accès à vos applications et services prenant en charge les revendications](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
Pour obtenir une liste détaillée des tâches que vous pouvez utiliser pour planifier et déployer la conception de SSO de web fédéré, voir [Checklist: Implementing a Federated Web SSO Design](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
