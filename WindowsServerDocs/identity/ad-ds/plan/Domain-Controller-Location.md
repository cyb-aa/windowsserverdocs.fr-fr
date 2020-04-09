---
ms.assetid: cc2834ec-8f66-4209-aba3-402d710cd1bd
title: Emplacement des contrôleurs de domaine
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 2b76f3fcad72c875a83f00e6ade37cfeb5675bf9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822522"
---
# <a name="domain-controller-location"></a>Emplacement des contrôleurs de domaine

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Les clients utilisent le système DNS (Domain Name System) pour localiser les contrôleurs de domaine et effectuer des opérations telles que le traitement des demandes de connexion ou la recherche de ressources publiées dans l’annuaire. Les contrôleurs de domaine inscrivent un grand nombre d’enregistrements dans DNS pour aider les clients et les autres ordinateurs à les localiser. Ces enregistrements sont collectivement désignés par le terme « enregistrements de localisateur ».  
  
Les contrôleurs de domaine utilisent également le DNS pour rechercher d’autres contrôleurs de domaine et effectuer des tâches telles que la réplication. Le processus par lequel les contrôleurs de domaine localisent les autres contrôleurs de domaine est le même que le processus par lequel les clients localisent les contrôleurs de domaine.  
  


