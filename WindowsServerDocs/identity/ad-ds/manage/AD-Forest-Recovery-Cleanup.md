---
title: Récupération de la forêt Active Directory-nettoyage
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 205c71930c14ef42596b0e08c27abae6646e4ddf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80824152"
---
# <a name="ad-forest-recovery---cleanup"></a>Récupération de la forêt Active Directory-nettoyage

>S’applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

 Effectuez les étapes suivantes de la récupération après récupération en fonction des besoins :  
  
- Une fois l’ensemble de la forêt récupéré, vous pouvez revenir à la configuration DNS d’origine, y compris la configuration des serveurs DNS préférés et secondaires sur chacun des contrôleurs de domaine. Une fois que les serveurs DNS ont été configurés comme ils l’étaient avant le bon fonctionnement, les fonctionnalités de résolution de noms précédentes seront restaurées. Supprimez tous les enregistrements DNS pour les contrôleurs de domaine qui n’ont pas été récupérés.  
- Supprimez les enregistrements WINS (Windows Internet Name Service) pour tous les contrôleurs de réseau qui n’ont pas été récupérés.  
- Vous pouvez transférer les rôles de maître d’opérations vers d’autres contrôleurs de domaine du domaine ou de la forêt et ajouter d’autres serveurs de catalogue global en fonction de la configuration avant la défaillance.  
- Étant donné que la forêt entière est restaurée à un état précédent, tous les objets (tels que les utilisateurs et les ordinateurs) qui ont été ajoutés et toutes les mises à jour (telles que les modifications de mot de passe) qui ont été apportées aux objets existants après ce point sont perdus. Par conséquent, vous devez recréer ces objets manquants et réappliquer les mises à jour manquantes, le cas échéant.  
- Vous devrez peut-être également restaurer les approbations sortantes avec des domaines et des forêts externes, car ces relations d’approbation externe ne sont pas restaurées automatiquement à partir des sauvegardes.

## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt Active Directory : procédures](AD-Forest-Recovery-Procedures.md)  
