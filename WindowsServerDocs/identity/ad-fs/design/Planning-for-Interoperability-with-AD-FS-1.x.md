---
ms.assetid: 04b63d9f-e924-4146-9b1d-785ed8b4239c
title: Planification de l’interopérabilité avec AD FS 1.x
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e9f72bd83c90a804749329521a72e3232589c735
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407969"
---
# <a name="planning-for-interoperability-with-ad-fs-1x"></a>Planification de l’interopérabilité avec AD FS 1.x

Les serveurs de Fédération Services ADFS \(AD FS @ no__t-1 exécutant Windows Server® 2012 peuvent interagir avec un AD FS 1,0 \(installed avec Windows Server 2003 R2 @ no__t-3 service FS (Federation Service) et un AD FS 1,1 \(installed avec Windows Server 2008 ou Windows Server 2008 R2 @ no__t-5 service FS (Federation Service). Toutes les combinaisons d’interopérabilité suivantes sont pris en charge :  

-   Tout AD FS 1. *x* service FS (Federation Service) pouvez envoyer une revendication qui peut être consommée par un service FS (Federation Service) AD FS dans Windows Server 2012. Pour plus d'informations, consultez [Liste de vérification : Configuration de AD FS pour consommer des revendications à partir de AD FS 1. x @ no__t-0.  

-   Tout AD FS service FS (Federation Service) dans Windows Server 2012 peut envoyer un AD FS 1. revendication *x*\-Compatible qui peut être consommée par un AD FS 1. service FS (Federation Service) *x* . Pour plus d'informations, consultez [Liste de vérification : Configuration de AD FS pour envoyer des revendications à un AD FS 1. x service FS (Federation Service) @ no__t-0.  

-   Tout AD FS service FS (Federation Service) dans Windows Server 2012 peut envoyer un AD FS 1. la revendication *x*\-Compatible qui peut être consommée par un ou plusieurs serveurs Web exécutant l’AD FS 1. *x* claims @ no__t-3aware Web agent. Pour plus d'informations, consultez [Liste de vérification : Configuration de AD FS pour envoyer des revendications à un AD FS 1. x agent Web prenant en charge les revendications @ no__t-0.  

> [!NOTE]  
> AD FS ne prend pas en charge ou n’interagit pas avec le AD FS 1. agent Web *x* basé sur un jeton Windows NT.  

AD FS 1. la revendication *x*\-Compatible est une revendication qui peut être envoyée par une AD FS service FS (Federation Service) dans Windows Server 2012 et comprise par un AD FS 1. service FS (Federation Service) *x* . Pour qu’une AD FS 1. *x* service FS (Federation Service) peut consommer les revendications envoyées par un AD FS service FS (Federation Service), un identificateur de nom \(ID @ no__t-2 doit être envoyé.  

## <a name="understanding-the-name-id-claim-type"></a>Comprendre le type de revendication d’identificateur de nom  
Le type de revendication d’identificateur de nom correspond au type de revendication d’identité utilisé par AD FS 1.*x* . Il doit être utilisé lorsque vous souhaitez interagir avec AD FS 1.*x*. Le type de revendication ID de nom active un AD FS 1. *x* service FS (Federation Service) ou le AD FS 1. *x* claims @ no__t-2Aware agent Web pour consommer les revendications que AD FS dans Windows Server 2012 envoie, tant que ces revendications sont envoyées dans l’un des formats d’ID de nom répertoriés dans le tableau suivant.  


|      Format d’identificateur de nom       |               URI correspondant                |
|---------------------------|------------------------------------------------|
| Adresse de messagerie AD FS 1.*x* | http://schemas.xmlsoap.org/claims/EmailAddress |
|   UPN de messagerie AD FS 1.*x*   |     http://schemas.xmlsoap.org/claims/UPN      |
|        Nom commun        |  http://schemas.xmlsoap.org/claims/CommonName  |
|           Regrouper           |    http://schemas.xmlsoap.org/claims/Group     |

Une seule revendication d’identificateur de nom ID dans le format approprié doit être envoyée. Lorsque ce critère est rempli, plusieurs autres revendications peuvent être envoyées, en supposant qu’elles sont conformes aux restrictions décrites dans le tableau.  

> [!NOTE]  
> AD FS 1. *x* service FS (Federation Service) pouvez interpréter uniquement les types de revendications entrantes qui commencent par le Uniform Resource Identifier \(URI @ no__t-2 de http://schemas.xmlsoap.org/claims/.  

## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
