---
ms.assetid: cc2834ec-8f66-4209-aba3-402d710cd1bd
title: Emplacement des contrôleurs de domaine
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 6b66b224278f15b6abeecbef8fe0778a98159bb7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872620"
---
# <a name="domain-controller-location"></a>Emplacement des contrôleurs de domaine

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Les clients utilisent le système DNS (Domain Name) pour localiser les contrôleurs de domaine pour accomplir des opérations telles que le traitement des demandes d’ouverture de session ou la recherche dans l’annuaire pour les ressources publiées. Contrôleurs de domaine enregistrent plusieurs enregistrements dans DNS pour aider les clients et autres ordinateurs pour les localiser. Ces enregistrements sont collectivement appelés les enregistrements de localisateur.  
  
Contrôleurs de domaine utilisent également DNS pour localiser les autres contrôleurs de domaine et effectuer des tâches telles que la réplication. Le processus par lequel les contrôleurs de domaine localiser d’autres contrôleurs de domaine est le même que le processus par lequel les clients rechercher les contrôleurs de domaine.  
  


