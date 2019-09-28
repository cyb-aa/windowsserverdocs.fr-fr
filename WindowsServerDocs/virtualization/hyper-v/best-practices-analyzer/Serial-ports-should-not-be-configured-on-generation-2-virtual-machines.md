---
title: Les ports série ne doivent pas être configurés sur des ordinateurs virtuels de 2e génération
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 87061193-dd3f-4398-aa5d-4cee83cadfa3
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8a8c15076921efa0e1e791a18c6a45ea1bf27b0e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364730"
---
# <a name="serial-ports-should-not-be-configured-on-generation-2-virtual-machines"></a>Les ports série ne doivent pas être configurés sur des ordinateurs virtuels de 2e génération

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>**Problème**  
*Un port série est configuré sur un ou plusieurs ordinateurs virtuels de 2e génération.*  
  
## <a name="impact"></a>**Impact**  
*Les performances peuvent être affectées aux machines virtuelles suivantes :*  
  
@no__t 0list de machines virtuelles >  
  
## <a name="resolution"></a>**Résolution**  
*If cette opération est intentionnelle, aucune autre action n’est requise. Sinon, envisagez d’utiliser le Gestionnaire Hyper-V ou Windows PowerShell pour supprimer la chaîne de connexion des ports série de l’ordinateur virtuel.*  
  


