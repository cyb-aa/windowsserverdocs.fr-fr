---
title: Les disques durs virtuels avec fichiers de pagination doivent être exclus de la réplication
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: c0be8a5f-64a1-488a-944e-bb913bb90517
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 94e03cf9de3991d003fad9019b9af33fad2f6bae
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855022"
---
# <a name="virtual-hard-disks-with-paging-files-should-be-excluded-from-replication"></a>Les disques durs virtuels avec fichiers de pagination doivent être exclus de la réplication

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Information|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Les fichiers de pagination doivent être exclus de la participation à la réplication, mais aucun disque n’a été exclu.*  
  
## <a name="impact"></a>Impact  
*Les fichiers de pagination subissent un volume élevé d’activités d’e/s, ce qui nécessitera inutilement des ressources beaucoup plus importantes pour participer à la réplication. Cela a un impact sur les machines virtuelles suivantes :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Si vous ne l’avez pas encore fait, créez un disque dur virtuel distinct pour le fichier de pagination Windows. Si la réplication initiale est déjà terminée, utilisez le Gestionnaire Hyper-V pour supprimer la réplication. Ensuite, configurez à nouveau la réplication et excluez le disque dur virtuel avec le fichier d’échange de la réplication.*  
  


