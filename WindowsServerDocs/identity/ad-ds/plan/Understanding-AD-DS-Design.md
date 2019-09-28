---
ms.assetid: d590c90e-9adf-4305-b226-eb2a5743337b
title: Présentation de la conception des services AD DS
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f46cad23e13edcef57bf00113e601069c02cfd4f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402458"
---
# <a name="understanding-ad-ds-design"></a>Présentation de la conception des services AD DS

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Les organisations peuvent utiliser Active Directory Domain Services (AD DS) dans Windows Server pour simplifier la gestion des utilisateurs et des ressources tout en créant des infrastructures évolutives, sécurisées et gérables. Vous pouvez utiliser AD DS pour gérer votre infrastructure réseau, notamment les environnements de succursales, Microsoft Exchange Server et de forêts multiples.  
  
Un projet de déploiement AD DS implique trois phases : une phase de conception, une phase de déploiement et une phase d’opérations. Pendant la phase de conception, l’équipe de conception crée une conception pour la structure logique AD DS qui répond le mieux aux besoins de chaque division de l’organisation qui utilisera le service d’annuaire. Une fois la conception approuvée, l’équipe de déploiement teste la conception dans un environnement Lab, puis implémente la conception dans l’environnement de production. Étant donné que les tests sont effectués par l’équipe de déploiement et qu’elle affecte potentiellement la phase de conception, il s’agit d’une activité intermédiaire qui chevauche à la fois la conception et le déploiement. Une fois le déploiement terminé, l’équipe d’exploitation est responsable de la gestion du service d’annuaire.  
  
Bien que les stratégies de conception et de déploiement de Windows Server AD DS présentées dans ce guide soient basées sur des tests de laboratoire et de programme pilote étendus et une implémentation réussie dans les environnements clients, vous devrez peut-être personnaliser votre conception de AD DS et déploiement pour mieux répondre à des environnements spécifiques et complexes.
  
- Pour plus d’informations sur le déploiement d’AD DS dans un environnement de succursales, consultez le [Guide de planification des succursales de contrôleur de domaine en lecture seule (RODC)](https://go.microsoft.com/fwlink/?LinkId=100207).  
- Pour plus d’informations sur le déploiement d’AD DS dans un environnement Exchange, consultez l’article [exchange 2007-Planning Active Directory](https://go.microsoft.com/fwlink/?LinkId=88904).  
- Pour plus d’informations sur le déploiement de AD DS dans un environnement à plusieurs forêts, consultez l’article considérations sur les [forêts multiples dans windows 2000 et Windows Server 2003](https://go.microsoft.com/fwlink/?LinkId=88905).  
