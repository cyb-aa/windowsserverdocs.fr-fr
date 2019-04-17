---
title: "Récupération de la forêt ActiveDirectory - nettoyage"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: 2f652d08304a17ecbfde51bbb6f35e4666cd9eca
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---cleanup"></a>Récupération de la forêt ActiveDirectory - nettoyage 

>S’applique à: Windows Server2016, Windows Server2012 et 2012R2, Windows Server2008 et 2008R2

 Effectuez les étapes de récupération suivantes post en fonction des besoins:  
  
-   Après la récupération de la forêt, vous pouvez revenir à la configuration DNS d’origine, y compris la configuration des serveurs DNS préférés et auxiliaire sur chacun des contrôleurs de domaine. Une fois que les serveurs DNS sont configurés comme ils étaient avant la défaillance, leurs possibilités de résolution de nom précédent seront restaurées. Supprimer tous les enregistrements DNS pour les contrôleurs de domaine qui n’ont pas été récupérés.  
  
-   Supprimer des enregistrements WINS WindowsInternetNameService () pour tous les contrôleurs de domaine qui n’ont pas été récupérés.  
  
-   Vous pouvez transférer les rôles de maître d’opérations à d’autres contrôleurs de domaine dans la forêt ou du domaine et ajouter des serveurs de catalogue global en fonction de la configuration avant la défaillance.  
  
-   Étant donné que l’ensemble de la forêt est restauré à un état antérieur, tous les objets (tels que les utilisateurs et ordinateurs) qui ont été ajoutées et toutes les mises à jour (par exemple, les modifications de mot de passe) qui ont été apportées aux objets existants à ce stade sont perdues. Par conséquent, vous devez recréer ces objets manquants et réappliquer les mises à jour manquantes appropriées.  
  
-   Vous devrez peut-être également restaurer les approbations sortantes avec des domaines externes et des forêts, étant donné que ces relations d’approbation externes ne sont pas automatiquement restaurées à partir de sauvegardes.

## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de forêt ActiveDirectory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt ActiveDirectory - procédures](AD-Forest-Recovery-Procedures.md)  



