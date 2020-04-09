---
title: L’appartenance au domaine est recommandée pour les serveurs exécutant Hyper-V
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2f4578e5-0848-46b4-a50b-7dbd480b80bf
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: de38374a127a15c2a1d4bf262b72781429fa477c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861992"
---
# <a name="domain-membership-is-recommended-for-servers-running-hyper-v"></a>L’appartenance au domaine est recommandée pour les serveurs exécutant Hyper-V

>S’applique à Windows Server 2016


  
*Pour plus d’informations sur les meilleures pratiques et les analyses, consultez* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*Ce serveur est membre d’un groupe de travail.*  
  
## <a name="impact"></a>Impact  
  
*Il n’existe pas de gestion centralisée pour ce serveur.*  
  
La jonction de cet ordinateur au domaine permet une gestion centralisée par le biais de stratégies d’identité, de sécurité et d’audit.  
  
## <a name="resolution"></a>Résolution  
  
*Si un environnement de domaine est disponible, joignez ce serveur à ce domaine.*  
  
> [!IMPORTANT]  
> Nous vous recommandons de passer en revue les charges de travail en cours d’exécution sur les ordinateurs virtuels sur cet ordinateur pour déterminer s’il existe des implications en matière de sécurité lors de la jonction de cet ordinateur à un domaine. Si des ordinateurs virtuels sont des contrôleurs de domaine virtualisés, voir [considérations relatives à la planification des contrôleurs de domaine virtualisés](https://go.microsoft.com/fwlink/?LinkId=190192) (https://go.microsoft.com/fwlink/?LinkId=190192).  
  
Pour joindre un ordinateur à un domaine, vous devez disposer d’autorisations sur l’ordinateur et le domaine :   
- Sur l’ordinateur, vous aurez besoin d’un compte d’utilisateur membre du groupe administrateurs. Connectez-vous avec ce type de compte, ou fournissez le nom d’utilisateur et le mot de passe du compte lorsque vous y êtes invité.   
- Sur le domaine, vous aurez besoin d’un compte d’utilisateur autorisé à joindre l’ordinateur au domaine. Vous êtes invité à entrer le nom d’utilisateur et le mot de passe.  
  
Pour obtenir des instructions, consultez [joindre l’ordinateur au domaine](https://go.microsoft.com/fwlink/?LinkId=190193) (https://go.microsoft.com/fwlink/?LinkId=190193).  
  


