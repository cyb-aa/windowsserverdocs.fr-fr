---
title: Vérifier que toutes les extensions de commutateur virtuel obligatoires sont disponibles
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2f2f2698-f5ec-4cad-aa64-d6987e8142a1
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 14c9fc31521a7d4f5e0eed821c0cc49dcfe74e69
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861932"
---
# <a name="ensure-that-all-mandatory-virtual-switch-extensions-are-available"></a>Vérifier que toutes les extensions de commutateur virtuel obligatoires sont disponibles

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Une ou plusieurs cartes réseau virtuelles sont connectées à un commutateur virtuel avec des extensions obligatoires qui sont désactivées ou ne sont pas installées.*  
  
## <a name="impact"></a>Impact  
*Le trafic réseau est bloqué sur une ou plusieurs cartes réseau virtuelles sur les machines virtuelles suivantes :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Tout d’abord, assurez-vous que l’extension obligatoire a été installée sur l’hôte et installez l’extension si nécessaire. Ensuite, si l’extension obligatoire est désactivée, utilisez le gestionnaire de commutateur virtuel ou l’applet de commande Windows PowerShell Enable-VMSwitchExtension pour activer l’extension.*  
  


