---
title: Mémoire dynamique est activé mais ne répond pas sur des machines virtuelles
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 91b7f50f-a071-4ab6-beb1-1b29f92f52b6
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 95fd426929f3e2f6f01bc10b207a21a57f1d8370
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887780"
---
# <a name="dynamic-memory-is-enabled-but-not-responding-on-some-virtual-machines"></a>Mémoire dynamique est activé mais ne répond pas sur des machines virtuelles

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Un ou plusieurs machines virtuelles rencontre des problèmes avec le pilote requis pour la mémoire dynamique dans le système d’exploitation invité.*  
  
## <a name="impact"></a>Impact  
*Le système d’exploitation invité dans les machines virtuelles suivantes peuvent ne pas fonctionner ou peut s’exécuter unreliably car Hyper-V ne peut pas ajuster la mémoire dynamiquement pour répondre aux modifications de la demande de mémoire. Cela affecte les ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Ce comportement est attendu si le démarrage de la machine virtuelle. Si la machine virtuelle ne démarre pas, assurez-vous que les services d’intégration sont mis à niveau vers la dernière version et que le système d’exploitation invité prend en charge la mémoire dynamique.*  
  
À compter de Windows Server 2016, les services d’intégration sont remis via Windows Update. Vérifiez que les ordinateurs virtuels sont configurés pour recevoir des mises à jour pour obtenir la dernière version des services d’intégration.  
  
Mémoire dynamique fonctionne avec des versions spécifiques des invités pris en charge. Consultez [Hyper-V Dynamic Memory Overview](https://technet.microsoft.com/library/hh831766.aspx) pour les versions antérieures à Windows Server 2016 et Windows 10.  
  


