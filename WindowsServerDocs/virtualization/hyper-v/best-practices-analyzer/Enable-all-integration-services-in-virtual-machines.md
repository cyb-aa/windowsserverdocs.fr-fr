---
title: Activer tous les services d’intégration sur les ordinateurs virtuels
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 16e202ad-3795-40c9-8176-7ca319e56d26
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 1984c3d1d6261756bf83f899985b457681537046
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364891"
---
# <a name="enable-all-integration-services-in-virtual-machines"></a>Activer tous les services d’intégration sur les ordinateurs virtuels

>S’applique à Windows Server 2016

Pour plus d'informations sur les meilleures pratiques et les analyses, consultez [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*Un ou plusieurs services d’intégration sont désactivés ou ne fonctionnent pas sur un ordinateur virtuel.*  
  
## <a name="impact"></a>Impact  
  
*La fonctionnalité de service ou d’intégration peut ne pas fonctionner correctement pour les machines virtuelles suivantes :*  
  
\<liste des noms de machine virtuelle >  
  
## <a name="resolution"></a>Résolution  
  
*Utilisez le composant logiciel enfichable Services ou l’outil de ligne de commande sc config pour vérifier que le service est configuré pour démarrer automatiquement et qu’il n’est pas arrêté.*  
  
#### <a name="to-configure-how-a-service-is-started-using-the-services-snap-in"></a>Pour configurer le mode de démarrage d’un service à l’aide du composant logiciel enfichable Services  
  
1.  Utilisez Services Bureau à distance ou la connexion à un ordinateur virtuel pour vous connecter à la machine virtuelle et ouvrir une session sur le système d’exploitation invité.  
  
2.  Ouvrez le dossier Services. (Cliquez sur **Démarrer**, cliquez dans la zone **Rechercher** , tapez **services. msc**, puis appuyez sur entrée.)  
  
3.  Dans le volet d'informations, cliquez avec le bouton droit sur le service à configurer, puis cliquez sur **Propriétés**.  
  
4.  Sous l’onglet **général** , dans type de **démarrage** , cliquez sur **automatique**.  
  
#### <a name="to-configure-how-a-service-is-started-using-sc-config"></a>Pour configurer le démarrage d’un service à l’aide de SC config  
  
1.  Ouvrez Windows PowerShell. (À partir du bureau, cliquez sur **Démarrer** et commencez à taper **Windows PowerShell**.)  
  
2.  Cliquez avec le bouton droit sur **Windows PowerShell** , puis cliquez sur **exécuter en tant qu’administrateur**.  
  
3.  Remplacez < service-name > par le nom du service, puis tapez :  
  
    ```  
    sc config <service-name> start=auto  
    ```  
  


