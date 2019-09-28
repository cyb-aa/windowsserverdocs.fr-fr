---
ms.assetid: 73897497-b189-4305-b234-e057ffda163a
title: Affectation de noms de domaine
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 357c136f108c6d8e9e2a15dd9449ab61663079e2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408995"
---
# <a name="assigning-domain-names"></a>Affectation de noms de domaine

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous devez attribuer un nom à chaque domaine de votre plan. Les domaines de Active Directory Domain Services (AD DS) ont deux types de noms : Noms DNS (Domain Name System) et noms NetBIOS. En général, les deux noms sont visibles par les utilisateurs finaux. Les noms DNS des domaines de Active Directory incluent deux parties : un préfixe et un suffixe. Lors de la création de noms de domaine, commencez par déterminer le préfixe DNS. Il s’agit de la première étiquette du nom DNS du domaine. Le suffixe est déterminé lorsque vous sélectionnez le nom du domaine racine de la forêt. Le tableau suivant répertorie les règles de nom de préfixe pour les noms DNS.  
  
|Règle|Explication|  
|--------|---------------|  
|Sélectionnez un préfixe qui n’est pas susceptible de devenir obsolète.|Évitez les noms tels qu’une gamme de produits ou un système d’exploitation qui pourraient changer à l’avenir. Nous vous recommandons d’utiliser des noms géographiques.|  
|Sélectionnez un préfixe qui contient uniquement des caractères Internet standard.|A-Z, a-z, 0-9 et (-), mais pas entièrement numériques.|  
|Incluez 15 caractères ou moins dans le préfixe.|Si vous choisissez une longueur de préfixe de 15 caractères ou moins, le nom NetBIOS est le même que le préfixe.|  
  
Pour plus d’informations, consultez conventions d’affectation de noms dans les Active Directory pour les ordinateurs, les domaines, les sites et les unités d’organisation ([https://go.microsoft.com/fwlink/?LinkId=106629](https://go.microsoft.com/fwlink/?LinkId=106629)).  
  
> [!NOTE]  
>  Même si Dcpromo.exe dans Windows Server 2008 et Windows Server 2003 vous permet de créer un nom de domaine DNS à un seul intitulé, vous ne devez pas utiliser ce type de nom pour un domaine pour plusieurs raisons. Dans Windows Server 2008 R2, Dcpromo.exe ne vous permet pas de créer un nom DNS à un seul intitulé pour un domaine. Pour plus d’informations, consultez [https://go.microsoft.com/fwlink/?LinkId=92467.](https://go.microsoft.com/fwlink/?LinkId=92467)   
  
Si le nom NetBIOS actuel du domaine n’est pas approprié pour représenter la région ou ne satisfait pas aux règles de nom de préfixe, sélectionnez un nouveau préfixe. Dans ce cas, le nom NetBIOS du domaine est différent du préfixe DNS du domaine.  
  
Pour chaque nouveau domaine que vous déployez, sélectionnez un préfixe adapté à la région et répondant aux règles de nom de préfixe. Nous recommandons que le nom NetBIOS du domaine soit identique au préfixe DNS.  
  
Documentez le préfixe DNS et les noms NetBIOS que vous sélectionnez pour chaque domaine de votre forêt. Vous pouvez ajouter les informations de nom DNS et NetBIOS à la feuille de calcul « planification de domaine » que vous avez créée pour documenter votre plan pour les domaines nouveaux et mis à niveau. Pour l’ouvrir, téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip à partir des outils de gestion des travaux pour Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) et ouvrez « Domain Planning » (DSSLOGI_5. doc).  
  


