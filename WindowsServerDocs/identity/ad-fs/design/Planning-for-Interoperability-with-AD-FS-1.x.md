---
ms.assetid: 04b63d9f-e924-4146-9b1d-785ed8b4239c
title: "Planification de l’interopérabilité avec ADFS 1.x"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f287261ce6cb56e40385ef4de922045153819a23
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="planning-for-interoperability-with-ad-fs-1x"></a>Planification de l’interopérabilité avec ADFS 1.x

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

ActiveDirectory Federation Services \(ADFS\) fédération serveurs exécutant Windows Server® 2012 peuvent interagir avec les deux an ADFS 1.0 \ (installé avec Windows Server2003R2\) Service de fédération et une ADFS 1.1 \ (installé avec Windows Server2008 ou Windows Server2008R2\) Service de fédération. Une des combinaisons d’interopérabilité suivantes sont prises en charge:  
  
-   Les services ADFS 1. *x* Service de fédération peut envoyer une revendication qui peut être consommée par un Service de fédération ADFS dans Windows Server2012. Pour plus d’informations, voir [liste de vérification: configuration d’ADFS pour consommer les revendications à partir d’ADFS 1.x](../../ad-fs/deployment/Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md).  
  
-   N’importe quel Service de fédération ADFS dans Windows Server2012 peut envoyer un ADFS 1. *x*\-compatible revendication pouvant être consommée par un ADFS 1. *x* Service de fédération. Pour plus d’informations, voir [liste de vérification: Configuring ADFS to Send Claims to an ADFS 1.x Federation Service](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md).  
  
-   N’importe quel Service de fédération ADFS dans Windows Server2012 peut envoyer un ADFS 1. *x*\-compatible revendication pouvant être consommée par un ou plusieurs serveurs Web exécutant ADFS 1. *x* l’agent Web prenant en charge claims\. Pour plus d’informations, voir [liste de vérification: Configuring ADFS to Send Claims to an ADFS 1.x Claims-Aware Web Agent](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md).  
  
> [!NOTE]  
> ADFS ne pas prendre en charge ou interagir avec ADFS 1. *x* agent Web de jeton Windows NT.  
  
Un ADFS 1. *x*\-compatible revendication est une revendication qui peut être envoyée par un Service de fédération ADFS dans Windows Server2012 et comprise par an ADFS 1. *x* Service de fédération. Afin qu’un ADFS 1. *x* Service de fédération puisse consommer les revendications qui envoie un Service de fédération ADFS, un type de revendication d’identificateur de nom \(ID\) doit être envoyé.  
  
## <a name="understanding-the-name-id-claim-type"></a>Comprendre le type de revendication d’identificateur de nom  
Le type de revendication d’identificateur de nom est l’équivalent de la revendication d’identité tapez par ADFS 1. *x* uses. Il doit être utilisé lorsque vous souhaitez interagir avec ADFS 1. *x*. La revendication d’identificateur de nom tapez permet soit an ADFS 1. *x* Service de fédération ou ADFS 1. *x* agent Web prenant en claims\ d’utiliser les revendications qui envoie des services ADFS dans Windows Server2012, tant que ces revendications sont envoyées dans un des formats de nom ID dans le tableau suivant.  
  
|Format du nom de code|URI correspondant|  
|------------------|---------------------|  
|ADFS 1. *x* adresse de messagerie|http://schemas.xmlsoap.org/claims/EmailAddress|  
|ADFS 1. *x* UPN de messagerie|http://schemas.xmlsoap.org/claims/UPN|  
|Nom commun|http://schemas.xmlsoap.org/claims/CommonName|  
|Groupe|http://schemas.xmlsoap.org/claims/Group|  
  
Une seule revendication d’identificateur de nom dans le format approprié doit être envoyée. Lorsque ce critère est rempli, plusieurs autres revendications peuvent être envoyées, en supposant qu’elles sont conformes aux restrictions décrites dans le tableau.  
  
> [!NOTE]  
> Un ADFS 1. *x* Service de fédération peut interpréter les types de revendications uniquement entrantes qui commencent par l’identificateur de ressource uniforme \(URI\) de http://schemas.xmlsoap.org/claims/.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
