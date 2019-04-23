---
ms.assetid: 4163cf03-3bff-426c-9844-4cc2d7897d52
title: Affectation du DNS pour le rôle de propriétaire des services AD DS
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: dde9ed6035b30ba5b5b96b7132d25a1f8a1c0b8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884020"
---
# <a name="assigning-the-dns-for-ad-ds-owner-role"></a>Affectation du DNS pour le rôle de propriétaire des services AD DS

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Le propriétaire de la forêt attribue un système DNS (Domain Name) pour le propriétaire de Services de domaine Active Directory (AD DS) pour la forêt. Le DNS pour le propriétaire de AD DS de la forêt est une personne (ou un groupe de personnes) qui est chargé de superviser le déploiement du système DNS pour l’infrastructure des services AD DS et pour s’assurer que (si nécessaire) les noms de domaine sont enregistrés avec les autorités Internet appropriées.  
  
Le DNS pour le propriétaire de AD DS est chargé pour le DNS pour la conception d’AD DS pour la forêt. Si votre organisation fonctionne actuellement un service de serveur DNS, le concepteur DNS pour le service serveur DNS existant fonctionne avec le DNS pour le propriétaire de AD DS déléguer le nom DNS racine de forêt pour les serveurs DNS exécutés sur les contrôleurs de domaine.  
  
Le DNS pour le propriétaire de AD DS pour la forêt assure le contact avec le groupe de Configuration protocole DHCP (Dynamic Host) et le groupe DNS de l’organisation et coordonne les plans des propriétaires DNS individuels de chaque domaine dans la forêt (le cas échéant) avec ces groupes. Le propriétaire DNS pour la forêt garantit que les groupes DHCP et DNS sont impliqués dans le DNS pour le processus de conception d’AD DS afin que chaque groupe est conscient du plan de conception DNS et peut fournir une entrée au début.  
  


