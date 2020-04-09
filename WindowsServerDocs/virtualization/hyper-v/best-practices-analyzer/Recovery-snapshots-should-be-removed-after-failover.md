---
title: Les instantanés de récupération doivent être supprimés après le basculement
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 922115fa-e8dd-4055-aaf1-4a4437c5cf28
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: c995293ca67b4cad0837affa854fb4ac366856e1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861842"
---
# <a name="recovery-snapshots-should-be-removed-after-failover"></a>Les instantanés de récupération doivent être supprimés après le basculement

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016| 
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Opérations|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>**Problème**  
*Une machine virtuelle basculée possède un ou plusieurs instantanés de récupération.*  
  
## <a name="impact"></a>**Effet**  
*L’espace disponible est peut-être insuffisant sur le disque physique qui stocke les fichiers d’instantanés. Si cela se produit, aucune opération de disque supplémentaire ne peut être effectuée sur le stockage physique. Tout ordinateur virtuel qui s’appuie sur le stockage physique peut être affecté. Cela a un impact sur les machines virtuelles suivantes :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>**Résolution**  
*Pour chaque machine virtuelle basculée, utilisez l’applet de commande Complete-VMFailover dans Windows PowerShell pour supprimer les instantanés de récupération et indiquer la fin du basculement.*  
  


