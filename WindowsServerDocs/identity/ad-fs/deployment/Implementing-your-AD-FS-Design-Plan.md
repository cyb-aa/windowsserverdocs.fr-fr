---
ms.assetid: d04dd17e-a843-46fd-8711-0039918f92d9
title: Implémentation de votre plan de conception AD FS
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ee822ef94e2723a4ce20e456a5507f572f038a93
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855412"
---
# <a name="implementing-your-ad-fs-design-plan"></a>Implémentation de votre plan de conception AD FS

Les conditions et exigences environnementales suivantes sont des facteurs importants dans l’implémentation de votre Services ADFS \(AD FS plan de conception de\) :  
  
-   **Partenaires pris en charge :** En général, vous utilisez AD FS pour travailler avec des organisations partenaires. Pour établir la Fédération des identités, déterminez les organisations avec lesquelles vous souhaitez former un partenariat. Après la mise en place d’un déploiement de AD FS de référence, l’utilisation de partenaires implique l’ajout de partenaires, la suppression de partenaires et la mise à jour des informations de partenaire. Les modifications apportées aux partenariats peuvent se produire pour diverses raisons. Par exemple, votre déploiement AD FS peut nécessiter des mises à jour de partenariat si votre partenaire change de manière significative son entreprise, si votre organisation fait partie d’une plus grande organisation ou d’une Fédération d’organisations, ou si votre organisation est acquise par une autre société. Dans tout scénario dans lequel vous fédérer des identités à partir de plusieurs domaines, vous devez connaître les domaines \(partenaires\) que vous prenez actuellement en charge et tous les domaines supplémentaires qui représentent des partenaires potentiels.  
  
-   **Types d’applications et de services pris en charge :** Certaines applications et certains services requièrent l’accès aux ressources du système d’exploitation, tandis que d’autres sont « compatibles avec les revendications ». Il est important de comprendre les types d’applications et de services pris en charge par AD FS afin que vous puissiez formuler des exigences d’administration.  
  
-   **Diagrammes d’architecture logique et physique ou topologie de déploiement :** Vous devez connaître les éléments suivants :  
  
    -   Si les serveurs de Fédération fonctionnent dans un ensemble de serveurs de batterie ou sur un serveur unique.  
  
    -   Où votre réseau déploie des pare-feu et des proxys.  
  
    -   L’emplacement des ressources et si les utilisateurs accèdent aux ressources de votre organisation, en dehors de l’organisation, ou les deux.  
  
## <a name="how-to-implement-your-ad-fs-design-using-this-guide"></a>Comment implémenter votre conception de AD FS à l’aide de ce guide  
L’étape suivante de l’implémentation de votre conception consiste à déterminer dans quel ordre chaque tâche de déploiement doit être exécutée. Ce guide utilise les listes de vérification pour vous guider à travers les différentes tâches de déploiement du serveur et de l’application requises pour l’implémentation de votre plan de conception. Les listes de contrôle parent et enfant sont utilisées si nécessaire pour représenter l’ordre dans lequel les tâches pour une conception de AD FS spécifique doivent être traitées.  
  
Utilisez les listes de contrôle parentes suivantes de cette section du guide pour vous familiariser avec les tâches de déploiement permettant d’implémenter la conception de AD FS préférée de votre organisation :  
  
-   [Liste de vérification : implémentation d’une conception SSO de Web](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [Liste de vérification : implémentation d’une conception SSO de Web fédéré](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
