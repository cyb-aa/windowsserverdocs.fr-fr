---
ms.assetid: 73897497-b189-4305-b234-e057ffda163a
title: Attribution de noms de domaine
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 0d605a2f0d0b98a65848f94be9803122c4492a8b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822842"
---
# <a name="assigning-domain-names"></a>Attribution de noms de domaine

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous devez attribuer un nom à chaque domaine de votre plan. Les domaines de Active Directory Domain Services (AD DS) ont deux types de noms : les noms DNS (Domain Name System) et les noms NetBIOS. En général, les deux noms sont visibles par les utilisateurs finaux. Les noms DNS des domaines de Active Directory incluent deux parties : un préfixe et un suffixe. Lors de la création de noms de domaine, commencez par déterminer le préfixe DNS. Il s’agit de la première étiquette du nom DNS du domaine. Le suffixe est déterminé lorsque vous sélectionnez le nom du domaine racine de la forêt. Le tableau suivant répertorie les règles de nom de préfixe pour les noms DNS.  
  
|Règle|Explication|  
|--------|---------------|  
|Sélectionnez un préfixe qui n’est pas susceptible de devenir obsolète.|Évitez les noms tels qu’une gamme de produits ou un système d’exploitation qui pourraient changer à l’avenir. Nous vous recommandons d’utiliser des noms géographiques.|  
|Sélectionnez un préfixe qui contient uniquement des caractères Internet standard.|A-Z, a-z, 0-9 et (-), mais pas entièrement numériques.|  
|Incluez 15 caractères ou moins dans le préfixe.|Si vous choisissez une longueur de préfixe de 15 caractères ou moins, le nom NetBIOS est le même que le préfixe.|  
  
Pour plus d’informations, consultez conventions d’affectation de noms dans les Active Directory pour les ordinateurs, les domaines, les sites et les unités d’organisation ([https://go.microsoft.com/fwlink/?LinkId=106629](https://go.microsoft.com/fwlink/?LinkId=106629)).  
  
> [!NOTE]  
>  Bien que Dcpromo.exe sous Windows Server 2008 et Windows Server 2003 permette de créer un nom de domaine DNS en une partie, plusieurs raisons contre-indiquent cet emploi. Sous Windows Server 2008 R2, Dcpromo.exe ne permet pas de créer un nom de domaine DNS en une partie pour un domaine. Pour plus d’informations, consultez [https://go.microsoft.com/fwlink/?LinkId=92467.](https://go.microsoft.com/fwlink/?LinkId=92467)   
  
Si le nom NetBIOS actuel du domaine n’est pas approprié pour représenter la région ou ne satisfait pas aux règles de nom de préfixe, sélectionnez un nouveau préfixe. Dans ce cas, le nom NetBIOS du domaine est différent du préfixe DNS du domaine.  
  
Pour chaque nouveau domaine que vous déployez, sélectionnez un préfixe adapté à la région et répondant aux règles de nom de préfixe. Nous recommandons que le nom NetBIOS du domaine soit identique au préfixe DNS.  
  
Documentez le préfixe DNS et les noms NetBIOS que vous sélectionnez pour chaque domaine de votre forêt. Vous pouvez ajouter les informations de nom DNS et NetBIOS à la feuille de calcul « planification de domaine » que vous avez créée pour documenter votre plan pour les domaines nouveaux et mis à niveau. Pour l’ouvrir, téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip à partir des outils de gestion des travaux pour le kit de déploiement de Windows Server 2003 ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) et ouvrez « planification de domaine » (DSSLOGI_5. doc).  
  


