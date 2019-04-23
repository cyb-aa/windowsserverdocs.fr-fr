---
ms.assetid: 73897497-b189-4305-b234-e057ffda163a
title: Affectation de noms de domaine
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ba5a7ee8b7d9728c48e2798853aab8d55047e86f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866560"
---
# <a name="assigning-domain-names"></a>Affectation de noms de domaine

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous devez attribuer un nom à chaque domaine dans votre plan. Les domaines Active Directory domaine Services (AD DS) ont deux types de noms : Noms de domaine DNS (Name System) et les noms NetBIOS. En règle générale, les deux noms sont visibles aux utilisateurs finaux. Les noms DNS de domaines Active Directory incluent deux parties : un préfixe et un suffixe. Lorsque vous créez des noms de domaine, d’abord déterminer le préfixe DNS. Il s’agit de la première étiquette dans le nom DNS du domaine. Le suffixe est déterminé lorsque vous sélectionnez le nom de domaine racine de forêt. Le tableau suivant répertorie les règles de préfixe pour les noms DNS.  
  
|Règle|Explication|  
|--------|---------------|  
|Sélectionnez un préfixe qui n’est pas susceptible de devenir obsolètes.|Évitez les noms tels que d’une gamme de produits ou d’un système d’exploitation qui peut changer à l’avenir. Nous vous recommandons d’utiliser des noms géographiques.|  
|Sélectionnez un préfixe qui inclut uniquement les caractères standard Internet.|A-Z, a-z, 0-9 et (-), mais pas entièrement numérique.|  
|Comprendre 15 caractères ou moins dans le préfixe.|Si vous choisissez une longueur de préfixe de 15 caractères ou moins, le nom NetBIOS est le même que le préfixe.|  
  
Pour plus d’informations, consultez conventions d’affectation de noms dans Active Directory pour les ordinateurs, domaines, sites et unités d’organisation ([https://go.microsoft.com/fwlink/?LinkId=106629](https://go.microsoft.com/fwlink/?LinkId=106629)).  
  
> [!NOTE]  
>  Même si Dcpromo.exe dans Windows Server 2008 et Windows Server 2003 vous permet de créer un nom de domaine DNS à un seul intitulé, vous ne devez pas utiliser ce type de nom pour un domaine pour plusieurs raisons. Dans Windows Server 2008 R2, Dcpromo.exe ne vous permet pas de créer un nom DNS à un seul intitulé pour un domaine. Pour plus d’informations, consultez [ https://go.microsoft.com/fwlink/?LinkId=92467.](https://go.microsoft.com/fwlink/?LinkId=92467)   
  
Si le nom NetBIOS actuel du domaine ne convient pas représenter la région ou ne parvient pas à satisfaire le préfixe de règles d’affectation de noms, sélectionnez un nouveau préfixe. Dans ce cas, le nom NetBIOS du domaine est différent à partir du préfixe DNS du domaine.  
  
Pour chaque nouveau domaine que vous déployez, sélectionnez un préfixe qui est approprié pour la région et est conforme aux règles de préfixe. Nous recommandons que le nom NetBIOS du domaine soit le même que le préfixe DNS.  
  
Documentez le préfixe DNS et les noms NetBIOS que vous sélectionnez pour chaque domaine dans votre forêt. Vous pouvez ajouter les informations de nom NetBIOS et DNS à la feuille de calcul « Planification de domaine » que vous avez créé pour documenter votre plan pour les domaines de nouveaux et mis à niveau. Pour l’ouvrir, télécharger Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip à partir de la tâche aides pour Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) et ouvrez « Planification de domaine » (DSSLOGI_5.doc).  
  


