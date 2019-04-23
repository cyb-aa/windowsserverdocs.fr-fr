---
title: Récupération de forêt AD - nettoyage
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: fa7193cc800eac0fee6425a66bd5cd82d8c822c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868960"
---
# <a name="ad-forest-recovery---cleanup"></a>Récupération de forêt AD - nettoyage

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

 Effectuez les étapes de post-récupération suivantes en fonction des besoins :  
  
- Une fois la forêt entière est récupérée, vous pouvez revenir à la configuration DNS d’origine, y compris la configuration des serveurs DNS préférés et auxiliaire sur chacun des contrôleurs de domaine. Une fois que les serveurs DNS sont configurés comme ils l’étaient avant la défaillance, leurs fonctionnalités de résolution de nom précédent seront restaurées. Supprimer tous les enregistrements DNS pour les contrôleurs de domaine qui n’ont pas été récupérés.  
- Supprimer les enregistrements de Windows Internet Service WINS (Name) pour tous les contrôleurs de domaine qui n’ont pas été récupérés.  
- Vous pouvez transférer les rôles de maître d’opérations aux autres contrôleurs de domaine dans la forêt ou un domaine et ajouter des serveurs de catalogue global, selon la configuration avant la défaillance.  
- Étant donné que la forêt entière est restaurée à un état antérieur, tous les objets (tels que les utilisateurs et ordinateurs) qui ont été ajoutés et toutes les mises à jour (telles que les modifications de mot de passe) qui ont été apportées aux objets existants après ce point sont perdues. Par conséquent, vous devez recréer ces objets manquants et réappliquez les mises à jour manquantes, comme il convient.  
- Vous devrez peut-être également restaurer des approbations sortantes avec des domaines externes et des forêts, étant donné que ces relations d’approbation externes ne sont pas restaurées automatiquement à partir de sauvegardes.

## <a name="next-steps"></a>Étapes suivantes

- [Guide de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de forêt AD - procédures](AD-Forest-Recovery-Procedures.md)  
