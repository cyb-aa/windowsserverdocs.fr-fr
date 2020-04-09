---
ms.assetid: 4163cf03-3bff-426c-9844-4cc2d7897d52
title: Affectation du DNS pour le rôle de propriétaire des services AD DS
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 8ef63fd9e53d20c812ff84e601e73d4276b304be
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80825262"
---
# <a name="assigning-the-dns-for-ad-ds-owner-role"></a>Affectation du DNS pour le rôle de propriétaire des services AD DS

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Le propriétaire de la forêt affecte un DNS (Domain Name System) pour le propriétaire Active Directory Domain Services (AD DS) de la forêt. Le DNS de AD DS propriétaire de la forêt est une personne (ou un groupe de personnes) qui est responsable de la surveillance du déploiement du DNS pour l’infrastructure AD DS et pour s’assurer que les noms de domaine (si nécessaire) sont inscrits auprès des autorités Internet appropriées.  
  
Le DNS pour AD DS propriétaire est responsable du DNS pour la conception des AD DS pour la forêt. Si votre organisation exécute actuellement un service serveur DNS, le concepteur DNS pour le service serveur DNS existant utilise le DNS pour AD DS propriétaire pour déléguer le nom DNS racine de la forêt aux serveurs DNS exécutés sur les contrôleurs de domaine.  
  
Le DNS pour AD DS propriétaire de la forêt gère également le contact avec le groupe DHCP (Dynamic Host Configuration Protocol) et le groupe DNS de l’organisation, et coordonne les plans des propriétaires DNS individuels de chaque domaine de la forêt (le cas échéant) avec ces groupes. Le propriétaire DNS de la forêt veille à ce que les groupes DHCP et DNS soient impliqués dans le processus de conception du DNS pour AD DS, de sorte que chaque groupe ait connaissance du plan de conception DNS et puisse fournir des entrées tôt.  
  


