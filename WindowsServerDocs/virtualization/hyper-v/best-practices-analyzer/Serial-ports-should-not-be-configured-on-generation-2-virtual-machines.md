---
title: Les ports série ne doivent pas être configurés sur des ordinateurs virtuels de 2e génération
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 87061193-dd3f-4398-aa5d-4cee83cadfa3
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: d9cfb8db2dc3fdff32544670d9443632c2d2264e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861782"
---
# <a name="serial-ports-should-not-be-configured-on-generation-2-virtual-machines"></a>Les ports série ne doivent pas être configurés sur des ordinateurs virtuels de 2e génération

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>**Problème**  
*Un port série est configuré sur un ou plusieurs ordinateurs virtuels de 2e génération.*  
  
## <a name="impact"></a>**Effet**  
*Les performances peuvent être affectées aux machines virtuelles suivantes :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>**Résolution**  
*Si cela est intentionnel, aucune action supplémentaire n’est requise. Sinon, envisagez d’utiliser le Gestionnaire Hyper-V ou Windows PowerShell pour supprimer la chaîne de connexion des ports série de l’ordinateur virtuel.*  
  


