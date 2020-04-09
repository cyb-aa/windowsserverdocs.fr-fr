---
ms.assetid: 04b63d9f-e924-4146-9b1d-785ed8b4239c
title: Planification de l’interopérabilité avec AD FS 1.x
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a0bbf64a7bf110e3d73084dd047c84b2b83be8d9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858612"
---
# <a name="planning-for-interoperability-with-ad-fs-1x"></a>Planification de l’interopérabilité avec AD FS 1.x

Services ADFS \(AD FS\) les serveurs de Fédération exécutant Windows Server&reg; 2012 peuvent interagir avec une AD FS 1,0 \(installée avec Windows Server 2003 R2\) service FS (Federation Service) et une AD FS 1,1 \(installée avec Windows Server 2008 ou Windows Server 2008 R2\) service FS (Federation Service). Toutes les combinaisons d’interopérabilité suivantes sont pris en charge :  

-   Tout AD FS 1. *x* service FS (Federation Service) pouvez envoyer une revendication qui peut être consommée par un service FS (Federation Service) AD FS dans Windows Server 2012. Pour plus d’informations, consultez [liste de vérification : configuration de AD FS pour consommer des revendications à partir de AD FS 1. x](../../ad-fs/deployment/Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md).  

-   Tout AD FS service FS (Federation Service) dans Windows Server 2012 peut envoyer un AD FS 1. *x*\-revendication compatible qui peut être consommée par un AD FS 1. service FS (Federation Service) *x* . Pour plus d'informations, voir [Checklist: Configuring AD FS to Send Claims to an AD FS 1.x Federation Service](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md).  

-   Tout AD FS service FS (Federation Service) dans Windows Server 2012 peut envoyer un AD FS 1. *x*\-revendication compatible qui peut être consommée par un ou plusieurs serveurs Web exécutant l’AD FS 1. *x* revendications\-l’agent Web prenant en charge. Pour plus d'informations, voir [Checklist: Configuring AD FS to Send Claims to an AD FS 1.x Claims-Aware Web Agent](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md).  

> [!NOTE]  
> AD FS ne prend pas en charge ou n’interagit pas avec le AD FS 1. agent Web *x* basé sur un jeton Windows NT.  

AD FS 1. *x*\-revendication compatible est une revendication qui peut être envoyée par une AD FS service FS (Federation Service) dans Windows Server 2012 et comprise par un AD FS 1. service FS (Federation Service) *x* . Pour qu’une AD FS 1. *x* service FS (Federation Service) peut consommer les revendications envoyées par un AD FS service FS (Federation Service), un identificateur de nom \(ID\) type de revendication doit être envoyé.  

## <a name="understanding-the-name-id-claim-type"></a>Comprendre le type de revendication d’identificateur de nom  
Le type de revendication d’identificateur de nom correspond au type de revendication d’identité utilisé par AD FS 1.*x* . Il doit être utilisé lorsque vous souhaitez interagir avec AD FS 1.*x*. Le type de revendication ID de nom active un AD FS 1. *x* service FS (Federation Service) ou le AD FS 1. *x* affirme\-agent Web prenant en charge les revendications que AD FS dans Windows Server 2012 envoie, tant que ces revendications sont envoyées dans l’un des formats d’ID de nom figurant dans le tableau suivant.  


|      Format d’identificateur de nom       |               URI correspondant                |
|---------------------------|------------------------------------------------|
| Adresse de messagerie AD FS 1.*x* | http://schemas.xmlsoap.org/claims/EmailAddress |
|   UPN de messagerie AD FS 1.*x*   |     http://schemas.xmlsoap.org/claims/UPN      |
|        Nom commun        |  http://schemas.xmlsoap.org/claims/CommonName  |
|           Groupe           |    http://schemas.xmlsoap.org/claims/Group     |

Une seule revendication d’identificateur de nom ID dans le format approprié doit être envoyée. Lorsque ce critère est rempli, plusieurs autres revendications peuvent être envoyées, en supposant qu’elles sont conformes aux restrictions décrites dans le tableau.  

> [!NOTE]  
> AD FS 1. *x* service FS (Federation Service) pouvez interpréter uniquement les types de revendications entrantes qui commencent par le Uniform Resource Identifier \(URI\) de http://schemas.xmlsoap.org/claims/.  

## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
