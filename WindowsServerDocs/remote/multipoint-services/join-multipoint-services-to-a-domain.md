---
title: Joindre MultiPoint Services à un domaine (facultatif)
Description: Fournit les étapes permettant de joindre MultiPoint Services à votre domaine
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 623b7c21-dcbb-402e-8b5a-8e434cd225bd
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: af5dd1f16e011161bbcf72c21c21088721ac1243
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827040"
---
# <a name="join-the-multipoint-services-computer-to-a-domain-optional"></a>Joindre l’ordinateur MultiPoint Services à un domaine (facultatif)
Si vous accéderez votre ordinateur MultiPoint Services sur un domaine Active Directory, l’étape suivante consiste à ajouter l’ordinateur au domaine.  
  
> [!IMPORTANT]  
> Vous devez vérifier votre fuseau horaire avant de joindre l’ordinateur à un domaine. Pour obtenir des instructions, consultez [définir la date et heure fuseau horaire](Set-the-date--time--and-time-zone.md).  
   
1.  À partir de l’écran d’**accueil**, ouvrez le **Panneau de configuration**. Cliquez sur **système et sécurité**, puis cliquez sur **système**.  
  
2.  Sous **Paramètres de nom d'ordinateur, de domaine et de groupe de travail**, cliquez sur **Modifier les paramètres**.  
  
3.  Sur le **nom de l’ordinateur** , cliquez sur **modification**.  
  
4.  Dans le **modification du nom ou du domaine d’ordinateur** boîte de dialogue, sélectionnez **domaine**, entrez le nom du domaine, puis cliquez sur **OK**, puis suivez les étapes décrites dans l’Assistant pour terminer le processus.  
  
5.  Une fois l’ordinateur redémarré, connectez-vous en tant qu’administrateur et attendez de gestionnaire MultiPoint pour l’ouvrir.  
  
> [!IMPORTANT]  
> Pour vous assurer que votre déploiement de MultiPoint Services de domaine fonctionne correctement, vous devez configurer deux stratégies de groupe et de mettre à jour le Registre. Pour plus d’informations, consultez [configurer des stratégies de groupe pour un déploiement de domaine](https://technet.microsoft.com/library/dn265982.aspx).  