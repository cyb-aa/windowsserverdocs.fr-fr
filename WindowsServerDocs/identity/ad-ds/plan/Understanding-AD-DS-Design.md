---
ms.assetid: d590c90e-9adf-4305-b226-eb2a5743337b
title: Présentation de la conception des services AD DS
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c94f6ddd19e3178243545b0cc71f6f4c7bb4dbec
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828790"
---
# <a name="understanding-ad-ds-design"></a>Présentation de la conception des services AD DS

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Les organisations peuvent utiliser les Services de domaine Active Directory (AD DS) dans Windows Server pour simplifier la gestion des utilisateurs et des ressources lors de la création d’infrastructures évolutives, sécurisées et gérables. Vous pouvez utiliser les services AD DS pour gérer votre infrastructure réseau, y compris le siège social, Microsoft Exchange Server et environnements de forêts multiples.  
  
Un projet de déploiement d’AD DS implique trois phases : une phase de conception, une phase de déploiement et une phase d’exploitation. Pendant la phase de conception, l’équipe de conception crée une conception pour la structure logique AD DS qui répond le mieux aux besoins de chaque division de l’organisation qui utilisera le service d’annuaire. Une fois que la conception est approuvée, l’équipe de déploiement teste la conception dans un environnement lab et puis implémente la conception dans l’environnement de production. Étant donné que le test est effectué par l’équipe de déploiement, et il peut affecte la phase de conception, il est une activité intermédiaire qui chevauche la conception et déploiement. Lorsque le déploiement est terminé, l’équipe d’exploitation est responsable de la maintenance du service d’annuaire.  
  
Bien que les stratégies de conception et de déploiement de Windows Server AD DS qui sont présentées dans ce guide sont basées sur le test de grande envergure et de programmes et de mise en œuvre réussie dans les environnements client, vous devrez peut-être personnaliser votre conception d’AD DS et déploiement mieux adaptées à des environnements spécifiques et complexes.
  
- Pour plus d’informations sur le déploiement des services AD DS dans un environnement de succursales, consultez le [Guide de planification de contrôleur de domaine en lecture seule (RODC) Branch Office](https://go.microsoft.com/fwlink/?LinkId=100207).  
- Pour plus d’informations sur le déploiement des services AD DS dans un environnement Exchange, consultez l’article [Exchange 2007 - planification d’Active Directory](https://go.microsoft.com/fwlink/?LinkId=88904).  
- Pour plus d’informations sur le déploiement des services AD DS dans un environnement à plusieurs forêts, consultez l’article [plusieurs considérations relatives à la forêt dans Windows 2000 et Windows Server 2003](https://go.microsoft.com/fwlink/?LinkId=88905).  
