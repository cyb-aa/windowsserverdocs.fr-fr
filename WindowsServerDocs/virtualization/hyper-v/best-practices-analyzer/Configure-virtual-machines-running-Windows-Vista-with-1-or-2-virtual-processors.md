---
title: Configurer des machines virtuelles exécutant Windows Vista avec 1 ou 2 processeurs virtuels
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: e562bce3-fd68-42c9-821c-12022ae4746c
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: fae7d5e437fd83b9c00afcceaaf0eb7e8a7b909b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812070"
---
# <a name="configure-virtual-machines-running-windows-vista-with-1-or-2-virtual-processors"></a>Configurer des machines virtuelles exécutant Windows Vista avec 1 ou 2 processeurs virtuels

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Configuration|  
|**Catégorie**|Erreur|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*Une machine virtuelle exécutant Windows Vista est configurée avec plus de 2 processeurs virtuels.*  
  
## <a name="impact"></a>Impact  
  
*Microsoft ne prend pas en charge la configuration des machines virtuelles suivantes :*  
  
\<liste des noms de machine virtuelle >  
  
## <a name="resolution"></a>Résolution  
  
*Arrêter la machine virtuelle et supprimer un ou plusieurs processeurs virtuels.*  
  
### <a name="to-remove-virtual-processors"></a>Pour supprimer des processeurs virtuels  
  
1.  Ouvrez le Gestionnaire Hyper-V. Cliquez sur **Démarrer**, pointez sur **Outils d'administration**, puis cliquez sur **Gestionnaire Hyper-V**.  
  
2.  Dans le volet de résultats, sous **Machines virtuelles**, sélectionnez la machine virtuelle que vous souhaitez configurer. L’état de la machine virtuelle doit être répertorié en tant que **hors**. Si elle n’est pas le cas, avec le bouton droit de la machine virtuelle, puis cliquez sur **arrêter**.  
  
3.  Dans le volet **Action**, sous le nom de l'ordinateur virtuel, cliquez sur **Paramètres**.  
  
4.  Dans le volet de navigation, cliquez sur **Processeur**.  
  
5.  Sur le **processeur** , définissez le nombre de processeurs à **1** ou **2** puis cliquez sur **OK**.  
  


