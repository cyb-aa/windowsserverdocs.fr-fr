---
title: Configurer des machines virtuelles exécutant Windows Vista avec 1 ou 2 processeurs virtuels
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: e562bce3-fd68-42c9-821c-12022ae4746c
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 0319039dfc26d59b1045ffc60f52e56eb900ff76
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862022"
---
# <a name="configure-virtual-machines-running-windows-vista-with-1-or-2-virtual-processors"></a>Configurer des machines virtuelles exécutant Windows Vista avec 1 ou 2 processeurs virtuels

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Configuration|  
|**Catégorie**|Error|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*Une machine virtuelle exécutant Windows Vista est configurée avec plus de 2 processeurs virtuels.*  
  
## <a name="impact"></a>Impact  
  
*Microsoft ne prend pas en charge la configuration des machines virtuelles suivantes :*  
  
\<liste des noms de machine virtuelle >  
  
## <a name="resolution"></a>Résolution  
  
*Arrêtez l’ordinateur virtuel et supprimez un ou plusieurs processeurs virtuels.*  
  
### <a name="to-remove-virtual-processors"></a>Pour supprimer des processeurs virtuels  
  
1.  Ouvrez le Gestionnaire Hyper-V. Cliquez sur **Démarrer**, pointez sur **Outils d'administration**, puis cliquez sur **Gestionnaire Hyper-V**.  
  
2.  Dans le volet de résultats, sous **machines virtuelles**, sélectionnez la machine virtuelle que vous souhaitez configurer. L’état de la machine virtuelle doit être défini sur **désactivé**. Si ce n’est pas le cas, cliquez avec le bouton droit sur l’ordinateur virtuel, puis cliquez sur **arrêter**.  
  
3.  Dans le volet **Action**, sous le nom de l’ordinateur virtuel, cliquez sur **Paramètres**.  
  
4.  Dans le volet de navigation, cliquez sur **Processeur**.  
  
5.  Sur la page **processeur** , définissez le nombre de processeurs sur **1** ou **2** , puis cliquez sur **OK**.  
  


