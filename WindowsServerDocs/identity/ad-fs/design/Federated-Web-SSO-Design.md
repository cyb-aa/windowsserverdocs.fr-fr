---
ms.assetid: 09f335bb-896a-45dd-adc2-f215b8fba828
title: Conception SSO de Web fédéré
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f7454279f234f65136b9fe6649a6e96ea53e5d51
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191509"
---
# <a name="federated-web-sso-design"></a>Conception SSO de Web fédéré

L’unique Web fédérée\-connexion\-sur \(SSO\) conception dans Active Directory Federation Services \(AD FS\) implique des communications sécurisées qui s’étend sur plusieurs pare-feux, périmètre réseaux et le nom\-serveurs de résolution, en plus de l’infrastructure de routage Internet.  
  
En règle générale, cette conception est utilisée lorsque deux organisations conviennent créer une relation d’approbation de fédération pour permettre aux utilisateurs d’une organisation \(l’organisation partenaire de compte\) pour accéder à Web\-en fonction des applications ou services , qui sont sécurisé par AD FS, dans l’autre organisation \(l’organisation partenaire ressource\).  
  
En d’autres termes, une relation d’approbation de fédération est l’incarnation d’une entreprise\-contrat de niveau ou d’un partenariat entre deux organisations. Comme indiqué dans l’illustration suivante, vous pouvez établir une relation d’approbation de fédération entre deux entreprises, ce qui entraîne une fin\-à\-bout le scénario de fédération.  
  
![sso de web fédéré](media/adfs2_FederatedWebSSODesign.gif)  
  
Celui\-flèches dans l’illustration indique la direction de la fédération d’approbation, qui, comme la direction des approbations de Windows, pointe toujours vers le côté compte de la forêt. Cela signifie que le cheminement de l’authentification s’effectue de l’organisation partenaire de compte vers l’organisation partenaire de ressource.  
  
Dans cette conception SSO de Web fédéré, deux serveurs de fédération \(un dans Fabrikam et l’autre dans Contoso\) acheminer les demandes d’authentification à partir de comptes d’utilisateur dans Fabrikam sur le Web\-en fonction des applications ou services dans Contoso.  
  
> [!NOTE]  
> Pour renforcer la sécurité, vous pouvez utiliser des serveurs proxy de fédération pour relayer les demandes aux serveurs de fédération qui ne sont pas directement accessibles à partir d’Internet.  
  
Dans cet exemple, Fabrikam est le fournisseur d’identité ou de compte. La partie consacrée à Fabrikam de la conception SSO de Web fédéré utilise l’objectif de déploiement AD FS suivant :  
  
-   [Fournir à vos utilisateurs Active Directory un accès aux applications et services d’autres organisations](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
Contoso est le fournisseur de ressources. La partie de Contoso de la conception SSO de Web fédéré atteint les objectifs de déploiement AD FS suivants :  
  
-   [Fournir aux utilisateurs d’une autre organisation un accès à vos applications et services prenant en charge les revendications](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [Fournir à vos utilisateurs Active Directory un accès à vos applications et services prenant en charge les revendications](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
Pour obtenir la liste détaillée des tâches que vous pouvez utiliser pour planifier et déployer la conception SSO de Web fédéré, consultez [liste de vérification : Implémentation d’une conception SSO Web fédéré](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
