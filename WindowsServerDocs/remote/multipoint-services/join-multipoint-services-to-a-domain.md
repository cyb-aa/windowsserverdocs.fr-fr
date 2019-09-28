---
title: Joindre MultiPoint services à un domaine (facultatif)
Description: Fournit les étapes permettant de joindre MultiPoint services à votre domaine
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 623b7c21-dcbb-402e-8b5a-8e434cd225bd
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 79bd9e594f94c7b3acd06265891dd646b3853b50
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394741"
---
# <a name="join-the-multipoint-services-computer-to-a-domain-optional"></a>Joindre l’ordinateur MultiPoint services à un domaine (facultatif)
Si vous accédez à votre ordinateur MultiPoint services sur un domaine Active Directory, l’étape suivante consiste à ajouter l’ordinateur au domaine.  
  
> [!IMPORTANT]  
> Vous devez vérifier votre fuseau horaire avant de joindre l’ordinateur à un domaine. Pour obtenir des instructions, consultez [définir la date, l’heure et le fuseau horaire](Set-the-date--time--and-time-zone.md).  
   
1.  À partir de l’écran d’**accueil**, ouvrez le **Panneau de configuration**. Cliquez sur **système et sécurité**, puis sur **système**.  
  
2.  Sous **Paramètres de nom d'ordinateur, de domaine et de groupe de travail**, cliquez sur **Modifier les paramètres**.  
  
3.  Sous l’onglet nom de l' **ordinateur** , cliquez sur **modifier**.  
  
4.  Dans la boîte de dialogue **modification du nom ou du domaine** de l’ordinateur, sélectionnez **domaine**, entrez le nom du domaine, cliquez sur **OK**, puis suivez les étapes de l’Assistant pour terminer le processus.  
  
5.  Après le redémarrage de l’ordinateur, ouvrez une session en tant qu’administrateur et attendez que le gestionnaire MultiPoint s’ouvre.  
  
> [!IMPORTANT]  
> Pour garantir le bon fonctionnement du déploiement de votre domaine MultiPoint services, vous devez configurer deux stratégies de groupe et mettre à jour le registre. Pour plus d’informations, consultez [configurer des stratégies de groupe pour un déploiement de domaine](https://technet.microsoft.com/library/dn265982.aspx).  