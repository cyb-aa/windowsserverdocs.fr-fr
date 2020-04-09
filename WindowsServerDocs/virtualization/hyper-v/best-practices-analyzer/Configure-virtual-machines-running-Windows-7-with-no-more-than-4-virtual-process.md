---
title: Configurer des machines virtuelles exécutant Windows 7 sans plus de 4 processeurs virtuels
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8fcf0868-b543-4f94-aee7-35324346da55
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: da58833b33f25a8ce724dcbe46161ae99c8d1224
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862032"
---
# <a name="configure-virtual-machines-running-windows-7-with-no-more-than-4-virtual-processors"></a>Configurer des machines virtuelles exécutant Windows 7 sans plus de 4 processeurs virtuels

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Error|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>**Problème**  
*Une machine virtuelle exécutant Windows 7 est configurée avec plus de 4 processeurs virtuels.*  
  
## <a name="impact"></a>**Effet**  
*Microsoft ne prend pas en charge la configuration des machines virtuelles suivantes :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>**Résolution**  
*Arrêtez l’ordinateur virtuel et supprimez un ou plusieurs processeurs virtuels.*  
  
#### <a name="to-remove-virtual-processors"></a>Pour supprimer des processeurs virtuels  
  
1.  Ouvrez le Gestionnaire Hyper-V. Cliquez sur **Démarrer**, pointez sur **Outils d'administration**, puis cliquez sur **Gestionnaire Hyper-V**.  
  
2.  Dans le volet de résultats, sous **machines virtuelles**, sélectionnez la machine virtuelle que vous souhaitez configurer. L’état de la machine virtuelle doit être défini sur **désactivé**. Si ce n’est pas le cas, cliquez avec le bouton droit sur l’ordinateur virtuel, puis cliquez sur **arrêter**.  
  
3.  Dans le volet **Action**, sous le nom de l’ordinateur virtuel, cliquez sur **Paramètres**.  
  
4.  Dans le volet de navigation, cliquez sur **Processeur**.  
  
5.  Sur la page **processeur** , définissez le nombre de processeurs sur une valeur supérieure ou égale à **3** , puis cliquez sur **OK**.  
  


