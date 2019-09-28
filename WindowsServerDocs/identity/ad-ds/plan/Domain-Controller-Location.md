---
ms.assetid: cc2834ec-8f66-4209-aba3-402d710cd1bd
title: Emplacement des contrôleurs de domaine
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 48ea357b952738c63274d194b4a5aa5d4adcb3d8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408843"
---
# <a name="domain-controller-location"></a>Emplacement des contrôleurs de domaine

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Les clients utilisent le système DNS (Domain Name System) pour localiser les contrôleurs de domaine et effectuer des opérations telles que le traitement des demandes de connexion ou la recherche de ressources publiées dans l’annuaire. Les contrôleurs de domaine inscrivent un grand nombre d’enregistrements dans DNS pour aider les clients et les autres ordinateurs à les localiser. Ces enregistrements sont collectivement désignés par le terme « enregistrements de localisateur ».  
  
Les contrôleurs de domaine utilisent également le DNS pour rechercher d’autres contrôleurs de domaine et effectuer des tâches telles que la réplication. Le processus par lequel les contrôleurs de domaine localisent les autres contrôleurs de domaine est le même que le processus par lequel les clients localisent les contrôleurs de domaine.  
  


