---
ms.assetid: d04dd17e-a843-46fd-8711-0039918f92d9
title: "Implémentation d’ADFS Plan de conception"
description: 
author: billmath
manager: femila
ms.date: 02/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 19cfe06a4dcf11e525d31865673e2f5c522cbb7a
ms.sourcegitcommit: 877a50cd8d6e727048cdfac9b614a98ac3220876
ms.translationtype: HT
ms.contentlocale: fr-FR
---
# <a name="implementing-your-ad-fs-design-plan"></a>Implémentation d’ADFS Plan de conception

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Les conditions suivantes et les conditions requises sont des facteurs importants dans l’implémentation de votre plan de conception d’ActiveDirectory Federation Services \(ADFS\):  
  
-   **Prise en charge de partenaires:** vous utilisez généralement ADFS pour travailler avec des organisations partenaires. Pour établir la fédération des identités, déterminer les organisations avec lequel vous souhaitez former un partenariat. Après qu’un déploiement de FS ActiveDirectory de la ligne de base est en place, fonctionne avec des partenaires implique l’ajout de partenaires, la suppression des partenaires et la mise à jour des informations sur le partenaire. Modifications apportées aux partenariats peuvent se produire pour plusieurs raisons. Par exemple, votre déploiement d’ADFS peut nécessiter des mises à jour de partenariat si votre partenaire change son activité de façon significative, votre organisation fait partie d’une grande entreprise ou d’une fédération d’organisations ou votre organisation a été achetée par une autre société. Dans n’importe quel scénario dans lequel vous fédérez des identités de plusieurs domaines, vous devez connaître le \(partners\) domaines qui vous êtes actuellement en charge et tous les domaines supplémentaires qui représentent des partenaires potentiels.  
  
-   **Prise en charge d’application et les types de service:** certains services et applications requièrent l’accès aux ressources du système d’exploitation, tandis que d’autres sont «revendications prenant en charge». Il est important de comprendre les types d’applications et des services ADFS prend en charge afin que vous pouvez élaborer des exigences de l’administration.  
  
-   **Schémas d’architecture logiques et physiques ou la topologie de déploiement:** vous devez connaître:  
  
    -   Indique si les serveurs de fédération fonctionnera dans un ensemble de serveurs de batterie de serveurs ou sur un seul serveur.  
  
    -   Où votre réseau déploie des pare-feu et proxys.  
  
    -   L’emplacement des ressources et indique si les utilisateurs accèdent à des ressources à partir de votre organisation, en dehors de l’organisation, ou les deux.  
  
## <a name="how-to-implement-your-ad-fs-design-using-this-guide"></a>Comment implémenter votre conception ADFS à l’aide de ce guide  
L’étape suivante de l’implémentation de votre conception consiste à déterminer dans quel ordre chaque tâche de déploiement doit être exécutée. Ce guide utilise les listes de vérification pour vous guider à travers les différentes tâches de déploiement serveur et d’application qui sont requis pour implémenter votre plan de conception. Listes de contrôle parent et enfant sont utilisés comme nécessaire pour représenter l’ordre dans lequel les tâches pour un spécifique ADFS design doit être traité.  
  
Utilisez les listes de contrôle parent suivantes dans cette section du guide pour vous familiariser avec les tâches de déploiement pour l’implémentation de conception ADFS par défaut de votre organisation:  
  
-   [Liste de vérification: Implémentation d’une conception SSO de Web](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [Liste de vérification: Implémentation d’une conception SSO de Web fédéré](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
