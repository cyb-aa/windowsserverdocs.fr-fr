---
title: L’appartenance au domaine est recommandé pour les serveurs exécutant Hyper-V
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2f4578e5-0848-46b4-a50b-7dbd480b80bf
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e9db1d28cfe1ae4afd6c5dc1a93253c83fc42113
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860900"
---
# <a name="domain-membership-is-recommended-for-servers-running-hyper-v"></a>L’appartenance au domaine est recommandé pour les serveurs exécutant Hyper-V

>S'applique à : Windows Server 2016


  
*Pour plus d’informations sur les meilleures pratiques et les analyses, consultez* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*Ce serveur est membre d’un groupe de travail.*  
  
## <a name="impact"></a>Impact  
  
*Il n’existe aucune gestion centrale pour ce serveur.*  
  
Joindre cet ordinateur au domaine permet une gestion centralisée via des stratégies pour l’identité, la sécurité et l’audit.  
  
## <a name="resolution"></a>Résolution  
  
*Si vous avez un environnement de domaine disponible, joindre ce serveur à ce domaine.*  
  
> [!IMPORTANT]  
> Nous vous recommandons de consulter les charges de travail en cours d’exécution sur les ordinateurs virtuels sur cet ordinateur pour déterminer s’il y a des implications en matière de sécurité visant à joindre cet ordinateur à un domaine. Si toutes les machines virtuelles sont des contrôleurs de domaine virtualisés, consultez [Planning Considerations for contrôleurs de domaine virtualisés](https://go.microsoft.com/fwlink/?LinkId=190192) (https://go.microsoft.com/fwlink/?LinkId=190192).  
  
Joindre un ordinateur à un domaine nécessite des autorisations sur l’ordinateur et le domaine :   
- Sur l’ordinateur, vous aurez besoin d’un compte d’utilisateur qui est membre du groupe Administrateurs. Ouvrez une session avec ce type de compte, soit fournir le nom d’utilisateur et le mot de passe pour le compte lorsque vous y êtes invité.   
- Sur le domaine, vous devez disposer d’un compte d’utilisateur qui a autorisé à joindre l’ordinateur au domaine. Vous êtes invité pour le nom d’utilisateur et le mot de passe.  
  
Pour obtenir des instructions, consultez [joindre l’ordinateur au domaine](https://go.microsoft.com/fwlink/?LinkId=190193) (https://go.microsoft.com/fwlink/?LinkId=190193).  
  


