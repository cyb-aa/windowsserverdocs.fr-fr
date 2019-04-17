---
ms.assetid: cc2834ec-8f66-4209-aba3-402d710cd1bd
title: "Emplacement de contrôleur de domaine"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 198453099336954ee44447a79fec267266caa99a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="domain-controller-location"></a>Emplacement de contrôleur de domaine

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Les clients utiliseront le système DNS (Domain Name) pour rechercher les contrôleurs de domaine pour accomplir des opérations telles que le traitement des demandes d’ouverture de session ou la recherche dans l’annuaire pour les ressources publiées. Contrôleurs de domaine inscrivent une variété d’enregistrements dans DNS pour aider les clients et autres ordinateurs les localiser. Ces enregistrements sont collectivement appelés les enregistrements de localisateur.  
  
Contrôleurs de domaine utilisent également DNS pour localiser d’autres contrôleurs de domaine et effectuer des tâches telles que la réplication. Le processus par lequel les contrôleurs de domaine localiser d’autres contrôleurs de domaine est le même que le processus par lequel les clients rechercher les contrôleurs de domaine.  
  


