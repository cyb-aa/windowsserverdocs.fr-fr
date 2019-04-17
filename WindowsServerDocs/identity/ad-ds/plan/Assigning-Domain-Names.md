---
ms.assetid: 73897497-b189-4305-b234-e057ffda163a
title: Affectation de noms de domaine
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0d5ec9b76798f21503f650527a7961cff0b592b4
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="assigning-domain-names"></a>Affectation de noms de domaine

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Vous devez attribuer un nom pour chaque domaine dans votre plan. ActiveDirectory (ADDS) domaines ont deux types de noms: noms du système DNS (Domain Name) et NetBIOS. En règle générale, les deux noms sont visibles pour les utilisateurs finaux. Les noms DNS de domaines ActiveDirectory comprennent deux parties, un préfixe et un suffixe. Lorsque vous créez des noms de domaine, tout d’abord déterminer le préfixe DNS. Il s’agit de la première étiquette dans le nom DNS du domaine. Le suffixe est déterminé lorsque vous sélectionnez le nom du domaine racine de forêt. Le tableau suivant répertorie le préfixe de règles d’affectation de noms pour les noms DNS.  
  
|Règle|Explication|  
|--------|---------------|  
|Sélectionnez un préfixe qui n’est pas susceptible de devenir obsolètes.|Évitez les noms comme une gamme de produits ou le système d’exploitation qui peuvent être modifiées à l’avenir. Nous vous recommandons d’utiliser des noms géographiques.|  
|Sélectionnez un préfixe qui inclut uniquement des caractères standards d’Internet.|0-9 A-Z, a-z et (-), mais pas entièrement numérique.|  
|Comprendre 15caractères ou moins dans le préfixe.|Si vous choisissez une longueur de préfixe de moins de 15caractères, le nom NetBIOS est le même que le préfixe.|  
  
Pour plus d’informations, voir les conventions d’affectation de noms dans ActiveDirectory pour les ordinateurs, domaines, sites et unités d’organisation ([https://go.microsoft.com/fwlink/?LinkId=106629](https://go.microsoft.com/fwlink/?LinkId=106629)).  
  
> [!NOTE]  
>  Même si Dcpromo.exe dans Windows Server2008 et Windows Server2003vous permet de créer un nom de domaine DNS en une partie, vous ne devez pas utiliser un nom DNS en une partie d’un domaine pour plusieurs raisons. Dans Windows Server2008R2, Dcpromo.exe n’autorise pas la création d’un nom DNS en une partie d’un domaine. Pour plus d’informations, voir [https://go.microsoft.com/fwlink/?LinkId=92467.](https://go.microsoft.com/fwlink/?LinkId=92467)   
  
Si le nom NetBIOS actuel du domaine n’est pas approprié représenter la région ou ne parvient pas à satisfaire le préfixe de règles d’affectation de noms, sélectionnez un nouveau préfixe. Dans ce cas, le nom NetBIOS du domaine est différent du préfixe du DNS du domaine.  
  
Pour chaque nouveau domaine que vous déployez, sélectionnez un préfixe qui est appropriée pour la région et qui satisfait aux règles d’affectation de noms de préfixe. Nous recommandons que le nom NetBIOS du domaine être le même que le préfixe DNS.  
  
Documentez le préfixe DNS et les noms NetBIOS que vous sélectionnez pour chaque domaine dans votre forêt. Vous pouvez ajouter les informations de nom DNS et NetBIOS à la feuille de calcul «La planification du domaine» que vous avez créé pour documenter votre plan de nouveaux et mis à niveau des domaines. Pour l’ouvrir, télécharger Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip à partir de la tâche des identificateurs d’applet pour Windows Server2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) et ouvrez "Planification domaine «(DSSLOGI_5.doc).  
  


