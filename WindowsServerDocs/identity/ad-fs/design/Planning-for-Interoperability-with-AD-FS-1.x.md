---
ms.assetid: 04b63d9f-e924-4146-9b1d-785ed8b4239c
title: Planification de l’interopérabilité avec AD FS 1.x
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7a1082b873f65a9f98b25425a392b2c62de8ca22
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191014"
---
# <a name="planning-for-interoperability-with-ad-fs-1x"></a>Planification de l’interopérabilité avec AD FS 1.x

Active Directory Federation Services \(AD FS\) des serveurs de fédération exécutant Windows Server® 2012 peuvent interagir avec les deux an AD FS 1.0 \(installé avec Windows Server 2003 R2\) fédération et un Service AD FS 1.1 \(installé avec Windows Server 2008 ou Windows Server 2008 R2\) Service de fédération. Toutes les combinaisons d’interopérabilité suivantes sont pris en charge :  
  
-   Les services AD FS 1. *x* Service de fédération peut envoyer une revendication qui peut être consommée par un Service de fédération AD FS dans Windows Server 2012. Pour plus d'informations, consultez [Liste de vérification : Configurer AD FS pour utiliser les revendications à partir d’AD FS 1.x](../../ad-fs/deployment/Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md).  
  
-   N’importe quel Service de fédération AD FS dans Windows Server 2012 peut envoyer un AD FS 1. *x*\-compatible revendication qui peut être consommée par an AD FS 1. *x* Service de fédération. Pour plus d'informations, consultez [Liste de vérification : Configurer AD FS pour envoyer des revendications à un Service de fédération AD FS 1.x](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md).  
  
-   N’importe quel Service de fédération AD FS dans Windows Server 2012 peut envoyer un AD FS 1. *x*\-revendication compatible avec qui peut être utilisée par un ou plusieurs serveurs Web exécutant AD FS 1. *x* revendications\-agent Web prenant en charge. Pour plus d'informations, consultez [Liste de vérification : Configurer AD FS pour envoyer des revendications à un Agent de Web prenant en charge les revendications AD FS 1.x](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md).  
  
> [!NOTE]  
> AD FS ne pas prendre en charge ou n’interagit pas avec les services AD FS 1. *x* agent Web de jeton Windows NT.  
  
An AD FS 1. *x*\-revendication compatible avec est une revendication qui peut être envoyée par un Service de fédération AD FS dans Windows Server 2012 et comprise par an AD FS 1. *x* Service de fédération. Afin qu’an AD FS 1. *x* Service de fédération puisse consommer les revendications qui envoie d’un Service de fédération AD FS, et un identificateur de nom \(ID\) revendication doit être envoyée.  
  
## <a name="understanding-the-nameid-claim-type"></a>Comprendre le type de revendication d’identificateur de nom  
Le type de revendication d’identificateur de nom correspond au type de revendication d’identité utilisé par AD FS 1.*x*. Il doit être utilisé lorsque vous souhaitez interagir avec AD FS 1.*x*. L’ID de nom type de revendication permet soit an AD FS 1. *x* Service de fédération ou AD FS 1. *x* revendications\-agent Web prenant en charge d’utiliser les revendications qui envoie des services AD FS dans Windows Server 2012, tant que ces revendications sont envoyées dans un des formats de nom ID dans le tableau suivant.  
  
|Format d’identificateur de nom|URI correspondant|  
|------------------|---------------------|  
|Adresse de messagerie AD FS 1.*x*|http://schemas.xmlsoap.org/claims/EmailAddress|  
|UPN de messagerie AD FS 1.*x*|http://schemas.xmlsoap.org/claims/UPN|  
|Nom commun|http://schemas.xmlsoap.org/claims/CommonName|  
|Regrouper|http://schemas.xmlsoap.org/claims/Group|  
  
Une seule revendication d’identificateur de nom ID dans le format approprié doit être envoyée. Lorsque ce critère est rempli, plusieurs autres revendications peuvent être envoyées, en supposant qu’elles sont conformes aux restrictions décrites dans le tableau.  
  
> [!NOTE]  
> An AD FS 1. *x* Service de fédération peut interpréter les types de revendications entrantes uniquement qui commencent par l’identificateur de ressource uniforme \(URI\) de http://schemas.xmlsoap.org/claims/.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
