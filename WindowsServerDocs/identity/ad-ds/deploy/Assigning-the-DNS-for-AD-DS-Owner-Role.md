---
ms.assetid: 4163cf03-3bff-426c-9844-4cc2d7897d52
title: "Affectation du DNS pour le rôle de propriétaire du service d’annuaire ActiveDirectory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e393cbf32aa5a13ff22044eabb8c575508baaf79
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="assigning-the-dns-for-ad-ds-owner-role"></a>Affectation du DNS pour le rôle de propriétaire du service d’annuaire ActiveDirectory

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Le propriétaire de la forêt affecte un système DNS (Domain Name) pour le propriétaire de Services de domaine ActiveDirectory (ADDS) pour la forêt. Le DNS pour le propriétaire du domaine ActiveDirectory de la forêt est la personne (ou un groupe d’utilisateurs) qui est chargé de surveiller le déploiement du système DNS pour l’infrastructure des services ADDS et de vérifier (si nécessaire) des noms de domaine sont enregistrés avec les autorités Internet appropriées.  
  
Le serveur DNS pour le propriétaire du domaine ActiveDirectory est responsable de DNS pour la conception de domaine ActiveDirectory pour la forêt. Si votre organisation est en cours d’exécution un service de serveur DNS, le concepteur DNS pour le service serveur DNS existant fonctionne avec le DNS pour le propriétaire du domaine ActiveDirectory déléguer le nom DNS racine de forêt pour les serveurs DNS en cours d’exécution sur les contrôleurs de domaine.  
  
Le serveur DNS pour le propriétaire du domaine ActiveDirectory pour la forêt également conserve le contact avec les groupes Dynamic Host Configuration Protocol (DHCP) et le DNS de l’organisation et coordonne les plans des propriétaires DNS individuels de chaque domaine dans la forêt (le cas échéant) à ces groupes. Le propriétaire DNS pour la forêt garantit que les groupes DHCP et DNS sont impliqués dans le DNS pour le processus de conception d’ADDS afin que chaque groupe du plan de conception DNS prend en charge et peut fournir des entrées au début.  
  


