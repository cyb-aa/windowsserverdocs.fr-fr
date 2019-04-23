---
ms.assetid: d04dd17e-a843-46fd-8711-0039918f92d9
title: Implémentation de votre plan de conception AD FS
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 31c2e048c3c125d0ea60610b049501151d7aa823
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830780"
---
# <a name="implementing-your-ad-fs-design-plan"></a>Implémentation de votre plan de conception AD FS

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Les conditions environnementales suivantes et les exigences sont des facteurs importants dans l’implémentation de votre Active Directory Federation Services \(AD FS\) plan de conception :  
  
-   **Partenaires pris en charge :** Généralement, vous utilisez AD FS pour travailler avec des organisations partenaires. Pour établir la fédération des identités, déterminez les organisations avec laquelle vous souhaitez forment un partenariat. Après qu’un déploiement de FS base AD est en place, le fonctionnement avec des partenaires implique l’ajout de partenaires, la suppression des partenaires et la mise à jour des informations sur le partenaire. Modifications apportées aux partenariats peuvent se produire pour diverses raisons. Par exemple, votre déploiement AD FS peut nécessiter des mises à jour de partenariat si votre partenaire change ses activités de façon significative, votre organisation devient partie intégrante d’une plus grande organisation ou d’une fédération des organisations, ou votre organisation est acquis par un autre entreprise. Dans tout scénario dans lequel vous fédérer des identités à partir de plusieurs domaines, vous devez connaître les domaines \(partenaires\) qui vous sont actuellement en charge et tous les domaines supplémentaires qui représentent des partenaires potentiels.  
  
-   **Applications prises en charge et les types de service :** Certains services et applications requièrent l’accès aux ressources du système d’exploitation, tandis que d’autres sont des « revendications prenant en charge. » Il est important de comprendre les types d’applications et services prenant en charge AD FS afin que vous pouvez formuler les besoins d’administration.  
  
-   **Diagrammes d’architecture logiques et physiques ou la topologie de déploiement :** Vous devez connaître :  
  
    -   Indique si les serveurs de fédération fonctionnera dans un ensemble de serveurs de batterie de serveurs ou sur un serveur unique.  
  
    -   Où votre réseau déploie les pare-feux et proxys.  
  
    -   L’emplacement des ressources et indique si les utilisateurs accèdent à des ressources à partir de votre organisation, à l’extérieur de l’organisation, ou les deux.  
  
## <a name="how-to-implement-your-ad-fs-design-using-this-guide"></a>Comment implémenter votre conception AD FS à l’aide de ce guide  
L’étape suivante de l’implémentation de votre conception consiste à déterminer l’ordre dans lequel chaque tâche de déploiement doit être exécutée. Ce guide utilise les listes de vérification pour vous guider à travers les différentes tâches de déploiement du serveur et de l’application requises pour l’implémentation de votre plan de conception. Listes de contrôle parent et enfant sont utilisés si nécessaire pour représenter l’ordre dans lequel les tâches pour un spécifique AD FS design doit être traité.  
  
Utilisez les listes de contrôle parent suivantes dans cette section du guide pour vous familiariser avec les tâches de déploiement pour l’implémentation de conception AD FS par défaut de votre organisation :  
  
-   [Liste de vérification : Implémentation d’une conception SSO de Web](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [Liste de vérification : Implémentation d’une conception SSO Web fédéré](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
