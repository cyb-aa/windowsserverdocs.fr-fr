---
ms.assetid: d590c90e-9adf-4305-b226-eb2a5743337b
title: Présentation de la conception des services AD DS
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 734d5eaef97b23b774eb286134d07a17dc380da1
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81623907"
---
# <a name="understanding-ad-ds-design"></a>Présentation de la conception des services AD DS

> S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Les organisations peuvent utiliser Active Directory Domain Services (AD DS) dans Windows Server pour simplifier la gestion des utilisateurs et des ressources tout en créant des infrastructures évolutives, sécurisées et gérables. Vous pouvez utiliser AD DS pour gérer votre infrastructure réseau, notamment les environnements de succursales, Microsoft Exchange Server et de forêts multiples.

Un projet de déploiement AD DS implique trois phases : une phase de conception, une phase de déploiement et une phase d’opérations. Pendant la phase de conception, l’équipe de conception crée une conception pour la structure logique AD DS qui répond le mieux aux besoins de chaque division de l’organisation qui utilisera le service d’annuaire. Une fois la conception approuvée, l’équipe de déploiement teste la conception dans un environnement Lab, puis implémente la conception dans l’environnement de production. Étant donné que les tests sont effectués par l’équipe de déploiement et qu’elle affecte potentiellement la phase de conception, il s’agit d’une activité intermédiaire qui chevauche à la fois la conception et le déploiement. Une fois le déploiement terminé, l’équipe d’exploitation est responsable de la gestion du service d’annuaire.

Bien que les stratégies de conception et de déploiement de Windows Server AD DS présentées dans ce guide soient basées sur des tests de laboratoire et de programme pilote étendus et une implémentation réussie dans les environnements clients, vous devrez peut-être personnaliser votre conception et votre déploiement AD DS pour mieux répondre à des environnements spécifiques et complexes.

- Pour plus d’informations sur le déploiement d’AD DS dans un environnement de succursales, consultez le [Guide de planification des succursales de contrôleur de domaine en lecture seule (RODC)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd734758(v=ws.10)).
- Pour plus d’informations sur le déploiement d’AD DS dans un environnement Exchange, consultez l’article [Active Directory dans les organisations Exchange Server](https://docs.microsoft.com/Exchange/plan-and-deploy/active-directory/active-directory).
- Pour plus d’informations sur le déploiement de AD DS dans un environnement à plusieurs forêts, consultez l’article considérations sur les [forêts multiples dans windows 2000 et Windows Server 2003](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc739395(v=ws.10)).
